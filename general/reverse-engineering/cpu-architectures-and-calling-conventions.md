---
title: "CPU Architectures and Calling Conventions"
domain: [General]
category: [Reverse Engineering]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention
  - https://developer.arm.com/documentation/
  - https://en.wikipedia.org/wiki/X86_calling_conventions
open_issues: []
---

# CPU Architectures and Calling Conventions

## Summary

Reverse engineering and low-level security work require understanding the **CPU architecture** a binary runs on and the **calling conventions** that govern how functions pass arguments and return — because this knowledge is what lets an analyst read disassembly, follow control flow, and reason about exploitation. An **architecture** (instruction set) defines the registers, instructions, and memory model: the dominant families are **x86** (32-bit) and **x86-64/AMD64** (64-bit) on desktops and servers, and **ARM/AArch64** on mobile and increasingly elsewhere, with others (MIPS, RISC-V, PowerPC) in embedded and specialized systems. Architectures differ in **register sets** (e.g. x86's EAX/EBX…, x86-64's RAX/RBX… plus more general registers, ARM's R0–R15/Xn), **endianness**, instruction encoding (variable-length CISC on x86 vs. fixed-length RISC on ARM), and the mechanics of the **stack** and function calls. A **calling convention** is the agreed contract for how a caller and callee cooperate: which **registers vs. stack** carry arguments, where the **return value** goes (typically a specific register like EAX/RAX/X0), who cleans up the stack, and which registers must be **preserved** (callee-saved) versus may be clobbered (caller-saved). Conventions vary by architecture and platform — for example 32-bit x86 has several (cdecl, stdcall, fastcall) that mostly pass arguments on the stack, while x86-64 uses register-based conventions that differ between the System V (Linux/macOS) and Microsoft Windows ABIs, and ARM passes the first arguments in registers. The security relevance is direct: reading disassembly and decompiler output, identifying function boundaries and parameters, understanding the stack layout that buffer-overflow and ROP exploits manipulate, and writing shellcode all depend on knowing the architecture and its calling convention. The core insight is that an architecture sets the vocabulary (registers, instructions, memory/stack model) and the calling convention sets the grammar of function calls — together they are the foundation for reading binaries and reasoning about low-level security.

## Architecture and convention essentials

| Element | What it defines |
|---|---|
| **Architecture (ISA)** | Registers, instructions, memory model (x86/x86-64, ARM/AArch64, …) |
| **Registers** | General-purpose and special registers; names differ per arch |
| **Endianness/encoding** | Byte order; variable-length (CISC) vs fixed (RISC) |
| **Calling convention** | Args in registers vs stack; who passes what |
| **Return value** | Register that holds the result (EAX/RAX/X0) |
| **Caller/callee-saved** | Which registers must be preserved across a call |

Architecture sets the vocabulary; the calling convention sets the grammar of function calls.

## Security relevance

- **Reading disassembly**: you cannot follow a binary without knowing its registers and instruction model.
- **Function analysis**: calling conventions reveal arguments, return values, and stack layout.
- **Exploitation**: overflow, ROP, and shellcode all manipulate the stack and registers the convention defines.
- **Portability of skills**: the same concepts transfer across x86, ARM, and others with different details.

## Practices

- **Identify the target architecture** (x86, x86-64, ARM/AArch64) before analysis — tools and semantics depend on it.
- **Learn the platform's calling convention(s)** to recover function arguments and return values from disassembly.
- **Track the stack and register state** to understand control flow and exploitation primitives.
- **Note ABI differences** (e.g. System V vs Windows x64) when analyzing cross-platform code.
- **Use the return-value register** (EAX/RAX/X0) to follow data out of functions.

## Explanation

To read a program at the machine level, you first have to know what machine you're on and the house rules it follows. The architecture — the instruction set — is like the language's vocabulary: it defines the registers (the CPU's tiny, fast scratchpads), the available instructions, and how memory and the stack work. The big ones are x86 and its 64-bit successor x86-64 on laptops and servers, and ARM on phones and more and more everywhere else, with a long tail of others in embedded gadgets. They differ in telling ways: how many registers and what they're called, which end of a number comes first in memory, and whether instructions are a tidy fixed size (ARM) or a jumble of lengths (x86). Sitting on top of the architecture is the calling convention, which is the grammar for how one function calls another. It settles the practical questions: do the arguments ride in registers or get pushed onto the stack, which register holds the answer when the function returns, who's responsible for tidying up the stack afterward, and which registers a function is allowed to scribble on versus must hand back untouched. These rules vary — old 32-bit x86 had a handful of conventions that mostly used the stack, while 64-bit code tends to hand the first few arguments in registers, and even then Windows and Linux disagree on the exact details. Why does any of this matter for security? Because every time you read disassembly to understand malware, spot a function's inputs and outputs, map the stack that a buffer overflow smashes, or hand-write shellcode, you are leaning on exactly this knowledge. The architecture gives you the words and the calling convention gives you the sentence structure, and together they let you actually read what a binary is doing.
