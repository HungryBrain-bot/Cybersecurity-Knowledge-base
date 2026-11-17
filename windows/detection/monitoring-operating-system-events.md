---
title: "Monitoring Operating System Events (Log Clearing, Services, Time, BCD)"
domain: [Windows]
category: [Detection, Logging & Monitoring, Defense Evasion]
difficulty: Intermediate
verification: Verified
attack_ids: [T1685.005, T1685.001, T1543.003, T1053.005, T1553.006]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4826
  - https://attack.mitre.org/datacomponents/DC0060/
  - https://attack.mitre.org/techniques/T1685/005/
  - https://attack.mitre.org/techniques/T1685/001/
  - https://attack.mitre.org/techniques/T1543/003/
  - https://attack.mitre.org/techniques/T1053/005/
  - https://attack.mitre.org/techniques/T1553/006/
open_issues: []
---

# Monitoring Operating System Events (Log Clearing, Services, Time, BCD)

## Summary

Some of the clearest signs of an intruder are not file or network events but changes to the operating system itself: the Security log being wiped, the audit policy being switched off, the clock being moved, a new service or scheduled task appearing, or boot settings being altered to allow unsigned drivers. Windows logs each of these, and because they are rare on a well-run system, they make high-value, low-noise alerts.

## Key events

| Event ID | Log | Meaning | Attacker purpose |
|---|---|---|---|
| **1102** | Security | The audit log was cleared | Cover tracks (T1685.005) |
| **4719** | Security | System audit policy was changed | Turn off auditing before acting (T1685.001) |
| **4912** | Security | Per-user audit policy was changed | Exempt an account from auditing |
| **4616** | Security | The system time was changed | Confuse timelines; disrupt Kerberos |
| **4697** | Security | Attempt to install a service | Persistence or remote execution via services (T1543.003) |
| **7045** | System | A service was installed (Service Control Manager) | Same, visible even without Security auditing |
| **4698–4702** | Security | Scheduled task created, deleted, enabled, disabled, updated | Persistence and execution (T1053.005) |
| **4826** | Security | Boot Configuration Data loaded | Shows boot settings such as test signing or disabled integrity checks (T1553.006) |

## Scenarios to alert on

- **Log cleared (1102)** on any server, or on any workstation outside imaging. Investigate the account in the event; legitimate clears are rare.
- **Audit policy changed (4719)** where success or failure auditing was removed from a subcategory you depend on.
- **Time changed (4616)** by anything other than the time service; large jumps on domain controllers deserve attention.
- **New service (4697/7045)** with a binary in a user-writable path, a random name, or a command line invoking `cmd.exe` or PowerShell; this pattern covers PsExec-style remote execution too.
- **Scheduled task created (4698)** by a non-admin tool or containing encoded PowerShell.
- **BCD (4826)** showing test signing on, kernel debugging enabled, or integrity checks disabled on production machines.

## Hardening the evidence

- Forward Security and System logs off the host in near real time so a clear only removes the local copy.
- Size logs so a busy server does not roll over within your investigation window.
- Restrict who holds the right to manage auditing and security logs.

## ATT&CK mapping

- **T1685.005 Clear Windows Event Logs**
- **T1685.001 Disable or Modify Windows Event Log**
- **T1543.003 Windows Service**
- **T1053.005 Scheduled Task**
- **T1553.006 Code Signing Policy Modification**

## Explanation

These are the building's own alarm panel events: someone shredded the visitor log, switched off a camera, moved the clocks, hired a new contractor who lives on site (a service), set a recurring reminder for themselves (a task), or changed the rules for who is let in at opening time (boot settings). Each happens rarely in a well-run building, so each deserves a look when it does.
