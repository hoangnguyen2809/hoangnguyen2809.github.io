---
title: "Network Security Attacks and Countermeasures"
excerpt: "SEED Lab – TCP/IP Attack Lab done by Hoang Nguyen.<br/><img src='/images/nwdiagram.png'>"
collection: portfolio
---

# Overview

Conducted a series of network security attacks and countermeasures as part of an in-depth lab exercise. These tasks provided hands-on experience with TCP SYN Flooding, TCP RST attacks, and TCP Session Hijacking using tools such as Netwox, Wireshark, and Scapy. This portfolio entry details the process, results, and key learnings from these tasks.

## Task 1: TCP SYN Flooding Attack

Objective: Demonstrate a TCP SYN flooding attack on a telnet server, analyze its impact, and implement SYN cookie countermeasures.

### Steps:

1. **SYN Flood Attack Execution:**

   - Utilized Netwox tool #76 to send numerous SYN packets to the victim machine without completing the TCP handshake.
   - Command used: `netwox 76 -i <target IP> -p <target port>`

2. **Packet Capture:**

   - Employed Wireshark to capture and analyze the traffic generated during the attack.

3. **System Monitoring:**

   - Ran `netstat -tna` on the victim machine before and during the attack to observe changes in the network connections.

4. **SYN Cookie Countermeasure:**
   - Enabled and disabled SYN cookies using `sudo sysctl -w net.ipv4.tcp_syncookies=1` (on) and `sudo sysctl -w net.ipv4.tcp_syncookies=0` (off) to evaluate their effectiveness.

### Results and Analysis:

- **Success Indicators**: High number of half-open connections (SYN_RECV state) indicated a successful attack when SYN cookies were off.
- **Netwox Command**: Successfully initiated attack with `netwox 76 -i <target IP> -p <target port>`
- **Netstat and Wireshark Outputs**: Screenshots showed increased SYN_RECV states without SYN cookies. With SYN cookies on, normal connection states were maintained.
- **Established Connections**: Pre-existing TCP sessions were unaffected during the SYN flood.
- **Effectiveness of SYN Cookies**: The mechanism prevented half-open connections, mitigating the attack impact.

## Task 2: TCP RST Attack

Objective: Terminate active telnet and ssh sessions by injecting TCP RST packets.

### Steps:

1. **Packet Construction with Scapy:**

   - Modified necessary header fields (source IP, destination IP, sequence numbers, and acknowledgment numbers) to craft the RST packets.

2. **Attack Execution:**
   - Injected RST packets to disrupt active telnet and ssh connections.

### Results and Analysis:

- **Header Fields:** Correctly calculated sequence and acknowledgment numbers.
- **Session Disruption:** Successfully terminated telnet sessions. SSH sessions showed resilience due to encryption.
- **Screenshots:** Documented steps and results.
- **LAN Dependency:** Attack efficacy reduced if attacker and victim are not on the same LAN due to difficulty in sequence number prediction and packet injection.

## Task 3: TCP Session Hijacking Attack

Objective: Inject malicious commands into an active telnet session to delete a specific file on the server.

### Steps:

1. **Packet Construction with Scapy:**

   - Modified TCP header fields to synchronize with the active telnet session.
   - Command injected: `rm /home/seed/myfile.txt`

2. **Attack Execution:**
   - Successfully injected packets to delete the target file.

### Results and Analysis:

- **Header Fields:** Sequence and acknowledgment numbers carefully calculated for successful injection.
- **Packet Capture:** Wireshark capture confirmed the injection.
- **SSH Session Hijacking:** Infeasible due to encryption preventing command injection.
- **Observations:** Demonstrated the vulnerability of unencrypted telnet sessions to hijacking.

# Conclusion

These tasks provided a comprehensive understanding of network attack mechanisms and defense strategies. Successfully conducting these attacks and implementing countermeasures showcased my proficiency in network security, use of advanced tools like Netwox, Wireshark, and Scapy, and understanding of TCP/IP protocols. This experience is a testament to my capability to analyze, execute, and mitigate real-world network security threats.

## Supporting Documents:

task2.py: Scapy script for TCP RST attack. <br>
task3.py: Scapy script for TCP session hijacking. <br>
Files are available [Github](https://github.com/hoangnguyen2809/TCP-Attack-Lab) <br>
Report: Detailed instructions, results, and analysis of the tasks at [REPORT](https://hoangnguyen2809.github.io/posts/2012/08/blog-post-1/)
