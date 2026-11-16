---
title: "Advanced Local Procedure Call (ALPC) and Windows Notification Facility"
domain: [Windows]
category: [Internals]
difficulty: Advanced
verification: Needs review
attack_ids: []
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/etw/alpc
  - https://learn.microsoft.com/en-us/windows/win32/rpc/protocol-sequence-constants
  - https://learn.microsoft.com/en-us/windows/win32/midl/ncalrpc
open_issues:
  - "Microsoft does not publicly document the ALPC (Nt*Alpc*) system calls or WNF; internal details here follow Windows Internals Part 2 and could not be confirmed against a primary source."
---

# Advanced Local Procedure Call (ALPC) and Windows Notification Facility

## Summary

**ALPC** is the kernel's fast inter-process communication channel for processes on the same machine. Most developers never call it directly: it is the transport behind local RPC (the `ncalrpc` protocol sequence), and through RPC it carries traffic between applications and services such as the Local Security Authority, the Service Control Manager and many COM servers. The **Windows Notification Facility (WNF)** is a separate kernel publish/subscribe mechanism: components publish small blobs of state under a well-known "state name" and other components subscribe to be told when that state changes. Both live inside the kernel, both connect low-privileged callers to high-privileged services, and so both are recurring sources of local privilege escalation bugs and of telemetry for defenders.

## Key ideas

| Concept | Meaning |
|---|---|
| **ALPC port** | A named kernel object a server creates; clients connect to it and get a private communication channel |
| **Connection model** | Server listens on a connection port; each accepted client gets its own pair of communication ports |
| **Messages** | Small messages are copied through the kernel; large payloads are shared through mapped sections (views) |
| **Message attributes** | Optional extras such as security context, handles passed between processes, and views |
| **Local RPC (`ncalrpc`)** | The RPC protocol sequence for local calls; supports Windows (NTLM-style) authentication only |
| **WNF state name** | An identifier for a piece of published state; subscribers are notified when it changes |

## How it works

### ALPC

A server process creates a named ALPC port in the object manager namespace (commonly under `\RPC Control`). A client asks the kernel to connect; the server inspects the request and accepts or refuses it. Once connected, the two sides exchange messages synchronously or asynchronously. Because the kernel brokers every message, it can attach the client's security context so the server can impersonate the caller, and it can duplicate handles from one process into the other when a message carries a handle attribute.

For large data, the two sides share a section (a region of memory mapped into both processes) instead of copying bytes through the kernel each time. ETW exposes ALPC activity through a dedicated event class (send, receive, wait-for-reply and related events), which is how tools can trace ALPC traffic without hooking.

### WNF

WNF lets kernel and user-mode components publish state such as "network connectivity changed" or "a shell feature toggled". Each state name has a scope and lifetime and a security descriptor that controls who may read or write it. Subscribers register a callback and are notified when the data changes.

## Security relevance

- **Privilege boundary.** ALPC and RPC servers often run as SYSTEM while accepting connections from any user. Bugs in how a server validates messages, impersonates, or handles passed handles have repeatedly produced local privilege escalation.
- **Impersonation.** A server that impersonates a client without checking the impersonation level, or a client tricked into connecting to an attacker-owned port, both lead to token abuse (see Access Tokens and Impersonation).
- **Handle passing.** Handles duplicated through message attributes are capabilities; a server that leaks a privileged handle hands that access to the caller.
- **WNF as a primitive.** Writable WNF state with a weak security descriptor has been used in exploitation research as a kernel memory primitive.

## Detection and defence

- Trace ALPC with the kernel ETW provider's ALPC event class and correlate unusual client-to-server pairs.
- Audit RPC interfaces with tools that enumerate endpoints and their security descriptors; restrict sensitive interfaces with RPC filters where available.
- Keep systems patched: most real ALPC/RPC issues are fixed server-side by Microsoft.

## ATT&CK mapping

No dedicated technique. ALPC and RPC underpin techniques covered elsewhere, such as token impersonation (T1134) and exploitation for privilege escalation (T1068).

## Explanation

Think of ALPC as the building's internal mail system. Every office (process) can send sealed envelopes to the facilities desk (a service) through the mailroom (the kernel), and the mailroom stamps each envelope with the sender's badge so the desk knows who asked. Big parcels go into a shared locker both sides can open instead of being passed hand to hand. WNF is the noticeboard in the lobby: some offices pin up status notes, others ask to be told when a note changes. Problems arise when the desk trusts an envelope it should have checked, or when anyone can scribble on a notice that a privileged office relies on.
