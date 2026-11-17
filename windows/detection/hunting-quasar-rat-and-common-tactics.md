---
title: "Hunting Quasar RAT and Common Tactics with Atomic Tests"
domain: [Windows]
category: [Threat Hunting, Detection]
difficulty: Intermediate
verification: Verified
attack_ids: [T1547.001, T1053.005, T1548.002, T1056.001, T1555.003, T1573.001, T1090, T1564.001, T1082, T1016, T1105]
checked_against:
  - https://attack.mitre.org/software/S0262/
  - https://github.com/quasar/Quasar
  - https://github.com/redcanaryco/atomic-red-team
  - https://github.com/redcanaryco/invoke-atomicredteam
  - https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
  - https://attack.mitre.org/techniques/T1547/001/
  - https://attack.mitre.org/techniques/T1053/005/
  - https://attack.mitre.org/techniques/T1548/002/
  - https://attack.mitre.org/techniques/T1056/001/
  - https://attack.mitre.org/techniques/T1555/003/
  - https://attack.mitre.org/techniques/T1573/001/
  - https://attack.mitre.org/techniques/T1090/
  - https://attack.mitre.org/techniques/T1564/001/
  - https://attack.mitre.org/techniques/T1082/
  - https://attack.mitre.org/techniques/T1016/
  - https://attack.mitre.org/techniques/T1105/
open_issues: []
---

# Hunting Quasar RAT and Common Tactics with Atomic Tests

## Summary

A practical way to build hunts is to **emulate one technique at a time, look at the data it leaves, and write the query that finds it**. Atomic Red Team supplies small, ATT&CK-mapped tests for this; a real, well-documented tool such as **QuasarRAT** gives an end-to-end target. QuasarRAT (ATT&CK S0262) is an open-source C# remote administration tool, public on GitHub since at least 2014 and used by several threat groups. Because its features (keylogging, browser password recovery, reverse proxy, persistence, UAC bypass) are documented and mapped in ATT&CK, it is a good teaching case for hunting across many tactics.

## Method

1. Pick a tactic and technique (for example *Persistence → Registry Run Keys*).
2. Run the matching Atomic test in the lab (`Invoke-AtomicTest` from the Invoke-AtomicRedTeam module runs tests by technique ID).
3. Find the resulting events in Sysmon and Security logs; note fields that distinguish malicious from normal use.
4. Write the hunt query and test it against a quiet baseline for false positives.
5. Clean up the test and repeat for the next technique; finally run a full tool such as QuasarRAT and see how many of your hunts fire.

## QuasarRAT behaviour mapped to hunts

ATT&CK records QuasarRAT using, among others, the techniques below.

| Technique | What Quasar does | Hunt idea |
|---|---|---|
| **T1547.001 Registry Run Keys / Startup Folder** | Run key persistence | Sysmon 13 on `...\CurrentVersion\Run` with a value pointing into user-writable paths |
| **T1053.005 Scheduled Task** | Task-based persistence | 4698 or `schtasks.exe` creation from a non-admin parent |
| **T1548.002 Bypass UAC** | Elevation without prompt | High-integrity process with a medium-integrity, unusual parent |
| **T1564.001 Hidden Files and Directories** | Hides its install folder | Files set hidden/system in profile paths |
| **T1056.001 Keylogging** | Built-in keylogger | Keyboard hook APIs from unsigned binaries; log files growing in user folders |
| **T1555.003 Credentials from Web Browsers** | Browser and FTP client password recovery | Non-browser process reading browser credential stores (Sysmon 10/11) |
| **T1573.001 Symmetric Cryptography** / TLS | Encrypted C2 | Long-lived outbound connections from a user-space binary to rare destinations (Sysmon 3) |
| **T1090 Proxy** | SOCKS5 reverse proxy | The host acting as a relay: inbound-looking traffic patterns from an endpoint |
| **T1082 / T1016** | System and network discovery | Bursts of `systeminfo`/`ipconfig`-style queries from one process |
| **T1105 Ingress Tool Transfer** | Download and execute | New executables written by the RAT process, then launched |

## Hunting tips

- Pivot on the **process tree**: most Quasar activity originates from one unsigned .NET executable in a user path.
- .NET malware loads CLR modules (`clr.dll`, `mscoree.dll`) into a process that has no business being .NET; Sysmon image loads (7) show it.
- Hunt the persistence pair together: a Run key or task **and** a hidden directory created minutes earlier by the same process.
- Treat each hunt as a reusable detection once it has a low false-positive rate.

## ATT&CK mapping

Techniques listed in the table, all taken from ATT&CK's QuasarRAT (S0262) entry.

## Explanation

Learning to hunt is like learning birdsong. You do not start by going into a forest and listening to everything at once; you play one recording, learn its pattern, and only then go out and pick it out of the noise. Atomic tests are the single recordings; running QuasarRAT is the field trip where you check whether you can still recognise each call when they are all mixed together.
