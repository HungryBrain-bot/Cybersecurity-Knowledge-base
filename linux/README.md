# Linux

Linux fundamentals, binary analysis, offensive technique, hardening, and defensive forensics: how the system works, how it is attacked, how it is hardened, and how intrusions are detected and investigated. Content is curated and written fresh from the knowledge base, cross-checked against [MITRE ATT&CK](https://attack.mitre.org/), Linux [man pages](https://man7.org/linux/man-pages/), [kernel.org](https://www.kernel.org/doc/html/latest/), and relevant RFCs and project documentation.

Each file carries a `verification` front-matter block listing the primary sources it was checked against.

## Fundamentals

| Concept | ATT&CK |
|---|---|
| [Linux Command-Line Fundamentals for Security Work](fundamentals/linux-command-line-fundamentals.md) | — |
| [Linux Filesystem Management and Software Installation](fundamentals/linux-filesystem-and-software-installation.md) | — |
| [Linux Process, Service and Network Management Basics](fundamentals/linux-process-service-and-network-management.md) | T1057, T1049 |

## Scripting

| Concept | ATT&CK |
|---|---|
| [Bash Scripting for Security Work](scripting/bash-scripting-for-security.md) | T1059.004 |
| [Python Scripting for Security Tools](scripting/python-scripting-for-security-tools.md) | T1059.006 |
| [Text Processing with sed, awk and Regular Expressions](scripting/text-processing-sed-awk-regex.md) | — |

## Binary analysis

| Concept | ATT&CK |
|---|---|
| [Anatomy of a Binary (Compilation, Symbols, Stripping, Loading)](binary-analysis/anatomy-of-a-binary.md) | — |
| [ELF Binary Format (Segments, Sections, Symbols, Relocations)](binary-analysis/elf-binary-format.md) | — |
| [Linux Binary Analysis Toolchain (objdump, readelf, strace, ltrace, ERESI)](binary-analysis/linux-binary-analysis-toolchain.md) | — |
| [Disassembly Approaches (Linear vs Recursive)](binary-analysis/disassembly-approaches.md) | — |
| [GOT/PLT Hooking](binary-analysis/got-plt-hooking.md) | T1574.006, T1055 |
| [Binary Instrumentation (Pin)](binary-analysis/binary-instrumentation-pin.md) | — |
| [Dynamic Taint Analysis](binary-analysis/dynamic-taint-analysis.md) | — |
| [Symbolic Execution (Triton)](binary-analysis/symbolic-execution-triton.md) | — |
| [ELF Binary Forensics (Detecting Infection and Hooks)](binary-analysis/elf-binary-forensics.md) | T1027.002, T1574.006 |

## Privilege escalation

| Concept | ATT&CK |
|---|---|
| [Linux File Permissions, SUID/SGID and Privilege Escalation](privilege-escalation/file-permissions-suid-sgid.md) | T1548.001 |
| [Linux ACLs and Shared Directory Management](privilege-escalation/linux-acls-and-shared-directories.md) | T1222.002 |

## Persistence

| Concept | ATT&CK |
|---|---|
| [Cron Job Scheduling and Persistence](persistence/cron-job-persistence.md) | T1053.003 |
| [LD_PRELOAD Hijacking](persistence/ld-preload-hijacking.md) | T1574.006 |

## Malware analysis

| Concept | ATT&CK |
|---|---|
| [Analyzing Bind and Reverse Shell Binaries on Linux](malware/analyzing-bind-and-reverse-shell-binaries.md) | T1059.004, T1571 |
| [ELF Binary Packers and Protectors](malware/elf-binary-packers-and-protectors.md) | T1027.002 |
| [ELF Virus and Parasite Infection Techniques](malware/elf-virus-and-parasite-infection.md) | T1027.002 |
| [Linux and IoT Malware Analysis](malware/linux-and-iot-malware-analysis.md) | T1059.004, T1498 |
| [Linux Kernel Rootkits (LKM)](malware/linux-kernel-rootkits-lkm.md) | T1014, T1547.006 |
| [Linux Shared Library and Code Injection (.so, ET_REL)](malware/linux-shared-library-and-code-injection.md) | T1055.001, T1055.009 |
| [Linux Process Tracing with ptrace](malware/linux-process-tracing-ptrace.md) | T1055.008 |

## Hardening and defense

| Concept | ATT&CK |
|---|---|
| [Linux Security Policy and Hardening Basics](hardening/linux-security-policy-and-hardening-basics.md) | — |
| [Linux User Authentication, PAM and sudo](hardening/user-authentication-pam-and-sudo.md) | T1548.003, T1556.003 |
| [Linux Firewalls and Network Hardening (iptables, TCP Wrappers)](hardening/firewalls-and-network-hardening.md) | — |
| [Linux Kernel Configuration and Hardening](hardening/kernel-configuration-and-hardening.md) | T1068, T1014 |
| [SELinux and Mandatory Access Control](hardening/selinux-and-mandatory-access-control.md) | — |
| [SSH Hardening and Remote Access](hardening/ssh-hardening-and-remote-access.md) | T1021.004, T1110 |
| [Linux Encryption Tools (GPG, LUKS, eCryptfs, VeraCrypt)](hardening/encryption-tools.md) | — |
| [Database Server Hardening and Encryption](hardening/database-server-hardening.md) | — |
| [DNS Server Security (DNSSEC, Amplification and Rate Limiting)](hardening/dns-server-security.md) | T1498.002, T1565.001 |
| [Email Server Hardening (Authentication and Encryption)](hardening/email-server-hardening.md) | T1566 |
| [Linux Service Security (Web, FTP, Mail, Remote Shell)](hardening/linux-service-security.md) | T1190 |
| [Qubes OS and Workstation Hardening (Compartmentalization)](hardening/qubes-os-and-workstation-hardening.md) | — |
| [Security-Focused Distributions and the Shellshock Lesson](hardening/security-focused-distros-and-shellshock.md) | T1190, T1059.004 |
| [Linux Vulnerability Scanning and Compliance Auditing](hardening/linux-vulnerability-scanning-and-compliance.md) | — |

## Detection and monitoring

| Concept | ATT&CK |
|---|---|
| [Linux Auditing with auditd](detection/linux-auditing-with-auditd.md) | T1070 |
| [Linux Intrusion Detection Tools (HIDS, File Integrity, Rootkit Scanners)](detection/linux-intrusion-detection-tools.md) | T1014, T1070 |
| [Linux Log Management and Log Evasion](detection/linux-log-management-and-log-evasion.md) | T1070, T1685.006, T1070.003 |

## Forensics

| Concept | ATT&CK |
|---|---|
| [Live Data Collection from Unix/Linux Systems](forensics/live-data-collection-from-unix.md) | T1070 |
| [Linux Process Memory Forensics](forensics/linux-process-memory-forensics.md) | T1055, T1620 |
| [Post-Mortem Malware Forensics on Linux](forensics/post-mortem-malware-forensics.md) | T1070, T1222.002 |
| [Linux Kernel Rootkit Forensics](forensics/linux-kernel-rootkit-forensics.md) | T1014, T1547.006 |

## Anonymity, exploitation and recon

| Concept | ATT&CK |
|---|---|
| [Anonymity Tools on Linux (Tor, Proxychains, VPNs)](anonymity/anonymity-tools-tor-and-proxychains.md) | T1090, T1090.003 |
| [Remote Kernel Exploitation (Linux)](exploitation/remote-kernel-exploitation.md) | T1068, T1203 |
| [Wireless Network Reconnaissance and Attacks](recon/wireless-network-recon-and-attacks.md) | T1040, T1557 |
