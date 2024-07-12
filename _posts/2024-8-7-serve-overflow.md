title: "Buffer-Overflow Attack Lab (Server Version)"
date: 2024-07-08
permalink: /posts/2013/08/blog-post-2/
tags:

- buffer overflow
- cyber

---

SEED Labs – Buffer Overflow Attack Lab (Server Version) done by Hoang Nguyen.

# Buffer Overflow

A buffer overflow occurs when more data is written to a buffer than it can hold, causing the data to overwrite adjacent memory locations. This can lead to various unintended behaviors, including crashes, data corruption, or the execution of arbitrary code.

## Steps in a Buffer Overflow Attack:

1. Overflowing the Buffer:
   An attacker inputs data that exceeds the buffer’s allocated size.
   For example, if a buffer is allocated 10 bytes of memory but the attacker provides 20 bytes of data, the additional 10 bytes will overflow into adjacent memory.

2. Overwriting the Return Address:
   In the stack memory layout, the buffer might be located just below the return address.
   As the buffer overflows, it can overwrite the return address stored on the stack.
   The return address is the location the program will jump to after the current function finishes execution.

3. Injecting Malicious Code:
   Along with the overflowing data, the attacker includes a new return address that points to their malicious code.
   The malicious code can be injected into the buffer itself or elsewhere in memory.

4. Executing:
   When the function attempts to return, it uses the overwritten return address.
   The CPU jumps to the new address, which now points to the attacker’s code.
   The system executes the malicious code with the same privileges as the original program.

# Buffer Overflow Attack Lab (Server Version)

More information on [SEED Buffer Overflow Attack](https://seedsecuritylabs.org/Labs_20.04/Software/Buffer_Overflow_Server/)
