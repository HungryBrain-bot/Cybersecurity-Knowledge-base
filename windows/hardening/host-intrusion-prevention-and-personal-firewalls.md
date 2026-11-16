---
title: "Host Intrusion Prevention and Personal Firewalls"
domain: [Windows]
category: [Defense Evasion]
difficulty: Intermediate
verification: Verified
attack_ids: [T1055, T1685, T1071]
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/fwp/windows-filtering-platform-start-page
  - https://learn.microsoft.com/en-us/defender-endpoint/tamper-protection-overview
  - https://attack.mitre.org/techniques/T1055/
  - https://attack.mitre.org/techniques/T1685/
  - https://attack.mitre.org/techniques/T1071/
open_issues: []
---

# Host Intrusion Prevention and Personal Firewalls

## Summary

A **personal (host) firewall** decides which programs on a machine may send and receive network traffic. A **host intrusion prevention system (HIPS)** goes further and watches what programs *do* (create processes, write to sensitive registry keys, inject into other processes) and blocks actions that look malicious. Both ideas now live inside modern endpoint products and in Windows itself, through the **Windows Filtering Platform** for network filtering and EDR behaviour engines for the rest. Their weakness has always been the same: they judge actions by who performs them and what they look like, so malware works hard to look like a trusted program or to switch the protection off.

## How they work

| Control | Decision basis | Classic weakness |
|---|---|---|
| **Per-application firewall** | Which executable owns the connection | Malware injects into an allowed process (browser, `svchost.exe`) and uses its permission (T1055) |
| **Signature-based HIPS** | Known-bad patterns of behaviour | New or slightly altered behaviour is not matched |
| **Behavioural HIPS** | Sequences of actions judged as malicious intent | Intent is hard to infer; legitimate admin tools look similar, causing false positives and user fatigue |
| **Self-protection** | Product protects its own processes and settings | Attackers with admin or kernel access try to disable it (T1685) |

On Windows, firewall and filtering products build on the **Windows Filtering Platform (WFP)**, a set of APIs and system services that let software inspect and filter network traffic at several layers of the stack.

## Evasion patterns to know

- **Riding a trusted process**: injecting into or launching through an allowed application so traffic appears to come from it.
- **Using allowed protocols**: C2 over HTTPS or DNS that an application firewall permits for browsers anyway (T1071).
- **Turning the control off**: stopping services, deleting filters, or editing settings. Modern products counter this with tamper protection; Microsoft Defender's tamper protection, for example, keeps real-time protection on and blocks changes to key security settings.
- **Prompt fatigue**: older personal firewalls asked users to allow or deny; users learned to click "allow".

## Defence today

- Use centrally managed host firewall policy rather than user prompts.
- Prefer EDR with behavioural detection plus tamper protection over standalone HIPS products.
- Combine with application control so unknown executables never get the chance to act.

## ATT&CK mapping

- **T1055 Process Injection** (to inherit a trusted program's permissions)
- **T1685 Disable or Modify Tools**
- **T1071 Application Layer Protocol** (blending into allowed traffic)

## Explanation

A personal firewall is a doorman with a guest list of people allowed to leave the building with parcels. A HIPS is a floor supervisor who watches what everyone does and stops anyone acting suspiciously. Both can be fooled by an intruder who borrows a regular's coat and badge, or who quietly sends the doorman home. That is why modern guards also lock their own office door (tamper protection) and the building checks every new face at the entrance (application control).
