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

- We send benign message to our first target on 10.9.0.5: `echo hello | nc 10.9.0.5 9090`
- Message returned by container:
  <img src='/images/benignreturn.png'>

- Based on the message returned by the container, we can tell that:

  - Distance between $ebp and buffer's starting address is : <br>
    0xffffd488 - 0xffffd418 = 112
  - return address starting from offset 116, ending at offset 120 and located at 0xffffd488+4 <br>
  - the first address that we can jump too will be 0xffffd488+8
    <img src='/images/stackmemorylayout.png'>

- exploit.py file:
  <img src='/images/exploit1file.png'>

  - The shellcode is put at the end of badfile.
  - The size of the shellcode in this 32-bit version is 136 bytes, so we can tell that the shellcode is located within the offset [380-516]
  - We want to replace the original return address that is located at offset[116-120] with our desired return address.
  - Since the first address we can jump to is 0xffffd488+8, any number within range[8-264] will work well in our case. I chose 200.

- As result, we succeed getting root shell on the target server:
  <img src='/images/exploit1succeed.png'>

## Task 3: Level-2 Attack
