# Network

Network security: how networks are mapped and attacked at Layer 2 and above, how protocols are analysed and exploited, and how networks are segmented, monitored and defended. Content is curated and written fresh from the knowledge base, cross-checked against [MITRE ATT&CK](https://attack.mitre.org/) and primary protocol and vendor references (IETF RFCs, NIST, Snort/Suricata, Wireshark, Tor Project).

Each file carries a `verification` front-matter block listing the primary sources it was checked against.

> Note: cross-listed concepts are filed under their primary domain. Network authentication protocols (NTLM, Kerberos) and credential-capture tooling (Responder, NTLM relay) are filed under **Windows**; SSH, iptables and Linux IDS under **Linux**; botnets and C2 frameworks under **Malware**/**General**.

## Reconnaissance

| Concept | ATT&CK |
|---|---|
| [Network Scanning and Vulnerability Discovery for Penetration Tests](recon/network-scanning-and-vulnerability-discovery.md) | T1595.001, T1595.002, T1046 |

## Network attacks

| Concept | ATT&CK |
|---|---|
| [ARP Poisoning and Man-in-the-Middle Attacks](attacks/arp-poisoning-and-man-in-the-middle-attacks.md) | T1557.002, T1557, T1040 |
| [HTTP Downgrade, Evil Update and IPv6 Attacks](attacks/http-downgrade-evil-update-and-ipv6-attacks.md) | T1557, T1557.001 |
| [Bypassing Network Access Control](attacks/bypassing-network-access-control.md) | T1599, T1200 |
| [Pivoting and Network Tunnelling](attacks/pivoting-and-network-tunnelling.md) | T1572, T1090, T1021 |
| [Wi-Fi and Bluetooth Attacks](attacks/wifi-and-bluetooth-attacks.md) | T1040, T1557, T1200 |
| [Python Raw Sockets, Sniffing and Layer-2 Attacks](attacks/python-raw-sockets-sniffing-and-layer-2-attacks.md) | T1040, T1557.002 |

## Protocol analysis and exploit development

| Concept | ATT&CK |
|---|---|
| [Network Protocol Analysis Methodology](protocol-analysis/network-protocol-analysis-methodology.md) | T1040 |
| [Network Protocol Structures (Binary and Text)](protocol-analysis/network-protocol-structures.md) | — |
| [Application Reverse Engineering for Protocols](protocol-analysis/application-reverse-engineering-for-protocols.md) | — |
| [Vulnerability Root Causes in Network Protocol Implementations](protocol-analysis/vulnerability-root-causes-in-network-protocols.md) | T1210 |
| [Fuzzing and Vulnerability Triage](protocol-analysis/fuzzing-and-vulnerability-triage.md) | T1210 |
| [Exploiting Memory Corruption and Writing Shellcode](protocol-analysis/exploiting-memory-corruption-and-writing-shellcode.md) | T1210, T1203 |
| [Cryptography in Network Protocols (Ciphers, Padding Oracle, PKI, TLS)](protocol-analysis/cryptography-in-network-protocols.md) | T1040 |
| [TLS Protocol and Post-Quantum Cryptography](protocol-analysis/tls-protocol-and-post-quantum-cryptography.md) | — |

## Command and control / malware infrastructure

| Concept | ATT&CK |
|---|---|
| [Malware C2 Network Communication (Domain Flux, DGA, Protocols)](c2/malware-c2-network-communication.md) | T1071, T1568.002, T1568.001, T1008 |
| [Domain and IP Research for Malware Infrastructure (WHOIS, Passive DNS, Fast Flux)](c2/domain-and-ip-research-for-malware-infrastructure.md) | T1596, T1568.001 |
| [Malware Collection with Honeypots (Nepenthes, Dionaea)](c2/malware-collection-with-honeypots.md) | — |

## Anonymity and OPSEC

| Concept | ATT&CK |
|---|---|
| [Anonymity Layers: Tor, VPNs and Combined Networks](anonymity/anonymity-layers-tor-vpns-and-combined-networks.md) | T1090.003, T1090, T1573 |

## Defense, monitoring and forensics

| Concept | ATT&CK |
|---|---|
| [Network Segmentation](defense/network-segmentation.md) | — |
| [Network-Based Evidence Collection and Monitoring](defense/network-based-evidence-collection-and-monitoring.md) | — |
| [Network Signatures for Malware Detection](defense/network-signatures-for-malware-detection.md) | — |
| [AI for Spam, Phishing and Network Anomaly Detection](defense/ai-for-spam-phishing-and-network-anomaly-detection.md) | — |
| [Analyzing Network Traffic and Investigating Hacker Tools](defense/analyzing-network-traffic-and-investigating-hacker-tools.md) | — |
