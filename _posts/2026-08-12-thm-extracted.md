---
title: "TryHackMe - Extracted"
date: 2026-08-12
permalink: /posts/2026/08/thm-extracted/
tags:
  - tryhackme
  - extracted
  - cybersecurity
---

Network forensics writeup covering malicious PowerShell analysis, TCP stream reconstruction, XOR/Base64 decoding, and KeePass memory forensics.


## Overview

This room starts with a single packet capture: `traffic.pcapng`.

The objective was to reconstruct what happened on the compromised host and determine how the attacker obtained access to a KeePass database. The investigation moved from basic protocol triage to malicious PowerShell analysis, raw TCP stream extraction, custom decoding, and finally memory-forensics-assisted password recovery.

The key finding was that the attacker did **not** transmit the KeePass password directly. Instead, the malicious script:

1. Downloaded and executed ProcDump.
2. Created a memory dump of the running KeePass process.
3. Read the KeePass database from disk.
4. XOR-obfuscated both artifacts.
5. Base64-encoded the data.
6. Exfiltrated the two files over separate raw TCP connections.

For portfolio purposes, the recovered credential is intentionally **redacted**.

---

## 1. Initial Traffic Triage

I began by reviewing the protocol distribution in Wireshark:

**Statistics → Protocol Hierarchy**

The capture was dominated by TCP traffic. During an initial investigation, protocols such as HTTP, FTP, and Telnet are useful places to start because they may expose commands, transferred files, or even plaintext credentials.

Filtering for HTTP showed only a very small number of packets, which made the HTTP exchange immediately worth inspecting.

```text
http
```

Following the HTTP stream revealed a response from a Python `SimpleHTTP` server:

```text
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.6.9
Content-type: application/octet-stream
Content-Length: 11195
```

The body contained an obfuscated PowerShell script rather than a normal document or web resource.

---

## 2. Analyzing the PowerShell Payload

After deobfuscating the PowerShell logic, several behaviors stood out.

### ProcDump download

The script first checked for ProcDump and downloaded it from Microsoft Sysinternals if it was not already present:

```powershell
$PRoCDumppATh = 'C:\Tools\procdump.exe'

if (-Not (Test-Path -Path $PRoCDumppATh)) {
    $ProcdUmpDOWNloADURL = 'https://download.sysinternals.com/files/Procdump.zip'
    ...
}
```

### KeePass process discovery

It then searched for a running KeePass process:

```powershell
$KEEPASsPrOCesS = Get-Process -Name 'KeePass'
```

### Memory dump creation

If KeePass was running, ProcDump was used to create a full process dump:

```powershell
$ProcStArtiNFO.Arguments = "-accepteula -ma $($KEEPASsPrOCesS.Id) `"$dUmPFilEpath`""
```

The resulting dump was stored as:

```text
1337.dmp
```

This is important because sensitive KeePass material can remain in process memory while the application is open.

---

## 3. Identifying the Exfiltration Channels

The script revealed two separate artifacts and two TCP destination ports.

| TCP Port | Artifact | XOR Key |
|---|---|---:|
| `1337` | KeePass process memory dump | `0x41` |
| `1338` | KeePass database (`Database1337.kdbx`) | `0x42` |

For the memory dump, the script performs an XOR operation with `0x41`, converts the result to Base64, writes it to disk, and sends it over TCP port `1337`.

For the KeePass database, the same process is repeated using XOR key `0x42` and TCP port `1338`.

A simplified representation of the attacker workflow is:

```text
KeePass process ──> ProcDump ──> XOR 0x41 ──> Base64 ──> TCP/1337

Database1337.kdbx ─────────────> XOR 0x42 ──> Base64 ──> TCP/1338
```

This explained why the password itself was not visible directly in the packet capture: the attacker exfiltrated the **database and memory state**, not a plaintext credential.

---

## 4. Extracting the Raw TCP Streams

The next step was to reconstruct the two exfiltrated payloads from the capture with TShark.

```bash
tshark -r traffic.pcapng -q -z follow,tcp,raw,1 \
  | tail -n +7 | head -n -1 > 1337_hex.txt
```

```bash
tshark -r traffic.pcapng -q -z follow,tcp,raw,2 \
  | tail -n +7 | head -n -1 > 1338_hex.txt
```

The resulting files contain the TCP stream data represented as hexadecimal text.

At this point, the transformation observed in the malicious script had to be reversed:

```text
Hex text
   ↓
Raw bytes
   ↓
Base64 decode
   ↓
XOR with original key
   ↓
Recovered file
```

---

## 5. Reconstructing the Exfiltrated Files

I used a small Python decoder to automate the reverse transformation.

```python
#!/usr/bin/env python3
import base64
import sys


def decode(hex_data, xor_key):
    raw = bytes.fromhex(
        hex_data.replace(":", "")
                .replace(" ", "")
                .replace("\n", "")
    )

    raw += b"=" * (-len(raw) % 4)
    decoded = base64.b64decode(raw)

    return bytes(byte ^ xor_key for byte in decoded)


if len(sys.argv) != 4:
    print(f"Usage: python {sys.argv[0]} <input.hex> <output> <xor_key>")
    sys.exit(1)

input_file = sys.argv[1]
output_file = sys.argv[2]
xor_key = int(sys.argv[3], 0)

