---
title: "Micro-Virtualization and Application Isolation Against Advanced Malware"
domain: [Windows, Malware]
category: [Defense Evasion]
difficulty: Advanced
verification: Verified
attack_ids: [T1189, T1566.001, T1203, T1611]
checked_against:
  - https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-overview
  - https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/microsoft-defender-application-guard/md-app-guard-overview
  - https://learn.microsoft.com/en-us/windows/whats-new/deprecated-features
  - https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/vsm
  - https://attack.mitre.org/techniques/T1189/
  - https://attack.mitre.org/techniques/T1566/001/
  - https://attack.mitre.org/techniques/T1203/
  - https://attack.mitre.org/techniques/T1611/
open_issues: []
---

# Micro-Virtualization and Application Isolation Against Advanced Malware

## Summary

Detection-based defences fail against malware nobody has seen before. **Isolation** takes a different approach: assume the content is malicious and run it somewhere it cannot hurt anything. **Micro-virtualization** applies this to individual risky tasks, opening each web page, email attachment or downloaded document inside its own lightweight, hardware-isolated virtual machine that is thrown away afterwards. Even if an exploit succeeds, it lands in a disposable VM with no access to the user's files, credentials or network. A side benefit is forensics: because the isolated environment is instrumented, it records what the malware tried to do.

## Why it exists

Browsers, document readers and formerly browser plug-ins such as Flash are the most exploited software on endpoints, because they process untrusted content from the internet all day. Drive-by compromise (T1189), malicious attachments (T1566.001) and client-side exploits (T1203) all depend on that content running with the user's privileges on the real desktop. Isolation removes that assumption.

## Implementations on Windows

| Technology | What it isolates | Status |
|---|---|---|
| **Microsoft Defender Application Guard (MDAG)** | Untrusted sites in Edge, and Office files, in a Hyper-V container | Deprecated for Microsoft Edge for Business (announced December 2023); no longer available from Windows 11 version 24H2 |
| **Windows Sandbox** | A whole temporary desktop for running untrusted applications; uses hardware-based virtualization for kernel isolation; everything is deleted when it is closed | Available on supported Windows editions |
| **Commercial micro-VM products** | Per-task micro-VMs for browsing and documents | Third-party |
| **Browser and app sandboxes** | Process-level isolation (AppContainer, job objects) without a VM | Built into modern browsers and Office |

All of the virtualization-based options depend on the Windows hypervisor (see Windows Hypervisor and Virtualization Stack).

## Strengths and limits

- **Strength:** protection does not depend on recognising the malware.
- **Strength:** isolated sessions provide clean telemetry of exploit behaviour.
- **Limit:** anything the user deliberately moves out of isolation (saving a file to the real desktop, copying credentials into a page) leaves the protection.
- **Limit:** isolation is only as strong as the hypervisor or container boundary; escaping it is the attacker's goal (T1611).
- **Limit:** product lifecycle; MDAG's deprecation means organisations relying on it need a replacement plan.

## Defence guidance

- Isolate the highest-risk activities (browsing unknown sites, opening external attachments), not everything.
- Combine with application control and attack surface reduction so content that escapes isolation still cannot run freely.
- Keep the hypervisor and host patched; container escape bugs are the main bypass.

## ATT&CK mapping

- **T1189 Drive-by Compromise**
- **T1566.001 Spearphishing Attachment**
- **T1203 Exploitation for Client Execution**
- **T1611 Escape to Host** (the bypass attackers need)

## Explanation

Micro-virtualization is a bomb disposal room for every suspicious parcel. Instead of guessing which parcels are dangerous, you open all of them inside a sealed room that is demolished and rebuilt after each one. If a parcel explodes, nothing outside is harmed, and the cameras in the room show exactly what it contained. The risk is a staff member carrying something out of the room by hand before it is demolished.
