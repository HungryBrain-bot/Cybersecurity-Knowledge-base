# Cloud and Container Security

Cloud infrastructure, container and orchestration security: how the platforms work, how they are attacked and misconfigured, and how to detect and defend them. Covers AWS, Azure, Kubernetes and Docker. Every file is verified against vendor primary docs and the current MITRE ATT&CK data before publishing.

Files are grouped by category.

## Cloud security

| Concept | Difficulty | ATT&CK |
|---|---|---|
| [AWS S3 Bucket Security and Misconfiguration](cloud-security/aws-s3-bucket-security-and-misconfiguration.md) | Beginner | T1530, T1580 |
| [AWS IAM Enumeration and Privilege Abuse](cloud-security/aws-iam-enumeration-and-privilege-abuse.md) | Intermediate | T1078.004, T1087.004, T1098 |
| [AWS Penetration Testing Methodology and Tooling](cloud-security/aws-penetration-testing-methodology-and-tooling.md) | Intermediate | T1078.004, T1526, T1580, T1087.004 |
| [AWS Lambda and Serverless Security](cloud-security/aws-lambda-and-serverless-security.md) | Intermediate | T1648, T1078.004, T1552.005, T1059 |
| [AWS Secrets Management and Credential Exposure](cloud-security/aws-secrets-and-credential-exposure.md) | Intermediate | T1552.001, T1552.004, T1552.005, T1528, T1530 |
| [Hunting for AWS Misconfigurations](cloud-security/hunting-for-aws-misconfigurations.md) | Intermediate | T1580, T1526, T1530, T1552.005, T1078.004, T1190, T1685.002 |
| [AWS Cloud Threat Prevention Framework](cloud-security/aws-cloud-threat-prevention-framework.md) | Advanced | — |
| [Azure Governance and Identity Security](cloud-security/azure-governance-and-identity-security.md) | Intermediate | T1078.004, T1098 |
| [Azure Network, Key Vault and Data Security](cloud-security/azure-network-key-vault-and-data-security.md) | Intermediate | T1552.001, T1530 |
| [Azure Security Center and Best Practices (Defender for Cloud)](cloud-security/azure-security-center-and-best-practices.md) | Intermediate | — |
| [Azure Cloud Threat Prevention and Detection Framework](cloud-security/azure-cloud-threat-prevention-and-detection-framework.md) | Advanced | — |
| [Microsoft Sentinel Overview and Data Connectors](cloud-security/microsoft-sentinel-overview-and-data-connectors.md) | Intermediate | — |
| [Sentinel Multi-Tenant Architecture and Operations](cloud-security/sentinel-multi-tenant-architecture-and-operations.md) | Advanced | — |
| [Kusto Query Language (KQL) for Threat Hunting](cloud-security/kql-for-threat-hunting.md) | Intermediate | — |
| [Cloud Threat Hunting with MITRE ATT&CK Tactics](cloud-security/cloud-threat-hunting-with-mitre-attack.md) | Advanced | — |
| [AI for User Authentication and Fraud Prevention](cloud-security/ai-for-user-authentication-and-fraud-prevention.md) | Intermediate | T1110, T1078 |
| [Google Workspace Takeover and CloudTrail Evasion](cloud-security/google-workspace-takeover-and-cloudtrail-evasion.md) | Advanced | T1098, T1685.002, T1556 |
| [Stealthy Persistence in Cloud Environments (Backdoors, Lambda)](cloud-security/stealthy-persistence-in-cloud-environments.md) | Advanced | T1098, T1098.001, T1648 |
| [Data Platform Attacks (Spark, Redshift) and Data Theft](cloud-security/data-platform-attacks-and-data-theft.md) | Advanced | T1567, T1530, T1190 |
| [Container Threat Model and Security Boundaries](cloud-security/container-threat-model-and-security-boundaries.md) | Beginner | — |
| [Container Images and Image Vulnerabilities](cloud-security/container-images-and-image-vulnerabilities.md) | Intermediate | T1525, T1195.002 |
| [Container Network Security, TLS and Secrets](cloud-security/container-network-security-tls-and-secrets.md) | Intermediate | T1552.007 |
| [Container Runtime Protection and the OWASP Top 10](cloud-security/container-runtime-protection-and-owasp-top-10.md) | Intermediate | — |
| [Strengthening and Breaking Container Isolation](cloud-security/strengthening-and-breaking-container-isolation.md) | Advanced | T1611 |
| [Virtual Machines versus Containers Isolation](cloud-security/virtual-machines-versus-containers-isolation.md) | Intermediate | T1611 |
| [Kubernetes Architecture and Attacks (Pods, RBAC, Service Accounts)](cloud-security/kubernetes-architecture-and-attacks.md) | Advanced | T1610, T1611, T1613 |

## Privilege escalation

| Concept | Difficulty | ATT&CK |
|---|---|---|
| [Container Escape and Docker Misconfigurations](privilege-escalation/container-escape-and-docker-misconfigurations.md) | Advanced | T1611, T1610 |
| [CI/CD Pipeline Takeover (Jenkins and Automation Tools)](privilege-escalation/cicd-pipeline-takeover.md) | Advanced | T1072, T1195.002, T1552.001 |

## Exploitation

| Concept | Difficulty | ATT&CK |
|---|---|---|
| [Redis Exploitation (Deserialization and Cache Poisoning)](exploitation/redis-exploitation.md) | Advanced | T1190, T1496 |

## Web security

| Concept | Difficulty | ATT&CK |
|---|---|---|
| [Server-Side Request Forgery and the Cloud Metadata API](web-security/ssrf-and-cloud-metadata-api.md) | Intermediate | T1552.005 |

## Walkthroughs

Worked, end-to-end examples that chain the concepts above into a single attack path, reconstructed from authorized labs with all credentials, account IDs, IPs and flags redacted. This section grows as more labs are added.

| Walkthrough | Difficulty | ATT&CK |
|---|---|---|
| [AWS EC2 Privilege Escalation (Lambda to IMDS to Secrets Manager)](walkthroughs/aws-ec2-lambda-imds-secrets-walkthrough.md) | Intermediate | T1078.004, T1087.004, T1526, T1552.001, T1580, T1190, T1552.005, T1555.006 |
| [AWS IAM Enumeration (Policies, Groups and Role Trust)](walkthroughs/aws-iam-enumeration-four-flags-walkthrough.md) | Beginner | T1078.004, T1069.003, T1087.004 |

## Interview prep

Rehearsal pages organised for explaining concepts out loud: each concept has a technical definition, why a pentester cares, a plain-language example and the likely interview question.

| Page | Difficulty | ATT&CK |
|---|---|---|
| [Interview Prep: AWS Penetration Testing](interview-prep/aws-pentest-interview-prep.md) | Beginner to Intermediate | T1078.004, T1087.004, T1069.003, T1580, T1526, T1619, T1530, T1552.001, T1552.005, T1555.006, T1098.001, T1098.003, T1548.005, T1648, T1190, T1685.002 |

---

The Cloud domain covers 30 concepts plus 2 walkthroughs and 1 interview-prep page. Other domains (Web, Network, Linux, macOS, Malware, Windows, General) are added in subsequent batches.
