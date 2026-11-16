---
title: "Filesystem, Removable Storage and Registry Auditing"
domain: [Windows]
category: [Detection, Logging & Monitoring]
difficulty: Intermediate
verification: Verified
attack_ids: [T1052.001, T1091, T1112, T1547.001, T1003.002]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-6416
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4664
  - https://attack.mitre.org/techniques/T1052/001/
  - https://attack.mitre.org/techniques/T1091/
  - https://attack.mitre.org/techniques/T1112/
  - https://attack.mitre.org/techniques/T1547/001/
  - https://attack.mitre.org/techniques/T1003/002/
open_issues: []
---

# Filesystem, Removable Storage and Registry Auditing

## Summary

Windows can log access to individual files, folders and registry keys, and can report when a new external device is plugged in. These **Object Access** events are some of the most useful for investigating data theft and persistence, and some of the noisiest if switched on carelessly. Like Active Directory auditing, they need two things: the relevant audit subcategory enabled, and a **SACL** (audit entry) on the object. Windows also offers **global object access auditing**, a single SACL applied to every file or every registry key on a machine, for cases where you cannot tag objects one by one.

## Key events

| Event ID | Meaning | Notes |
|---|---|---|
| **4656** | A handle to an object was requested | Records requested access; failures show denied attempts |
| **4663** | An attempt was made to access an object | The actual access (read, write, delete) on a file or key |
| **4660** | An object was deleted | Pair with 4663 using the handle ID to get the path |
| **4664** | An attempt was made to create a hard link | Hard-link tricks against privileged file operations |
| **4657** | A registry value was modified | Old and new value data for audited keys |
| **4670** | Permissions on an object were changed | ACL changes on files or keys |
| **6416** | A new external device was recognized by the system | From the *PNP Activity* subcategory; USB drives and other devices |

## What to audit

**Files and folders**
- Folders holding sensitive data (finance shares, source code, HR) for write and delete, and for read where volume allows.
- Locations used for persistence and tooling: startup folders, `System32`, scheduled-task folders.
- Credential stores: access to backups of `SAM`, `SECURITY` and `NTDS.dit` (T1003.002, T1003.003).

**Registry**
- Run and RunOnce keys and other autostart locations (T1547.001).
- Service configuration under `HKLM\SYSTEM\CurrentControlSet\Services`.
- LSA configuration and security-product settings (T1112 Modify Registry).

**Removable storage**
- Event 6416 shows the device being recognised; with the *Removable Storage* subcategory enabled, file access on removable drives is logged as 4663. Together they cover data exfiltration to USB (T1052.001) and malware arriving on removable media (T1091).

## Keeping volume manageable

- Audit **write, delete and permission changes** broadly; audit **reads** only on genuinely sensitive data.
- Avoid auditing for *Everyone* on busy paths such as user profiles or temp folders.
- Use global object access auditing sparingly and for a defined period, for example during an investigation.
- Sysmon file and registry events (IDs 11, 12, 13) are often cheaper for persistence monitoring; use native auditing where you need access attempts and failures.

## ATT&CK mapping

- **T1052.001 Exfiltration over USB**
- **T1091 Replication Through Removable Media**
- **T1112 Modify Registry**
- **T1547.001 Registry Run Keys / Startup Folder**
- **T1003.002 Security Account Manager**

## Explanation

Object access auditing is like putting a sign-in sheet on specific filing cabinets. A sheet on the payroll cabinet tells you exactly who opened it and when; a sheet on every drawer in the building just produces a mountain of paper nobody reads. The USB event is the front desk noting every visitor who arrives carrying a box.
