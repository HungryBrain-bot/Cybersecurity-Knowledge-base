---
title: "Monitoring Network File Shares and Named Pipes"
domain: [Windows, Network]
category: [Detection, Logging & Monitoring, Lateral Movement]
difficulty: Intermediate
verification: Needs review
attack_ids: [T1021.002, T1569.002, T1570, T1039, T1135, T1070.005]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5145
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5142
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5143
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5144
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5168
  - https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
  - https://attack.mitre.org/techniques/T1021/002/
  - https://attack.mitre.org/techniques/T1569/002/
  - https://attack.mitre.org/techniques/T1570/
  - https://attack.mitre.org/techniques/T1039/
  - https://attack.mitre.org/techniques/T1135/
  - https://attack.mitre.org/techniques/T1070/005/
open_issues:
  - "The mapping of specific named pipes (svcctl, atsvc, samr) to remote service, task and SAM operations follows Windows Security Monitoring (2018); it was not confirmed against Microsoft protocol documentation."
---

# Monitoring Network File Shares and Named Pipes

## Summary

SMB is how Windows machines share files and how much remote administration travels: copying a tool to `ADMIN$`, then asking the remote Service Control Manager or Task Scheduler to run it over a **named pipe** on the hidden `IPC$` share. That makes share and pipe access one of the best places to see **lateral movement**. Windows can log every connection to a share, every file or pipe touched on it, and every change to the list of shares.

## Key events

| Event ID | Subcategory | Meaning |
|---|---|---|
| **5140** | File Share | A network share object was accessed (one per session per share) |
| **5145** | Detailed File Share | A share object was checked to see whether the client can be granted access (one per file or pipe accessed) |
| **5142** | File Share | A network share object was added |
| **5143** | File Share | A network share object was modified |
| **5144** | File Share | A network share object was deleted |
| **5168** | File Share | SPN check for SMB/SMB2 failed |
| Sysmon **17 / 18** | Sysmon | Named pipe created / connected (on the endpoint, not the share) |

5145 is high-volume but the most informative: it names the share, the relative target (file or pipe) and the requested access.

## What lateral movement looks like

1. A logon from a remote host (4624, logon type 3) by an admin account.
2. 5140 for `\\*\ADMIN$` or `\\*\C$`, then 5145 showing a write of an executable (T1570 Lateral Tool Transfer).
3. 5140 for `\\*\IPC$` and 5145 for a named pipe such as `svcctl` (service control), `atsvc` (task scheduler) or `samr` (SAM queries).
4. On the target, a new service (4697/7045) or scheduled task (4698) running the copied binary (T1569.002).

This is the shape of PsExec and many of its clones; WMI-based movement looks different (no admin-share write is required).

## Detection ideas

- Alert on writes of executables or scripts to `ADMIN$` and `C$` from workstations; admin shares are rarely written from user endpoints.
- Baseline which hosts legitimately touch `svcctl` and `atsvc` pipes remotely (management servers, deployment tools) and alert on others.
- Alert on new shares (5142) on workstations, and on changes to share permissions (5143).
- Large numbers of 5140/5145 across many hosts in a short time indicate share enumeration (T1135) or bulk collection from shared drives (T1039).
- Removal of remote share connections on the source host is a cleanup step (T1070.005).

## Keeping volume manageable

Enable *File Share* broadly, and *Detailed File Share* on servers and high-value hosts, or only for failure events, if volume is a concern.

## ATT&CK mapping

- **T1021.002 SMB/Windows Admin Shares**
- **T1569.002 Service Execution**
- **T1570 Lateral Tool Transfer**
- **T1039 Data from Network Shared Drive**
- **T1135 Network Share Discovery**
- **T1070.005 Network Share Connection Removal**

## Explanation

Shares are the doors between buildings, and named pipes are intercoms on the other side of a special service door (`IPC$`). Lateral movement usually means walking through the loading dock (`ADMIN$`) with a box, then using the intercom to ask the facilities office (the Service Control Manager) to open it. Logging who used which door and which intercom, and when, reconstructs the visit even if the box has since been removed.
