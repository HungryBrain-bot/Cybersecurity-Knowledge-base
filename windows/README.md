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
| [Advanced Local Procedure Call (ALPC) and Windows Notification Facility](internals/alpc-and-windows-notification-facility.md) | — |
| [Windows Image Loader and API Sets](internals/image-loader-and-api-sets.md) | T1574.001, T1129, T1622 |
| [Windows Kernel Deferred Execution (APCs, DPCs, Work Items, Timers, Callbacks)](internals/kernel-deferred-execution-apcs-dpcs-work-items.md) | T1055.004, T1014 |
| [Windows Kernel Driver Structure and IRP Handling](internals/kernel-driver-structure-and-irp-handling.md) | T1014, T1543.003, T1068 |
| [Thread Scheduling and Context Switching](internals/thread-scheduling-and-context-switching.md) | T1055.003, T1497.003 |
| [Windows Trap, Interrupt, Exception and System Service Dispatching](internals/trap-interrupt-exception-and-system-service-dispatching.md) | T1014, T1106 |
| [Windows Cache Manager and File Systems (NTFS, ReFS, Filter Drivers)](internals/cache-manager-and-file-systems.md) | T1564.004, T1006, T1685 |
| [CPU Side-Channel Vulnerabilities and Windows Mitigations (KVA Shadow, Retpoline)](internals/cpu-side-channel-vulnerabilities-and-windows-mitigations.md) | — |
| [Windows Hypervisor and Virtualization Stack (Hyper-V)](internals/hyper-v-hypervisor-and-virtualization-stack.md) | T1611, T1564.006 |
| [Windows Job Objects and Containers (Server Silos)](internals/job-objects-and-server-silos.md) | T1611 |
| [Protected Processes (PPL), Pico Processes and Trustlets](internals/protected-processes-ppl-pico-and-trustlets.md) | T1003.001, T1685, T1068 |
| [WoW64 (32-bit on 64-bit, Redirection and Emulation)](internals/wow64-32-bit-on-64-bit.md) | T1106, T1055 |

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
| [Vulnerabilities in Security Software (AV Self-Protection)](exploitation/vulnerabilities-in-security-software.md) | T1068, T1685, T1574.009, T1574.001, T1203 |
| [Kernel Exploitation on UNIX, macOS and Windows](exploitation/kernel-exploitation-on-unix-macos-and-windows.md) | T1068 |
| [UEFI Firmware Vulnerabilities (SMM, S3 Boot Script, SPI Flash, Intel ME)](exploitation/uefi-firmware-vulnerabilities.md) | T1542.001, T1068 |

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
| [Hardware and Firmware Rootkit Techniques (Keyboard Controller, Microcode)](rootkits/hardware-and-firmware-rootkit-techniques.md) | T1542.001, T1542.002, T1056.001, T1014 |
| [Rootkit Covert Channels and Network Command and Control](rootkits/rootkit-covert-channels-and-network-c2.md) | T1095, T1205, T1001, T1041, T1014 |

## Reverse engineering

| Concept | ATT&CK |
|---|---|
| [Analyzing Malicious DLLs (Exports, rundll32, Host Processes)](reverse-engineering/analyzing-malicious-dlls.md) | T1218.011, T1129, T1574.001 |
| [Debugging Malware (Breakpoints and Stepping)](reverse-engineering/debugging-malware-breakpoints-and-stepping.md) | T1622 |
| [Deciphering File Formats and Undocumented Software Interfaces](reverse-engineering/deciphering-file-formats-and-undocumented-interfaces.md) | T1001, T1027 |
| [PowerShell for Windows Security Research (NtObjectManager)](reverse-engineering/powershell-ntobjectmanager-for-security-research.md) | T1106, T1059.001 |
| [Recognizing C Code Constructs in Assembly](reverse-engineering/recognizing-c-code-constructs-in-assembly.md) | — |
| [Reversing Bytecode Malware (.NET, Visual Basic, Java, Python)](reverse-engineering/reversing-bytecode-malware.md) | T1027, T1140, T1059.006 |
| [Software Piracy, Copy Protection and Cracking](reverse-engineering/software-piracy-copy-protection-and-cracking.md) | T1622, T1027.002 |

