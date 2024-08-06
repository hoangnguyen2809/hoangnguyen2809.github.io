---
title: "Buffer-Overflow Attack Lab (Server Version)"
date: 2024-07-08
permalink: /posts/2024/07/blog-post-5/
tags:
  - buffer overflow
  - cyber
  - SEED lab
---

SEED Labs – Buffer Overflow Attack Lab (Server Version) done by Hoang Nguyen.

# Buffer Overflow

A buffer overflow occurs when more data is written to a buffer than it can hold, causing the data to overwrite adjacent memory locations. This can lead to various unintended behaviors, including crashes, data corruption, or the execution of arbitrary code.

## Steps in a Buffer Overflow Attack:

1. **Overflowing the Buffer**:

   - An attacker inputs data that exceeds the buffer’s allocated size.
   - For example, if a buffer is allocated 10 bytes of memory but the attacker provides 20 bytes of data, the additional 10 bytes will overflow into adjacent memory.

2. **Overwriting the Return Address**:

   - In the stack memory layout, the buffer might be located just below the return address.
   - As the buffer overflows, it can overwrite the return address stored on the stack.
   - The return address is the location the program will jump to after the current function finishes execution.

3. **Injecting Malicious Code**:

   - Along with the overflowing data, the attacker includes a new return address that points to their malicious code.
   - The malicious code can be injected into the buffer itself or elsewhere in memory.

4. **Executing**:

   - When the function attempts to return, it uses the overwritten return address.
   - The CPU jumps to the new address, which now points to the attacker’s code.
   - The system executes the malicious code with the same privileges as the original program.

# Buffer Overflow Attack Lab (Server Version)

More information on [SEED Buffer Overflow Attack](https://seedsecuritylabs.org/Labs_20.04/Software/Buffer_Overflow_Server/)

### Tools

Programming language: python, C <br>
Tools used in this lab: scapy, Wireshark <br>
Software: VirtualBox

## Task 1: Get Familiar with the Shellcode

- Shellcode is typically used in code injection attacks. It is basically a piece of code that launches a shell, and is usually written in assembly languages

- Modified shellcode to delete _important.txt_:

  - 32-bit version:
    <img src='/images/shellcoderemove32.png'>

  - 64-version:
    <img src='/images/shellcoderemove64.png'>

## The Vulnerable Program Memory layout

<img src='/images/stackfile.png'>
- stack.c reads data from standard input then passes data to another buffer in function bof(). The original input can have a maximum length of 517 bytes, but the buffer in bof() is only 200 bytes long. Buffer overflow will occur.

## Task 2: Level-1 Attack

- After turning off address randomization countermeasure with: <br>
  `$ sudo /sbin/sysctl -w kernel.randomize_va_space=0`

- We send benign message to our target on 10.9.0.5: `echo hello | nc 10.9.0.5 9090`
- Message returned by container:
  <img src='/images/benignreturn.png'>

- Based on the message returned by the container, we can tell that:

  - Distance between $ebp and buffer's starting address is : <br>
    0xffffd488 - 0xffffd418 = 112
  - return address starting from offset 116, ending at offset 120 and located at 0xffffd488+4 <br>
  - the first address that we can jump too will be 0xffffd488+8
    <img src='/images/stackmemorylayout.png'>

- exploit.py file: <br >
  <img src='/images/exploit1file.png'>

  - The shellcode is put at the end of badfile.
  - The size of the shellcode in this 32-bit version is 136 bytes (generate shellcode and check its size using `ls -l`), so we can tell that the shellcode is located within the offset [381-517]
  - We want to replace the original return address that is located at offset[116-120] with our desired return address.
  - Since the first address we can jump to is 0xffffd488+8, any number within range[8-265] will work well in our case. I chose 200.

- As result, we succeed getting root shell on the target server 10.9.0.5:
  <img src='/images/exploit1succeed.png'>

## Task 3: Level-2 Attack

- We send benign message to our target on 10.9.0.6: `echo hello | nc 10.9.0.6 9090`
- Message returned by container:
  <img src='/images/benignreturn2.png'>
- exploit2.py file: <br>
  <img src='/images/exploit2file.png'>

  - The shellcode is still put at the end of badfile.
  - The size of shellcode is still 136 bytes, and lies within the offset [381-517] <br>
    <img src='/images/stack2memorylayout.png'>
  - Using the hint that the range of the buffer size on target server is [100-300] bytes, we can tell that range of NOP will be from [300-381]. I chose the return address as 0xffffd3e8 + 350
  - So we would want to spray the first 320 bytes (extra 20 for overhead) of the badfile with the desired return address so that when overflow the buffer at target server, it will overwrite the original return address.

- As result, we succeed getting root shell on the target server 10.9.0.6:
  <img src='/images/exploit2succeed.png'>

## Task 4: Level-3 Attack

- In this task, we switch to a 64-bit server program
- We send benign message to our target on 10.9.0.7: `echo hello | nc 10.9.0.6 9090`
- Message returned by container:
  <img src='/images/benignreturn3.png'>
- Based on the result returned by the container, we can tell that:

  - Distance between $rbp and buffer's starting address is : <br>
    0x00007fffffffe230 - 0x00007fffffffe160 = 208
  - return address starting from offset 216, ending at offset 224 and located at 0x00007fffffffe230+8 <br>

#### Challenge in 64-bits server program:

- The 64-bit version is more difficult than the 32-bit version, because we have to avoid zeros in the payload.
- When _strcpy()_ copies a string, it will treat zeros as the end of the string. Therefore, avoiding zeros in the payload is necessary to successfully overflow the buffer.
- We overcome this challenge by **realocating the shellcode to the place before the return address.**

- exploit.py file: <br>
  <img src='/images/exploit3file.png'>

  - Based on the distace between $rbp and &buffer, we know that size of buffer is ~208 bytes.
  - The size of the shellcode in this 32-bit version is 165 bytes.
  - We will put the shellcode in the beginning of the buffer, start value can be vary from [0-43]. I chose 8.
    <img src='/images/badfilelayout64.png'>
  - The desired return address will now be the begining of the buffer, so the NOP will take us to the shellcode.

- As result, we succeed getting root shell on the target server 10.9.0.7:
  <img src='/images/exploit3succeed.png'>
