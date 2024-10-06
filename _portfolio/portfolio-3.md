---
title: "Memory Exploitation and Defense Evasion"
excerpt: "SEED Lab – Buffer Overflow Attack Lab (Server version) done by Hoang Nguyen <br/>"
collection: portfolio
---

# Overview

This lab involved performing a buffer overflow attack, where I explored the vulnerabilities caused by writing excess data into buffers beyond their capacity. I applied this to both 32-bit and 64-bit programs, analyzing the stack memory layout and exploiting the overflow to gain root access.

## Lab Environment

The lab was conducted on a SEED Ubuntu 20.04 virtual machine provided by SEED Labs, where each server program had a buffer overflow vulnerability. This environment simulated real-world scenarios, and tasks progressively increased in difficulty as buffer sizes and memory layouts changed.

## Tasks Performed

### Task 1: Familiarization with Shellcode

- Modified and injected shellcode to delete sensitive file.

### Task 2: Level-1 Attack (32-bit Program)

- Identified buffer memory boundaries and adjusted the return address to jump to the malicious code.
- Successfully gained root access on a 32-bit target server.

### Task 3: Level-2 Attack

- Conducted further exploitation by fine-tuning return addresses and memory layouts.
- Used NOP sleds to guide execution to the shellcode, gaining root access again.

### Task 4: Level-3 Attack (64-bit Program)

- Overcame additional challenges with null bytes in 64-bit architecture, which required relocating shellcode.
- Successfully relocated the shellcode and exploited the vulnerability in a 64-bit server program, gaining root access.

### Task 5: Level-4 Attack (Small Buffer)

- Faced the challenge of exploiting a program where the buffer was smaller than the shellcode size. Instead of injecting the shellcode into the vulnerable function's buffer, I exploited a larger buffer in the main() function.
- Adjusted the return address to point to this alternative buffer, successfully executing the shellcode and obtaining root privileges.

### Task 6: Experimenting with Address Randomization

- Investigated how turning on Address Space Layout Randomization (ASLR) impacts the predictability of memory addresses, making exploitation significantly more challenging.

## Outcome

- Successfully exploited multiple levels of buffer overflow vulnerabilities in both 32-bit and 64-bit server programs.
- Developed a strong understanding of buffer overflow attacks, shellcode injection, and various stack-based exploit techniques.
- Gained practical experience with countermeasures like ASLR, Non-executable Stack, and StackGuard, understanding their impact on the success of attacks.

## Supporting Documents:

[REPORT](https://hoangnguyen2809.github.io/posts/2024/07/blog-post-5/)
