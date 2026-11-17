---
title: "WoW64 (32-bit on 64-bit, Redirection and Emulation)"
domain: [Windows]
category: [Internals]
difficulty: Intermediate
verification: Needs review
attack_ids: [T1106, T1055]
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/winprog64/wow64-implementation-details
  - https://learn.microsoft.com/en-us/windows/win32/winprog64/file-system-redirector
  - https://learn.microsoft.com/en-us/windows/win32/winprog64/registry-redirector
  - https://learn.microsoft.com/en-us/windows/win32/winprog64/running-32-bit-applications
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg-query
  - https://attack.mitre.org/techniques/T1106/
  - https://attack.mitre.org/techniques/T1055/
open_issues:
  - "The 'Heaven's Gate' mode-switch technique is described from malware research, not a Microsoft or ATT&CK primary source; mapping to T1106 is our inference."
---

# WoW64 (32-bit on 64-bit, Redirection and Emulation)

## Summary

**WoW64** ("Windows 32-bit on Windows 64-bit") is the user-mode compatibility layer that lets 32-bit programs run on 64-bit Windows. It sits between the 32-bit `ntdll.dll` and the 64-bit kernel, converting ("thunking") each 32-bit system call into its 64-bit form. It also gives 32-bit programs a separate view of parts of the file system and registry, so `System32` and `HKLM\Software` mean something different to a 32-bit process than to a 64-bit one. That redirection is a frequent cause of confusion in incident response ("the file is not there") and something malware both exploits and trips over.

## Components

| Component | Role |
|---|---|
| `Wow64.dll` | Core emulation infrastructure and thunks for `ntoskrnl.exe` entry points |
| `Wow64Win.dll` | Thunks for `win32k.sys` (GUI) entry points |
| `Wow64Cpu.dll` | x64 only: support for running x86 code on x64 |
| `xtajit.dll` | ARM64 only: x86 emulator |
| `wowarmw.dll` | ARM64 only: runs ARM32 programs |
| 64-bit `ntdll.dll` | The only other 64-bit binary loaded into a 32-bit process |

At startup `Wow64.dll` loads the 32-bit `ntdll.dll`, which loads the usual 32-bit DLLs. 32-bit binaries make system calls through a custom calling sequence that WoW64 can intercept in user mode; the CPU switches to 64-bit mode, `Wow64.dll` converts the arguments and makes the real 64-bit system call.

## Redirection

### File system

| A 32-bit process asks for | It actually gets (x64) | On ARM64 for ARM32 apps |
|---|---|---|
| `%windir%\System32` | `%windir%\SysWOW64` | `%windir%\SysArm32` |
| `%windir%\lastgood\system32` | `%windir%\lastgood\SysWOW64` | `%windir%\lastgood\SysArm32` |
| `%windir%\regedit.exe` | `%windir%\SysWOW64\regedit.exe` | `%windir%\SysArm32\regedit.exe` |

A 32-bit process can reach the real 64-bit directory through the special alias `%windir%\Sysnative`, which WoW64 does not redirect.

### Registry

The registry redirector gives 32-bit and 64-bit applications separate logical views of certain keys. For example, `HKLM\Software` seen by a 32-bit process is physically stored under `HKLM\Software\Wow6432Node`. Some keys are shared rather than redirected.

## Security relevance

- **Investigation pitfalls.** A 32-bit tool run on a 64-bit host sees `SysWOW64` when it asks for `System32` and the `Wow6432Node` view of the registry, so it can miss 64-bit persistence entries (or vice versa). Use 64-bit tools or check both views.
- **Persistence in both views.** Run keys and COM registrations exist in both the native and `Wow6432Node` views; attackers use whichever the target loader reads, and defenders must check both.
- **"Heaven's Gate".** Malware running as 32-bit code can switch the CPU into 64-bit mode itself and call 64-bit `ntdll` or make 64-bit system calls directly, bypassing 32-bit user-mode hooks placed by security products (T1106).
- **Cross-architecture injection (T1055).** Injecting from a 32-bit process into a 64-bit one (or the reverse) needs special handling; tooling that gets this wrong crashes the target, which itself is a useful detection signal.

## Detection

- Note the bitness of every suspicious process; on x64, a process loaded with `wow64.dll` is 32-bit.
- When hunting registry persistence, query both the 64-bit and 32-bit registry views (`reg query ... /reg:64` and `/reg:32`).
- Watch for 32-bit processes that execute 64-bit code segments, which is unusual outside WoW64 itself.

## ATT&CK mapping

- **T1106 Native API**: direct 64-bit system calls from WoW64 processes.
- **T1055 Process Injection**: cross-bitness injection.

## Explanation

WoW64 is an interpreter at an embassy. A visitor who only speaks the old language (a 32-bit program) talks to the interpreter, who turns each request into the local language (64-bit calls) for the officials (the kernel). The interpreter also keeps a separate filing cabinet for visitors, so when a visitor asks for "the main file room" they are shown the visitors' room instead. Investigators who forget that there are two rooms will search one and conclude a file does not exist.
