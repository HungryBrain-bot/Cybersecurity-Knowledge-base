---
title: "Active Directory Object Auditing (SACL and Change Events)"
domain: [Windows]
category: [Logging & Monitoring, Detection]
difficulty: Advanced
verification: Verified
attack_ids: [T1003.006, T1484.001, T1098, T1134.005]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations
  - https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4765
  - https://learn.microsoft.com/en-us/windows/win32/adschema/r-ds-replication-get-changes
  - https://learn.microsoft.com/en-us/windows/win32/adschema/r-ds-replication-get-changes-all
  - https://attack.mitre.org/techniques/T1003/006/
  - https://attack.mitre.org/techniques/T1484/001/
  - https://attack.mitre.org/techniques/T1098/
  - https://attack.mitre.org/techniques/T1134/005/
open_issues: []
---

# Active Directory Object Auditing (SACL and Change Events)

## Summary

Many of the most damaging Active Directory attacks are just **writes to directory objects**: adding an account to a privileged group, editing a Group Policy Object, granting someone replication rights, changing the AdminSDHolder template, or adding SID history. Domain controllers will record these changes, but only if two things are in place: the **Directory Service** audit subcategories are enabled, and the objects you care about carry **audit entries in their SACL** (system access control list). With both, the Security log on each domain controller shows who changed what, including the old and new attribute values.

## Two halves of AD auditing

| Piece | What it does |
|---|---|
| **Audit policy** (Advanced Audit Policy, DS Access category) | Turns on the event families on domain controllers: *Directory Service Changes* and *Directory Service Access* |
| **SACL on objects** | Says which principals' which operations on which objects or attributes are audited; inherited down the tree |

Without a matching SACL entry, enabling the policy produces little; without the policy, SACLs produce nothing.

## Key events

| Event ID | Meaning | Use |
|---|---|---|
| **5136** | A directory service object was modified | Attribute-level changes with old and new values (e.g. `member`, `gPCFileSysPath`, `nTSecurityDescriptor`) |
| **5137** | A directory service object was created | New users, computers, GPOs, trust objects |
| **5138** | A directory service object was undeleted | Restored objects |
| **5139** | A directory service object was moved | Objects moved between OUs (can change which GPOs apply) |
| **5141** | A directory service object was deleted | Deletions |
| **4662** | An operation was performed on an object | Property reads and **extended rights** use, e.g. replication rights |
| **4765 / 4766** | SID History was added / an attempt to add failed | SID-history injection |

## What to put SACLs on

- **Domain root (domain naming context)**: audit use of the extended rights *Replicating Directory Changes* (`1131f6aa-9c07-11d1-f79f-00c04fc2dcd2`) and *Replicating Directory Changes All* (`1131f6ad-9c07-11d1-f79f-00c04fc2dcd2`). Event 4662 with these GUIDs from a machine that is not a domain controller is the classic **DCSync** signal (T1003.006).
- **AdminSDHolder** and privileged groups (Domain Admins, Enterprise Admins, Administrators, Schema Admins): audit writes to `member` and to the security descriptor.
- **Group Policy containers**: audit modifications so GPO abuse (T1484.001) shows as 5136 on `groupPolicyContainer` objects.
- **Sensitive accounts**: audit writes to attributes used for persistence, such as service principal names, delegation settings and SID history (T1098, T1134.005).

## Operating it

- Enable *Directory Service Changes* (success) and, for targeted objects, *Directory Service Access* (success) on domain controllers via GPO.
- Keep SACLs narrow: auditing every read on every object floods the log.
- Forward DC Security logs to a SIEM; a local log on a compromised DC is not evidence you can trust.
- Correlate 5136 changes with the account and source host from the same logon session.

## ATT&CK mapping

- **T1003.006 DCSync**
- **T1484.001 Group Policy Modification**
- **T1098 Account Manipulation**
- **T1134.005 SID-History Injection**

## Explanation

Active Directory is the company's master staff register. An audit policy is the decision to keep a change log for the register; SACLs are the sticky notes on specific pages that say "write down every edit to this page". Without the sticky notes the log stays empty, and if you put sticky notes on every page the log becomes unreadable. Put them on the pages that grant power: the executive list, the policy binder, and the rule that lets someone photocopy the whole register.
