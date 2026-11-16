---
title: "Microsoft Defender ATP, Azure Monitor and Sentinel for Security Operations"
domain: [Windows, Cloud]
category: [Cloud Security]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://www.microsoft.com/en-us/security/blog/2020/09/22/microsoft-unified-siem-xdr-modernize-security-operations/
  - https://learn.microsoft.com/en-us/azure/sentinel/overview
  - https://learn.microsoft.com/en-us/defender-for-identity/what-is
  - https://learn.microsoft.com/en-us/defender-cloud-apps/what-is-defender-for-cloud-apps
  - https://learn.microsoft.com/en-us/defender-xdr/microsoft-secure-score
  - https://learn.microsoft.com/en-us/defender-endpoint/network-protection
  - https://learn.microsoft.com/en-us/defender-endpoint/tamper-protection-overview
open_issues: []
---

# Microsoft Defender ATP, Azure Monitor and Sentinel for Security Operations

## Summary

Organisations running Windows at scale often build their security operations on Microsoft's own cloud tooling: an **EDR** on every endpoint, **identity and cloud-app** sensors, a **SIEM/SOAR** that pulls everything together, and a **posture score** that tracks hardening. The product names have changed several times. The Notion source uses the 2020 names. Current names are used below; Microsoft's 2020 announcement confirms the Defender for Endpoint and Defender for Identity renames, and the other source names are listed for cross-reference.

## The pieces

| Current name | Name used in the source | Role |
|---|---|---|
| **Microsoft Defender for Endpoint** | Microsoft Defender Advanced Threat Protection (Defender ATP) | EDR: device onboarding, alerts, investigation, automated investigation and remediation, device groups and role-based permissions |
| **Microsoft Defender for Identity** | Azure Advanced Threat Protection (Azure ATP) | Watches on-premises Active Directory signals for identity attacks (reconnaissance, credential theft, lateral movement) |
| **Microsoft Defender for Cloud Apps** | Cloud App Security | Visibility and control over cloud application use |
| **Microsoft Sentinel** | Azure Sentinel | Cloud-native SIEM and SOAR; playbooks are built on Azure Logic Apps workflows |
| **Azure Monitor** | Azure Monitor / Log Analytics | Collects platform and activity logs and metrics; used for baselines and alerting |
| **Microsoft Secure Score** | Secure Score | Measures security posture; found in the Microsoft Defender portal |

## How a SOC uses them

1. **Onboard** every Windows device to Defender for Endpoint, and deploy the identity sensor on domain controllers.
2. **Tune** device groups, permissions and automation levels so investigations run automatically on low-risk devices and with approval on critical ones.
3. **Connect** Defender, identity, cloud-app, Azure activity and third-party logs into Sentinel.
4. **Detect and respond** with analytics rules and playbooks in Sentinel, and incident queues in the Defender portal.
5. **Harden** using Secure Score recommendations, network protection and tamper protection on endpoints.
6. **Plan for continuity**: decide what the SOC does when cloud tooling is unavailable, and keep critical logs exportable.

## Strengths and limits

- Deep integration across endpoint, identity and cloud reduces correlation work.
- Licensing tiers decide which features exist; check before assuming automated investigation or advanced hunting is available.
- Coverage is only as good as onboarding; unmanaged devices and non-Windows systems need separate attention.
- Attackers target the tooling itself, so protect admin roles in these portals as tier-0 assets and keep tamper protection on.

## ATT&CK mapping

None directly; this is defensive tooling. Detection content built on it maps to the techniques covered elsewhere in this knowledge base.

## Explanation

Think of a city's emergency services. Each building has smoke detectors (endpoint EDR), the city watches traffic and ID checks (identity), there is a register of which delivery companies visit which buildings (cloud apps), and a central dispatch room sees every alarm on one map and can send crews automatically (Sentinel). A yearly inspection score tells each building how fire-safe it is (Secure Score). The names on the vans have been repainted a few times, but the jobs have stayed the same.