## Malware analysis

| Concept | ATT&CK |
|---|---|
| [Encryption Identification in Malware (RC4, CryptoAPI, X-Raying)](malware-analysis/encryption-identification-in-malware.md) | T1027, T1140, T1573.001 |
| [Malicious Document and Web Exploit Analysis (JavaScript, PDF, Office)](malware-analysis/malicious-document-and-web-exploit-analysis.md) | T1566.001, T1204.002, T1203, T1189, T1059.007 |
| [Malicious Script Deobfuscation (VBS, PowerShell, JavaScript)](malware-analysis/malicious-script-deobfuscation.md) | T1027.010, T1140, T1059.001, T1059.005, T1059.007 |
| [Malware Identification and Manual Removal Procedures](malware-analysis/malware-identification-and-manual-removal.md) | T1547.001, T1543.003, T1053.005, T1014 |
| [MBR Ransomware (Petya and Satana)](malware-analysis/mbr-ransomware-petya-and-satana.md) | T1486, T1561.002, T1542.003, T1529, T1053.005 |
| [Mutexes as Malware Infection Markers](malware-analysis/mutexes-as-malware-infection-markers.md) | T1480.002 |
| [Post-Mortem Malware Forensics on Windows Systems](malware-analysis/post-mortem-malware-forensics-on-windows.md) | T1547.001, T1543.003, T1053.005, T1036 |
| [Process and Registry Monitoring (Procmon, Process Explorer, Regshot)](malware-analysis/process-and-registry-monitoring.md) | T1112, T1547.001, T1055 |
| [System and Network Discovery by Malware](malware-analysis/system-and-network-discovery-by-malware.md) | T1082, T1016, T1018, T1033, T1069.002, T1518.001, T1135 |
| [Typical Malware Behaviors and Installation Footprints](malware-analysis/typical-malware-behaviors-and-installation-footprints.md) | T1547.001, T1543.003, T1070.006, T1055, T1685, T1112, T1056.001 |
| [Vulnerability Types and Exploit Analysis in Malware](malware-analysis/vulnerability-types-and-exploit-analysis.md) | T1203, T1189, T1068 |

## Forensics

| Concept | ATT&CK |
|---|---|
| [Windows Event Logs and Security Auditing](forensics/windows-event-logs-and-security-auditing.md) | T1078, T1685.005 |
| [Registry Forensic Artifacts](forensics/registry-forensic-artifacts.md) | T1547.001, T1012 |
| [Timeline Analysis and Execution Artifacts](forensics/timeline-analysis-and-execution-artifacts.md) | T1070.006, T1070 |
| [Memory Forensics (RAM Analysis and Volatility)](forensics/memory-forensics-and-volatility.md) | T1055, T1620, T1014 |
| [Live Response and Triage Collection](forensics/live-response-and-triage-collection.md) | — |
| [Reverse Engineering Tooling (Ghidra, IDA, WinDbg, x86/x64)](forensics/reverse-engineering-tooling.md) | T1027, T1620 |
| [BIOS/UEFI Firmware Forensics](forensics/bios-uefi-firmware-forensics.md) | T1542.001, T1495 |
| [Command History Recovery](forensics/command-history-recovery.md) | T1059.003, T1059.001, T1059.004, T1070.003 |
| [Disk Partitions and File System Forensics (MBR, GPT, FAT, NTFS)](forensics/disk-partitions-and-file-system-forensics.md) | T1006, T1564.005, T1070.004 |
| [Encryption Key Recovery from Memory](forensics/encryption-key-recovery-from-memory.md) | T1555.001, T1003 |
| [Investigating Windows and Unix Systems](forensics/investigating-windows-and-unix-systems.md) | T1543.003, T1053.005, T1053.003, T1548.001, T1014 |
| [Log Analysis for Incident Investigation](forensics/log-analysis-for-incident-investigation.md) | T1190, T1685.005, T1071 |
| [Network Artifacts in Memory](forensics/network-artifacts-in-memory.md) | T1071, T1040, T1014 |

## Detection