with open(input_file, "r") as f:
    hex_data = f.read()

result = decode(hex_data, xor_key)

with open(output_file, "wb") as f:
    f.write(result)

print(f"[+] Decoded {len(result)} bytes")
print(f"[+] Saved to: {output_file}")
print(f"[+] Header: {result[:16].hex()}")

if result.startswith(b"\x03\xd9\xa2\x9a"):
    print("[+] KeePass KDBX detected")
elif result.startswith(b"MDMP"):
    print("[+] Windows minidump detected")
```

### Recover the memory dump

```bash
python decoder.py 1337_hex.txt memdump.dmp 0x41
```

### Recover the KeePass database

```bash
python decoder.py 1338_hex.txt database.kdbx 0x42
```

The file headers provide a useful sanity check:

- `MDMP` indicates a Windows minidump.
- `03 d9 a2 9a` is associated with a KeePass KDBX file header.

---

## 6. KeePass Memory Forensics

With the reconstructed process dump available, I analyzed the KeePass memory artifact using:

```text
matro7sh/keepass-dump-masterkey
```

The tool recovered multiple password candidates from memory, but the **first character was missing** from the reconstructed value.

Rather than brute-forcing the entire password space, the recovered partial password dramatically reduced the search space: only the unknown leading character needed to be tested.

---

## 7. Generating a Focused Wordlist

To generate candidate passwords, I prepended every printable character to the recovered suffix.

```python
#!/usr/bin/env python3
import string

# Recovered suffix intentionally redacted
suffix = "REDACTED"

with open("wordlist.txt", "w") as f:
    for char in string.printable:
        f.write(char + suffix + "\n")

print("Wordlist generated: wordlist.txt")
```

This produces a small, targeted wordlist rather than performing a large generic brute-force attack.

---

## 8. Validating the KeePass Password

The final step was to test the candidate list against the reconstructed KeePass database using:

```text
r3nt0n/keepass4brute
```

Once the correct candidate was identified, the database could be opened successfully in KeePass.

The recovered password and vault contents are intentionally omitted from this public writeup.

---

## Investigation Timeline

```text
traffic.pcapng
     │
     ├── Protocol triage in Wireshark
     │
     ├── Suspicious HTTP response
     │        │
     │        └── Obfuscated PowerShell payload
     │                 │
     │                 ├── ProcDump KeePass memory
     │                 ├── XOR + Base64
     │                 └── Raw TCP exfiltration
     │
     ├── TCP/1337 ──> memory dump ──> XOR 0x41
     │
     └── TCP/1338 ──> KeePass DB ───> XOR 0x42
                         │
                         ├── Reconstruct artifacts
                         ├── Analyze KeePass memory
                         ├── Recover partial password
                         └── Targeted candidate generation
                                  │
                                  └── Open reconstructed vault
```

---

## Key Takeaways

### 1. Network traffic may contain more than credentials

A packet capture does not need to contain a plaintext password to reveal sensitive information. Exfiltrated databases, process dumps, scripts, and encryption material can be equally valuable to an attacker or investigator.

### 2. Malware behavior can reveal the decoding procedure

The PowerShell payload documented its own transformation chain. By identifying the XOR keys, Base64 encoding, destination ports, and source files, the exfiltrated traffic could be reconstructed deterministically.

### 3. Memory artifacts can undermine password protections

The KeePass database remained encrypted on disk, but a memory dump of the running process exposed enough password-related material to significantly reduce the recovery effort.

### 4. File signatures are useful validation points

Checking reconstructed output for expected magic bytes such as `MDMP` or the KeePass KDBX header is a simple way to confirm that decoding steps were applied correctly.

### 5. Targeted recovery beats blind brute force

Once most of the password was recovered from memory, testing only the missing character reduced the problem from a general password-cracking exercise to a tiny candidate set.

---

## Tools Used

| Tool | Purpose |
|---|---|
| **Wireshark** | Protocol triage and stream inspection |
| **TShark** | Raw TCP stream extraction |
| **Python** | Hex/Base64/XOR artifact reconstruction |
| **ProcDump** | Identified in attacker script as the memory dumping utility |
| **keepass-dump-masterkey** | KeePass memory artifact analysis |
| **keepass4brute** | Candidate validation against the recovered KDBX database |
| **KeePass** | Final database validation |

---

## Skills Demonstrated

- PCAP analysis and protocol triage
- HTTP and raw TCP stream reconstruction
- Static analysis of obfuscated PowerShell
- Identification of data-exfiltration logic
- Base64 and XOR decoding
- Python scripting for forensic artifact recovery
- Windows process memory analysis
- KeePass database forensics
- Targeted password candidate generation

---

## Conclusion

This room was a useful end-to-end network forensics exercise because the answer was not available in a single packet or protocol. Solving it required correlating several pieces of evidence:

- the HTTP-delivered PowerShell script,
- its ProcDump behavior,
- the XOR keys,
- two raw TCP exfiltration streams,
- reconstructed memory and database artifacts,
- and finally KeePass memory analysis.

The most valuable lesson was that understanding **how the attacker transformed and transmitted the data** made it possible to reverse the entire workflow and rebuild the original artifacts from network traffic alone.

---

> **Note:** This writeup documents a controlled TryHackMe lab environment for educational and portfolio purposes. Credentials and sensitive challenge answers are intentionally redacted.