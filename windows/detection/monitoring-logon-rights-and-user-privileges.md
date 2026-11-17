---
title: "Monitoring Logon Rights and User Privileges"
domain: [Windows]
category: [Detection, Logging & Monitoring, Privilege Escalation]
difficulty: Intermediate
verification: Verified
attack_ids: [T1134, T1078, T1003.002]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations
  - https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants
  - https://attack.mitre.org/techniques/T1134/
  - https://attack.mitre.org/techniques/T1078/
  - https://attack.mitre.org/techniques/T1003/002/
open_issues: []
---

# Monitoring Logon Rights and User Privileges

## Summary

What an account can do on a Windows machine is decided by two lists in local security policy (pushed by Group Policy in a domain): **logon rights**, which say *how* an account may log on (interactively, over the network, as a batch job, as a service, through Remote Desktop), and **privileges**, which grant powerful system-wide abilities such as debugging any process, backing up any file regardless of ACLs, or loading drivers. Attackers who gain a foothold try to acquire these rights, and once they have them they use them. Windows records both the assignment of rights and their use, so a monitoring programme should watch both.

## Key events

| Event ID | Meaning | Why it matters |
|---|---|---|
| **4704** | A user right was assigned | Someone granted a privilege or logon right, e.g. `SeDebugPrivilege` to a new group |
| **4705** | A user right was removed | Rights stripped, sometimes to lock out defenders |
| **4672** | Special privileges assigned to new logon | Logon of an account holding admin-equivalent privileges; baseline who should generate it |
| **4673** | A privileged service was called | Use of a sensitive privilege to call a privileged service |
| **4674** | An operation was attempted on a privileged object | Use of a sensitive privilege on an object (e.g. a handle opened with special rights) |

## Privileges worth watching

| Privilege | Why attackers want it |
|---|---|
| `SeDebugPrivilege` | Open any process, including LSASS, for memory reading or injection |
| `SeBackupPrivilege` / `SeRestorePrivilege` | Read or write any file regardless of its ACL, e.g. copy registry hives (T1003.002) |
| `SeImpersonatePrivilege` / `SeAssignPrimaryTokenPrivilege` | Impersonate or create processes with other tokens, the root of "potato" style escalations (T1134) |
| `SeLoadDriverPrivilege` | Load kernel drivers, including vulnerable ones |
| `SeTakeOwnershipPrivilege` | Take ownership of any object and rewrite its ACL |
| `SeTcbPrivilege` | Act as part of the operating system |

## Logon rights worth watching

Allow and deny pairs exist for local (interactive) logon, Remote Desktop, network access, batch jobs and services. Unexpected grants of *Log on as a service* or *Allow log on through Remote Desktop Services*, or removal of *Deny* entries for privileged accounts on lower-tier systems, undermine tiered administration (T1078).

## Detection ideas

- Alert on any 4704 outside a change window, and on grants of the privileges in the table above.
- Baseline accounts that generate 4672; a new account appearing is often the first sign of privilege escalation.
- 4673/4674 are noisy by default; filter to the privileges above and to processes that should not hold them.
- Pair privilege-use events with process creation (4688) to see which program used the privilege.

## ATT&CK mapping

- **T1134 Access Token Manipulation** (impersonation and debug privileges)
- **T1078 Valid Accounts** (abuse of granted logon rights)
- **T1003.002 Security Account Manager** (backup privilege used to read hives)

## Explanation

Logon rights are the doors an employee's badge opens; privileges are the master keys some employees carry for special jobs, like the key that opens every filing cabinet "for backups". Watching who is issued a master key catches the setup, and watching when master keys are used catches the act. A cleaner who suddenly holds the backup master key, or uses it at 3 a.m., is worth a conversation.