| Concept | ATT&CK |
|---|---|
| [Detection Engineering and Data Sources (Sysmon, ETW)](detection/detection-engineering-and-data-sources.md) | — |
| [Indicators of Compromise and Detection Logic](detection/indicators-of-compromise-and-detection-logic.md) | — |
| [Antivirus Engine Architecture](detection/antivirus-engine-architecture.md) | T1027, T1685 |
| [Endpoint Detection and Response (EDR)](detection/endpoint-detection-and-response.md) | T1685, T1211 |
| [Threat Hunting on Windows](detection/threat-hunting-on-windows.md) | — |
| [Adversary Emulation and Purple Teaming](detection/adversary-emulation-and-purple-teaming.md) | — |
| [Active Directory Object Auditing (SACL and Change Events)](detection/active-directory-object-auditing.md) | T1003.006, T1484.001, T1098, T1134.005 |
| [Defender Quick Reference: Identify, Protect, Detect, Respond, Recover](detection/defender-quick-reference.md) | — |
| [Filesystem, Removable Storage and Registry Auditing](detection/filesystem-removable-storage-and-registry-auditing.md) | T1052.001, T1091, T1112, T1547.001, T1003.002 |
| [Monitoring Logon Rights and User Privileges](detection/monitoring-logon-rights-and-user-privileges.md) | T1134, T1078, T1003.002 |
| [Monitoring Network File Shares and Named Pipes](detection/monitoring-network-file-shares-and-named-pipes.md) | T1021.002, T1569.002, T1570, T1039, T1135, T1070.005 |
| [Monitoring Operating System Events (Log Clearing, Services, Time, BCD)](detection/monitoring-operating-system-events.md) | T1685.005, T1685.001, T1543.003, T1053.005, T1553.006 |
| [Security Logging and Monitoring Policy](detection/security-logging-and-monitoring-policy.md) | T1685.001, T1685.005 |
| [Hunting Quasar RAT and Common Tactics with Atomic Tests](detection/hunting-quasar-rat-and-common-tactics.md) | T1547.001, T1053.005, T1548.002, T1056.001, T1555.003, T1573.001, T1090, T1564.001, T1082, T1016, T1105 |
| [Microsoft Defender ATP, Azure Monitor and Sentinel for Security Operations](detection/microsoft-defender-and-sentinel-for-security-operations.md) | — |
| [Threat Hunting Research Lab (Active Directory, ELK, HELK, Sysmon)](detection/threat-hunting-research-lab.md) | — |

## Hardening

| Concept | ATT&CK |
|---|---|
| [Windows Security Baselines and Hardening](hardening/windows-security-baselines-and-hardening.md) | — |
| [Application Control (WDAC and AppLocker)](hardening/application-control-wdac-and-applocker.md) | T1059, T1218, T1204.002 |
| [Attack Surface Reduction and Exploit Protection](hardening/attack-surface-reduction-and-exploit-protection.md) | T1204.002, T1055, T1068 |
| [Credential Protection (Credential Guard and LSASS Hardening)](hardening/credential-protection-credential-guard-and-lsass.md) | T1003.001, T1550.002 |
| [Virtualization-Based Security (VBS, HVCI, Device Guard)](hardening/virtualization-based-security-hvci-and-device-guard.md) | T1068, T1014 |
| [Privileged Access Management and Administrative Tiering](hardening/privileged-access-management-and-tiering.md) | T1078, T1550.002 |
| [Azure AD Identity Security (SSO, MFA, Windows Hello, Conditional Access)](hardening/entra-id-identity-security.md) | T1078.004, T1110, T1621, T1556.006 |
| [Host Intrusion Prevention and Personal Firewalls](hardening/host-intrusion-prevention-and-personal-firewalls.md) | T1055, T1685, T1071 |
| [Micro-Virtualization and Application Isolation Against Advanced Malware](hardening/micro-virtualization-and-application-isolation.md) | T1189, T1566.001, T1203, T1611 |
| [Windows Network Hardening (Firewall, Exploit Guard, Name Resolution Poisoning)](hardening/windows-network-hardening.md) | T1557.001, T1021.002, T1071 |
