# macOS

macOS (and iOS) security model, forensics, and threat hunting: how Apple's platform protections work, how macOS malware persists and is analyzed, and how incidents are investigated. Content is curated and written fresh from the knowledge base, cross-checked against [MITRE ATT&CK](https://attack.mitre.org/) and [Apple Platform Security](https://support.apple.com/guide/security/welcome/web).

Each file carries a `verification` front-matter block listing the primary sources it was checked against.

## Internals and security model

| Concept | ATT&CK |
|---|---|
| [macOS and iOS Security Model and Threats](internals/macos-and-ios-security-model-and-threats.md) | T1553.001, T1647 |

## Detection and hunting

| Concept | ATT&CK |
|---|---|
| [macOS Persistence and Malicious Behaviour Hunting](detection/macos-persistence-and-malicious-behaviour-hunting.md) | T1543.001, T1543.004, T1547.015, T1647, T1569.001 |

## Forensics

| Concept | ATT&CK |
|---|---|
| [macOS Incident Response Data Collection and System Manipulation](forensics/macos-incident-response-data-collection.md) | T1543.001, T1543.004 |
| [macOS Memory Forensics](forensics/macos-memory-forensics.md) | T1055, T1057 |
