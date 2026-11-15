# Windows

Windows internals, attack techniques, and platform defenses: how the OS works under the hood, how it is attacked across the kill chain, how those techniques are detected, and how the platform is hardened. Content is curated and written fresh from the knowledge base, cross-checked against [MITRE ATT&CK](https://attack.mitre.org/) and [Microsoft Learn](https://learn.microsoft.com/).

Each file carries a `verification` front-matter block listing the primary sources it was checked against.

## Internals and platform foundations

| Concept | ATT&CK |
|---|---|
| [Windows System Architecture (User Mode and Kernel Mode)](internals/windows-system-architecture.md) | — |
| [Processes and Threads Internals](internals/processes-and-threads-internals.md) | — |
| [Windows Memory Management (Virtual Memory and Paging)](internals/windows-memory-management.md) | — |
| [Object Manager and Handles](internals/object-manager-and-handles.md) | — |
| [Windows Boot Process](internals/windows-boot-process.md) | T1542.003, T1542.001 |
| [Windows Services Architecture (SCM and svchost)](internals/windows-services-architecture.md) | — |
| [WMI Architecture (Windows Management Instrumentation)](internals/wmi-architecture.md) | T1047, T1546.003 |
| [Windows Security Model and Access Checks](internals/windows-security-model-and-access-checks.md) | — |
| [Security Descriptors and SDDL](internals/security-descriptors-and-sddl.md) | T1222.001 |
| [Windows Registry Internals (Hives and Structure)](internals/windows-registry-internals.md) | — |
| [PE File Format (Portable Executable)](internals/pe-file-format.md) | — |

## Credential access and authentication

| Concept | ATT&CK |
|---|---|
| [Identity as the New Perimeter and User Identity Attacks](credential-access/identity-as-the-new-perimeter.md) | T1078, T1110, T1621 |
| [Interactive Logon (LsaLogonUser and Logon Sessions)](credential-access/interactive-logon-and-logon-sessions.md) | T1078 |
| [NTLM Authentication](credential-access/ntlm-authentication.md) | T1550.002 |
| [NTLM Relay Attack](credential-access/ntlm-relay-attack.md) | T1557.001 |
| [Negotiate, CredSSP and Other Security Packages](credential-access/negotiate-credssp-and-security-packages.md) | T1550 |
| [Access Tokens, Impersonation and Restricted/Sandbox Tokens](credential-access/access-tokens-impersonation-and-sandbox-tokens.md) | T1134, T1134.001, T1134.002 |
| [Credential Dumping (Hash Dumping and GINA Interception)](credential-access/credential-dumping.md) | T1003, T1003.001, T1003.002, T1556 |
| [Credential Hunting in Files, Processes and Memory](credential-access/credential-hunting-in-files-processes-and-memory.md) | T1552.001, T1552.002, T1003.001 |
| [Lateral Movement with PsExec, WMI and Stolen Credentials](credential-access/lateral-movement-with-psexec-wmi-and-stolen-credentials.md) | T1021.002, T1047, T1569.002, T1550.002 |
| [Network Compromise: Responder, CrackMapExec and Credential Capture](credential-access/network-compromise-responder-crackmapexec.md) | T1557.001, T1040, T1110 |

## Active Directory

| Concept | ATT&CK |
|---|---|
| [Active Directory Security Model (Objects, Schema, Access Checks)](active-directory/active-directory-security-model.md) | T1087.002 |
| [Kerberos Authentication](active-directory/kerberos-authentication.md) | T1558, T1550.003 |
| [Kerberos Delegation](active-directory/kerberos-delegation.md) | T1558, T1134.001 |
| [Kerberoasting](active-directory/kerberoasting.md) | T1558.003 |
| [Active Directory Attack Paths (BloodHound, SPNs, Pass-the-Hash, DCOM)](active-directory/active-directory-attack-paths.md) | T1021, T1550.002, T1558.003, T1087.002 |
| [Domain Persistence (Golden Ticket, Skeleton Key and Sticky Keys)](active-directory/domain-persistence-golden-ticket-skeleton-key.md) | T1558.001, T1207, T1556, T1546.008 |
| [Defending the Windows Domain](active-directory/defending-the-windows-domain.md) | — |

## Privilege escalation

| Concept | ATT&CK |
|---|---|
| [Windows Privilege Escalation Techniques](privilege-escalation/windows-privilege-escalation-techniques.md) | T1068, T1548.002, T1574 |
| [UAC Bypass Techniques (fodhelper and Token Abuse)](privilege-escalation/uac-bypass-techniques.md) | T1548.002 |
| [Token Impersonation and SeDebugPrivilege / SeImpersonate Abuse](privilege-escalation/token-impersonation-and-sedebug-abuse.md) | T1134.001, T1134 |
| [Unquoted Service Paths and Weak Service Permissions](privilege-escalation/unquoted-service-path-and-weak-service-permissions.md) | T1574.009, T1574.010, T1543.003 |

## Persistence

| Concept | ATT&CK |
|---|---|
| [Registry Run Keys and Autostart Extensibility Points](persistence/registry-run-keys-and-autostart.md) | T1547.001, T1547 |
| [Scheduled Task Persistence](persistence/scheduled-task-persistence.md) | T1053.005 |
| [Startup Folder and Shortcut (LNK) Persistence](persistence/startup-folder-and-shortcut-persistence.md) | T1547.001, T1547.009 |
| [Windows Service Persistence](persistence/windows-service-persistence.md) | T1543.003 |

## Process injection

| Concept | ATT&CK |
|---|---|
| [Process Injection (Overview)](injection/process-injection-overview.md) | T1055 |
| [DLL Injection](injection/dll-injection.md) | T1055.001 |
| [APC Injection](injection/apc-injection.md) | T1055.004 |
| [Process Hollowing](injection/process-hollowing.md) | T1055.012 |
| [Process Doppelgänging](injection/process-doppelganging.md) | T1055.013 |
| [Hook Injection (SetWindowsHookEx)](injection/hook-injection-setwindowshookex.md) | T1055, T1056.001 |
| [API Hooking (IAT and Inline Hooks)](injection/api-hooking-iat-and-inline-hooks.md) | T1056.004, T1574.001 |

## Defense evasion

| Concept | ATT&CK |
|---|---|
| [Antimalware Scan Interface (AMSI)](defense-evasion/antimalware-scan-interface-amsi.md) | T1685 |
| [Event Tracing for Windows (ETW) and Windows Error Reporting](defense-evasion/event-tracing-for-windows-and-wer.md) | T1685 |
| [Direct Syscalls and EDR Bypass (Userland Hook Evasion)](defense-evasion/direct-syscalls-and-edr-bypass.md) | T1685, T1106 |
| [PowerShell Obfuscation and Logging Bypass](defense-evasion/powershell-obfuscation-and-logging-bypass.md) | T1059.001, T1027, T1140 |
| [Static Signature Evasion (Obfuscation, Junk Code, Binary Patching)](defense-evasion/static-signature-evasion.md) | T1027 |
| [Security Software Discovery (AV/EDR Fingerprinting)](defense-evasion/security-software-discovery.md) | T1518.001 |
| [DLL Search Order Hijacking](defense-evasion/dll-search-order-hijacking.md) | T1574.001 |
| [NTFS Alternate Data Streams (ADS)](defense-evasion/ntfs-alternate-data-streams.md) | T1564.004 |
| [Keylogging](defense-evasion/keylogging.md) | T1056.001 |
| [Timestomping](defense-evasion/timestomping.md) | T1070.006 |

## Offensive tooling (concepts)

| Concept | ATT&CK |
|---|---|
| [COM Automation and COM Hijacking](offensive-tooling/com-automation-and-com-hijacking.md) | T1546.015, T1559.001 |
| [Antivirus Evasion with Interpreted Payloads (Concepts)](offensive-tooling/antivirus-evasion-with-interpreted-payloads.md) | T1059.006, T1027, T1620 |
| [Custom Droppers and Loaders (Concepts)](offensive-tooling/custom-droppers-and-loaders.md) | T1105, T1140, T1620 |
| [Web-Service C2 and Trojanized Binaries (Concepts)](offensive-tooling/web-service-c2-and-trojanized-binaries.md) | T1102, T1105, T1554 |
| [Graph-Based Attack Path Discovery (BloodHound-Style)](offensive-tooling/graph-based-attack-path-discovery.md) | T1482, T1069, T1087 |
| [Offensive Triage and Enumeration Automation (Concepts)](offensive-tooling/offensive-triage-and-enumeration-automation.md) | T1082, T1057, T1518.001 |

## Exploitation

| Concept | ATT&CK |
|---|---|
| [Exploit Mitigations (DEP, ASLR, CFG, CET)](exploitation/exploit-mitigations-dep-aslr-cfg.md) | — |
| [Stack Buffer Overflows](exploitation/stack-buffer-overflows.md) | T1068 |
| [Return-Oriented Programming (ROP)](exploitation/return-oriented-programming.md) | T1068 |
| [Driver Signature Enforcement and BYOVD](exploitation/driver-signature-enforcement-and-byovd.md) | T1068, T1553.006, T1211 |

## Rootkits

| Concept | ATT&CK |
|---|---|
| [Rootkits Overview (User-Mode vs Kernel-Mode)](rootkits/rootkits-overview.md) | T1014 |
| [User-Mode Rootkits](rootkits/user-mode-rootkits.md) | T1014, T1564 |
| [Kernel-Mode Rootkits](rootkits/kernel-mode-rootkits.md) | T1014, T1547.006 |
| [Direct Kernel Object Manipulation (DKOM)](rootkits/direct-kernel-object-manipulation.md) | T1014 |
| [Bootkits](rootkits/bootkits.md) | T1542.003, T1014 |
| [UEFI Rootkits and Firmware Implants](rootkits/uefi-rootkits-and-firmware-implants.md) | T1542.001, T1542.002, T1014 |
| [Secure Boot and UEFI Security](rootkits/secure-boot-and-uefi-security.md) | T1542.001, T1542.003 |
| [Hidden Processes and Artifacts](rootkits/hidden-processes-and-artifacts.md) | T1564, T1014, T1564.001 |
| [Hidden File Systems and Covert Storage](rootkits/hidden-file-systems-and-covert-storage.md) | T1564.005, T1564.004, T1014 |
| [Rootkit Detection (Memory Forensics, Cross-View, ELAM)](rootkits/rootkit-detection-and-elam.md) | T1014 |

## Forensics

| Concept | ATT&CK |
|---|---|
| [Windows Event Logs and Security Auditing](forensics/windows-event-logs-and-security-auditing.md) | T1078, T1685.005 |
| [Registry Forensic Artifacts](forensics/registry-forensic-artifacts.md) | T1547.001, T1012 |
| [Timeline Analysis and Execution Artifacts](forensics/timeline-analysis-and-execution-artifacts.md) | T1070.006, T1070 |
| [Memory Forensics (RAM Analysis and Volatility)](forensics/memory-forensics-and-volatility.md) | T1055, T1620, T1014 |
| [Live Response and Triage Collection](forensics/live-response-and-triage-collection.md) | — |
| [Reverse Engineering Tooling (Ghidra, IDA, WinDbg, x86/x64)](forensics/reverse-engineering-tooling.md) | T1027, T1620 |

## Detection

| Concept | ATT&CK |
|---|---|
| [Detection Engineering and Data Sources (Sysmon, ETW)](detection/detection-engineering-and-data-sources.md) | — |
| [Indicators of Compromise and Detection Logic](detection/indicators-of-compromise-and-detection-logic.md) | — |
| [Antivirus Engine Architecture](detection/antivirus-engine-architecture.md) | T1027, T1685 |
| [Endpoint Detection and Response (EDR)](detection/endpoint-detection-and-response.md) | T1685, T1211 |
| [Threat Hunting on Windows](detection/threat-hunting-on-windows.md) | — |
| [Adversary Emulation and Purple Teaming](detection/adversary-emulation-and-purple-teaming.md) | — |

## Hardening

| Concept | ATT&CK |
|---|---|
| [Windows Security Baselines and Hardening](hardening/windows-security-baselines-and-hardening.md) | — |
| [Application Control (WDAC and AppLocker)](hardening/application-control-wdac-and-applocker.md) | T1059, T1218, T1204.002 |
| [Attack Surface Reduction and Exploit Protection](hardening/attack-surface-reduction-and-exploit-protection.md) | T1204.002, T1055, T1068 |
| [Credential Protection (Credential Guard and LSASS Hardening)](hardening/credential-protection-credential-guard-and-lsass.md) | T1003.001, T1550.002 |
| [Virtualization-Based Security (VBS, HVCI, Device Guard)](hardening/virtualization-based-security-hvci-and-device-guard.md) | T1068, T1014 |
| [Privileged Access Management and Administrative Tiering](hardening/privileged-access-management-and-tiering.md) | T1078, T1550.002 |
