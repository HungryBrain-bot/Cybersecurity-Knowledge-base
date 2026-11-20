# General

Cross-cutting security knowledge that is not tied to a single platform: cryptography, identity and access management, threat intelligence and hunting, incident response, digital forensics, offensive operations and OPSEC, malware analysis, reverse engineering, and security program fundamentals. Content is curated and written fresh from a personal knowledge base, cross-checked against [MITRE ATT&CK](https://attack.mitre.org/) and primary/vendor documentation.

Each file carries a `verification` front-matter block listing the primary sources it was checked against.

**Scope note:** these are concepts whose *primary* domain is General. Platform-specific techniques (Windows, Linux, macOS, malware internals, network, web, cloud) live under their own domain folders.

## Cryptography

| Concept | ATT&CK |
|---|---|
| [Bitcoin, Blockchains and Ethereum](cryptography/bitcoin-blockchains-and-ethereum.md) | — |
| [Block Ciphers, AES and Modes of Operation](cryptography/block-ciphers-aes-and-modes-of-operation.md) | — |
| [Cryptographic Hash Functions](cryptography/cryptographic-hash-functions.md) | — |
| [Encryption Security Notions and Cryptographic Security Levels](cryptography/encryption-security-notions-and-levels.md) | — |
| [MACs and Authenticated Encryption](cryptography/macs-and-authenticated-encryption.md) | — |
| [Public-Key Certificate Management and PKI Use Cases](cryptography/public-key-certificate-management-and-pki.md) | — |
| [Public-Key Cryptography (Hard Problems, RSA, Diffie-Hellman, Elliptic Curves)](cryptography/public-key-cryptography.md) | — |
| [Randomness, PRNGs and Entropy](cryptography/randomness-prngs-and-entropy.md) | — |
| [Stream Ciphers](cryptography/stream-ciphers.md) | — |

## Identity and Access Management

| Concept | ATT&CK |
|---|---|
| [Identity and Access Management Foundations (Five A's)](identity-and-authentication/iam-foundations-five-as.md) | — |
| [Identity Governance and Regulatory Compliance](identity-and-authentication/identity-governance-and-regulatory-compliance.md) | — |
| [Identity Obfuscation, SCIM, Remote Access and Emerging Identity Risks](identity-and-authentication/identity-obfuscation-scim-remote-access.md) | — |
| [Just-in-Time Access and Identity Program Planning](identity-and-authentication/just-in-time-access-and-program-planning.md) | — |
| [Multifactor Authentication Attack Techniques](identity-and-authentication/multifactor-authentication-attack-techniques.md) | T1621, T1111 |
| [Selecting and Evaluating MFA Solutions](identity-and-authentication/selecting-and-evaluating-mfa-solutions.md) | — |

## Credential Access

| Concept | ATT&CK |
|---|---|
| [Identity Attack Vectors and Identity-Based Threat Response](credential-access/identity-attack-vectors-and-threat-response.md) | — |
| [Password Cracking and Tool Automation](credential-access/password-cracking-and-tool-automation.md) | T1110, T1110.002 |
| [Passwords, Email and Wiretapping Privacy Risks](credential-access/passwords-email-and-wiretapping-privacy-risks.md) | — |

## Threat Intelligence

| Concept | ATT&CK |
|---|---|
| [Cyber Kill Chain](threat-intelligence/cyber-kill-chain.md) | — |
| [Cyber Threat Collection Sources (OSINT, Honeypots, Sandboxing)](threat-intelligence/cyber-threat-collection-sources.md) | — |
| [Diamond Model of Intrusion Analysis](threat-intelligence/diamond-model-of-intrusion-analysis.md) | — |
| [Intelligence Basics and the Intelligence Cycle](threat-intelligence/intelligence-basics-and-the-intelligence-cycle.md) | — |
| [MITRE ATT&CK Framework](threat-intelligence/mitre-attack-framework.md) | — |
| [Strategic Intelligence and Building an Intelligence Program](threat-intelligence/strategic-intelligence-and-building-a-program.md) | — |
| [Threat Intelligence Data Standards and Platforms](threat-intelligence/threat-intelligence-data-standards-and-platforms.md) | — |

## Threat Hunting

| Concept | ATT&CK |
|---|---|
| [Adversary Emulation (Atomic Red Team, CALDERA, Mordor)](threat-hunting/adversary-emulation.md) | — |
| [Data Quality and Hunting Program Metrics](threat-hunting/data-quality-and-hunting-program-metrics.md) | — |
| [Documenting and Automating Hunts](threat-hunting/documenting-and-automating-hunts.md) | — |
| [Threat Hunting Methodology (Hypothesis, Pyramid of Pain, Maturity Model)](threat-hunting/threat-hunting-methodology.md) | — |

## Detection Engineering

| Concept | ATT&CK |
|---|---|
| [Sigma Rules and MITRE CAR Analytics](detection/sigma-rules-and-mitre-car-analytics.md) | — |

## Incident Response

| Concept | ATT&CK |
|---|---|
| [Active Defense (Deny, Disrupt, Degrade, Deceive, Destroy)](incident-response/active-defense.md) | — |
| [F3EAD (Find, Fix, Finish, Exploit, Analyze, Disseminate)](incident-response/f3ead.md) | — |
| [Forensic Report Writing](incident-response/forensic-report-writing.md) | — |
| [Incident Response Cycle](incident-response/incident-response-cycle.md) | — |
| [Incident Response Methodology and Process](incident-response/incident-response-methodology-and-process.md) | — |
| [Initial Response and Incident Scoping](incident-response/initial-response-and-incident-scoping.md) | — |
| [Pre-Incident Preparation](incident-response/pre-incident-preparation.md) | — |

## Digital Forensics

| Concept | ATT&CK |
|---|---|
| [Evidence Handling and Chain of Custody](forensics/evidence-handling-and-chain-of-custody.md) | — |
| [Forensic Data Analysis Techniques](forensics/forensic-data-analysis-techniques.md) | — |
| [Forensic Duplication and Disk Imaging](forensics/forensic-duplication-and-disk-imaging.md) | — |
| [Storage Fundamentals and File System Layers for Forensics](forensics/storage-fundamentals-and-file-system-layers.md) | — |

## Security Foundations, Reconnaissance and Investigations

| Concept | ATT&CK |
|---|---|
| [APT Attribution Process and Methods](discovery-and-recon/apt-attribution-process-and-methods.md) | — |
| [Information Processing, Attack Strategy and Psychology in Attacks](discovery-and-recon/attack-psychology-and-strategy.md) | — |
| [Attribution Pitfalls: False Flags, Doxing, Geopolitics and Ethics](discovery-and-recon/attribution-pitfalls-false-flags-and-ethics.md) | — |
| [Computer Security Concepts and Principles](discovery-and-recon/computer-security-concepts-and-principles.md) | — |
| [Data Traces, Secure Deletion and Data Recovery](discovery-and-recon/data-traces-secure-deletion-and-data-recovery.md) | — |
| [Email Forensics](discovery-and-recon/email-forensics.md) | — |
| [Expert Witness Ethics and Testimony](discovery-and-recon/expert-witness-ethics-and-testimony.md) | — |
| [Forensic Workstation, Response Kit and Tool Validation](discovery-and-recon/forensic-workstation-response-kit-and-tool-validation.md) | — |
| [Hacker Types, Cracking versus Hacking and Learning Path](discovery-and-recon/hacker-types-and-learning-path.md) | — |
| [Industry Compliance Standards and Frameworks](discovery-and-recon/industry-compliance-standards-and-frameworks.md) | — |
| [Integrating Cyber Intelligence with Security Operations](discovery-and-recon/integrating-cyber-intelligence-with-security-operations.md) | — |
| [Internet and Cloud Artifacts Forensics](discovery-and-recon/internet-and-cloud-artifacts-forensics.md) | — |
| [Open-Source Intelligence Gathering on a Target (GitHub, Certificates, Web Infrastructure)](discovery-and-recon/open-source-intelligence-gathering-on-a-target.md) | T1593, T1596 |
| [OSCP Course Methodology (Kali, Scripting, Enumeration, Tunneling)](discovery-and-recon/oscp-course-methodology.md) | — |
| [Patch and Configuration Management](discovery-and-recon/patch-and-configuration-management.md) | — |
| [Penetration Test Reporting and Continuing Education](discovery-and-recon/penetration-test-reporting-and-continuing-education.md) | — |
| [Penetration Tester Command-Line Quick Reference](discovery-and-recon/penetration-tester-command-line-quick-reference.md) | — |
| [AWS and API Penetration Testing Cheat Sheet](discovery-and-recon/aws-and-api-pentesting-cheat-sheet.md) | — |
| [Purple Teaming](discovery-and-recon/purple-teaming.md) | — |
| [Red Team Reconnaissance and Environment Monitoring](discovery-and-recon/red-team-reconnaissance-and-environment-monitoring.md) | T1595, T1590 |
| [Scientific Method and Experimental Research in Cyber Security](discovery-and-recon/scientific-method-and-experimental-research.md) | — |
| [Security Policies, Standards and Procedures](discovery-and-recon/security-policies-standards-and-procedures.md) | — |
| [Security Posture and the Assume-Breach Mindset](discovery-and-recon/security-posture-and-assume-breach.md) | — |
| [Security Program Creation and Asset Management](discovery-and-recon/security-program-creation-and-asset-management.md) | — |
| [Security Stack and Intelligence-Driven Defense](discovery-and-recon/security-stack-and-intelligence-driven-defense.md) | — |
| [Seven Habits of Highly Effective Security](discovery-and-recon/seven-habits-of-highly-effective-security.md) | — |
| [Shinobi-Inspired Security Principles (Zero Trust, Insider Threat, Hiring)](discovery-and-recon/shinobi-inspired-security-principles.md) | — |
| [Staying Protected Against Attackers (Individual and Business)](discovery-and-recon/staying-protected-against-attackers.md) | — |
| [The Attacker Mindset Framework and Laws](discovery-and-recon/the-attacker-mindset-framework-and-laws.md) | — |
| [Threat Hunting Maturity Model and Team Structure](discovery-and-recon/threat-hunting-maturity-model-and-team-structure.md) | — |
| [Types of Computer-Based Investigations](discovery-and-recon/types-of-computer-based-investigations.md) | — |
| [Vulnerability Management Programs](discovery-and-recon/vulnerability-management-programs.md) | — |
| [Web Tracking, Location and Device Surveillance](discovery-and-recon/web-tracking-location-and-device-surveillance.md) | — |

## Exploitation and Offensive Operations

| Concept | ATT&CK |
|---|---|
| [Building and Managing an Offensive Security Program](exploitation/building-and-managing-an-offensive-security-program.md) | — |
| [Client-Side Attacks, Public Exploits and File Transfers](exploitation/client-side-attacks-public-exploits-and-file-transfers.md) | T1203, T1105 |
| [Exploiting Scanner Findings with Metasploit](exploitation/exploiting-scanner-findings-with-metasploit.md) | T1210, T1203 |
| [Hacking Android Apps](exploitation/hacking-android-apps.md) | — |
| [Kernel Exploitation Fundamentals and Vulnerability Taxonomy](exploitation/kernel-exploitation-fundamentals.md) | T1068 |
| [Mainframe Hacking](exploitation/mainframe-hacking.md) | — |
| [Measuring a Red Team Program and Progressive Operations](exploitation/measuring-a-red-team-program.md) | — |
| [Mobile, Embedded and Firmware Security Research](exploitation/mobile-embedded-and-firmware-security-research.md) | — |
| [Ninjutsu-Inspired Penetration Testing Strategy](exploitation/ninjutsu-inspired-penetration-testing-strategy.md) | — |
| [Penetration Testing Lab and Attack Box Setup](exploitation/penetration-testing-lab-and-attack-box-setup.md) | — |

## Initial Access

| Concept | ATT&CK |
|---|---|
| [Gamified Security Awareness Training and Habit Building](initial-access/gamified-security-awareness-training.md) | — |
| [Malware Propagation and Infection Vectors](initial-access/malware-propagation-and-infection-vectors.md) | T1566, T1189 |
| [Physical Security Attacks (Cloners, LAN Turtle, Bash Bunny)](initial-access/physical-security-attacks.md) | T1200, T1091 |
| [Red Team Campaign Setup and Assumed Breach Exercises](initial-access/red-team-campaign-setup-and-assumed-breach.md) | — |
| [Root Causes of Data Breaches](initial-access/root-causes-of-data-breaches.md) | — |
| [Security Awareness and User Education](initial-access/security-awareness-and-user-education.md) | — |

## Social Engineering

| Concept | ATT&CK |
|---|---|
| [Phishing and Social Engineering Campaigns](social-engineering/phishing-and-social-engineering-campaigns.md) | T1566, T1598 |

## Command and Control

| Concept | ATT&CK |
|---|---|
| [Attack Infrastructure and Containerized Tooling](command-and-control/attack-infrastructure-and-containerized-tooling.md) | T1583, T1071 |
| [Command and Control Frameworks (Merlin, Koadic, SILENTTRINITY)](command-and-control/command-and-control-frameworks.md) | T1071, T1105 |

## OPSEC, Anonymity and Evasion

| Concept | ATT&CK |
|---|---|
| [Anonymous Operating Systems, Online Identity and Payments](defense-evasion/anonymous-operating-systems-identity-and-payments.md) | — |
| [Attacker Operational Security and Anonymity](defense-evasion/attacker-operational-security-and-anonymity.md) | — |
| [Maintaining Anonymity Against Investigators](defense-evasion/maintaining-anonymity-against-investigators.md) | — |
| [Protecting the Pen Tester (Operational Security for Red Teams)](defense-evasion/protecting-the-pen-tester-red-team-opsec.md) | — |
| [Shinobi-Inspired Attack Tradecraft (Covert Communication and Infiltration)](defense-evasion/shinobi-inspired-attack-tradecraft.md) | — |
| [Traps, Deception and Honeypots Against Attackers](defense-evasion/traps-deception-and-honeypots.md) | — |
| [Virtual Machine Rootkits (Hypervisor and VM-Based)](defense-evasion/virtual-machine-rootkits.md) | T1014, T1564.006 |

## Malware Analysis

| Concept | ATT&CK |
|---|---|
| [Android Malware Analysis](malware-analysis/android-malware-analysis.md) | — |
| [Enterprise Malware Defense (Layered Controls and Response)](malware-analysis/enterprise-malware-defense.md) | — |
| [Legal Considerations in Malware Investigations](malware-analysis/legal-considerations-in-malware-investigations.md) | — |
| [Malware Taxonomy and Types (Virus, Worm, Trojan, Ransomware, Adware)](malware-analysis/malware-taxonomy-and-types.md) | — |
| [Malware Threat Ecosystem and Malware Factory (Crimeware Economy)](malware-analysis/malware-threat-ecosystem-and-crimeware-economy.md) | — |
| [Neural Network Architectures for Security (ML in Malware and Detection)](malware-analysis/neural-network-architectures-for-security.md) | — |
| [Notable Malware Families, Source Leaks and Ransomware](malware-analysis/notable-malware-families-and-source-leaks.md) | — |

## Reverse Engineering

| Concept | ATT&CK |
|---|---|
| [Code Obfuscation and Deobfuscation](reverse-engineering/code-obfuscation-and-deobfuscation.md) | T1027 |
| [CPU Architectures and Calling Conventions](reverse-engineering/cpu-architectures-and-calling-conventions.md) | — |

## Network Security

| Concept | ATT&CK |
|---|---|
| [CIS Controls and Blue-Team Tool Selection](network-security/cis-controls-v7-and-blue-team-tool-selection.md) | — |

## Impact, Breaches and Recovery

| Concept | ATT&CK |
|---|---|
| [Disaster Recovery and Business Continuity](impact/disaster-recovery-and-business-continuity.md) | — |
| [Major Breach Case Studies (Capital One, Marriott, Equifax, OPM, Yahoo, Target, JPMorgan)](impact/major-breach-case-studies.md) | — |

---

**111 concepts** across 18 categories.

