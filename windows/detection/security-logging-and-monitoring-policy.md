---
title: "Security Logging and Monitoring Policy"
domain: [Windows, General]
category: [Logging & Monitoring, Detection]
difficulty: Beginner
verification: Verified
attack_ids: [T1685.001, T1685.005]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/auditpol
  - https://attack.mitre.org/techniques/T1685/001/
  - https://attack.mitre.org/techniques/T1685/005/
open_issues: []
---

# Security Logging and Monitoring Policy

## Summary

Turning on every audit setting does not produce security monitoring; it produces a full disk and an unread SIEM. Good monitoring starts with a **policy** that answers, for each kind of system: which events are collected and why, how long they are kept, where they are sent, who looks at them, and what happens when a rule fires. On Windows that policy is expressed through **Advanced Audit Policy** subcategories, log size and retention settings, and forwarding, and it should be tied to concrete threat scenarios rather than to the full list of available events.

## Two separate jobs

| Job | Question it answers | Output |
|---|---|---|
| **Security logging** | What do we record, where, for how long, and how do we protect it? | Audit policy baselines, log sizes, retention, forwarding, tamper protection |
| **Security monitoring** | What do we look for, who responds, how fast? | Detection scenarios, alert rules, owners, playbooks |

## Building the policy

1. **Start from scenarios.** For each threat you care about (credential theft, lateral movement, persistence, data theft, log tampering), write down which actions an attacker takes and which events those actions produce.
2. **Map scenarios to audit subcategories.** Enable the Advanced Audit Policy subcategories that produce those events (for example *Process Creation*, *Logon*, *Special Logon*, *Security Group Management*, *Directory Service Changes*, *Audit Policy Change*). Microsoft's audit policy recommendations give baseline and stronger settings per subcategory for workstations, member servers and domain controllers.
3. **Tier by system role.** Domain controllers, admin workstations and internet-facing servers deserve more coverage and longer retention than standard desktops.
4. **Scope noisy subcategories.** File system, registry, filtering platform and detailed file share events can overwhelm collection; enable them only with targeted SACLs or on specific servers.
5. **Size and retain.** Set Security log size so local logs cover at least the time it takes to detect and respond; keep central copies for your investigation and compliance windows.
6. **Forward and protect.** Send logs off-host quickly, restrict who can change audit policy and clear logs, and alert when either happens (T1685.001, T1685.005).
7. **Define monitoring.** For each scenario: the alert logic, its severity, who owns it, and the response playbook. Review false positives and missed detections regularly.

## Practical tools

- `auditpol` shows and sets the effective Advanced Audit Policy on a host, which is the quickest way to check that Group Policy actually applied.
- Microsoft's *Events to Monitor* appendix rates event IDs by criticality and is a useful starting list for alert rules.
- Sysmon or an EDR adds process, network and file telemetry the native Security log does not provide.

## Common failures

- Logging enabled but never forwarded, so the evidence is lost when the attacker clears the log.
- Default log sizes that roll over in hours on busy servers.
- Alerts with no owner.
- Policies copied from a template without checking that the subcategories actually map to the threats in scope.

## ATT&CK mapping

- **T1685.001 Disable or Modify Windows Event Log**
- **T1685.005 Clear Windows Event Logs**

## Explanation

A logging policy is a plan for CCTV in a shop. You decide which doors and tills get cameras because of the theft you expect, how long recordings are kept, and that recordings go to an off-site recorder so a thief cannot simply take the tape. A monitoring policy is the rota of who actually watches the screens and what they do when they see something. Cameras everywhere with nobody watching, or recordings kept in the till drawer, protect nothing.
