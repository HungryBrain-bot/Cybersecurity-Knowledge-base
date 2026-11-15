---
title: "Root Causes of Data Breaches"
domain: [General]
category: [Initial Access]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://www.verizon.com/business/resources/reports/dbir/
  - https://www.cisa.gov/topics/cyber-threats-and-advisories
open_issues: []
---

# Root Causes of Data Breaches

## Summary

Despite the variety of attacks, **data breaches cluster around a small set of recurring root causes**, and knowing them focuses defensive effort where it matters most. Industry breach analyses consistently identify: **stolen or weak credentials** (phishing, reuse, weak passwords, and lack of MFA — a leading cause, since valid credentials bypass many controls); **phishing and social engineering** (the dominant initial-access vector, targeting people); **exploitation of vulnerabilities** (unpatched, known flaws in internet-facing systems and software); **misconfiguration** (exposed cloud storage, default settings, over-permissive access — a growing cause as cloud adoption increases); **third-party/supply-chain compromise** (breaching a vendor or supplier to reach the target); **insider threats** (malicious or, very commonly, **negligent** insiders — human error such as misdelivery or mishandling data); and **lost/stolen devices or improper disposal**. A crucial cross-cutting theme is the **human element**: a large share of breaches involve human error, social engineering, or misuse, meaning people — not just technology — are central. Another is that most breaches exploit **known, preventable weaknesses** (unpatched vulns, weak credentials, misconfigurations) rather than novel zero-days. The defensive implication is clear and prioritized: the highest-leverage controls map directly to the top causes — **MFA and credential hygiene** (credentials), **awareness and email security** (phishing), **patch and vulnerability management** (exploitation), **configuration management and cloud security** (misconfiguration), **third-party risk management** (supply chain), and **data handling and DLP** (insider/error). The core insight is that breaches are largely **preventable and repetitive**: by concentrating on the handful of dominant root causes — especially credentials, phishing, unpatched vulnerabilities, and misconfiguration — organizations address the majority of real-world breach risk.

## Leading root causes

| Root cause | Primary defense |
|---|---|
| **Stolen/weak credentials** | MFA, password hygiene |
| **Phishing / social engineering** | Awareness, email security |
| **Vulnerability exploitation** | Patch & vuln management |
| **Misconfiguration** | Config management, cloud security |
| **Third-party / supply chain** | Vendor risk management |
| **Insider / human error** | Least privilege, DLP, training |

Breaches are largely preventable and repetitive: focus on the handful of dominant causes to address most real-world risk.

## Security relevance

- **Credentials and phishing lead**: valid credentials and human manipulation bypass technical controls.
- **Known weaknesses dominate**: unpatched vulns and misconfigurations, not zero-days, cause most breaches.
- **Human element is central**: error, social engineering, and misuse feature in a large share of breaches.
- **Cloud misconfiguration rising**: exposed storage and over-permissions are an increasing cause.

## Practices

- **Enforce MFA and credential hygiene** to counter the top cause.
- **Train against phishing** and deploy email security.
- **Patch known vulnerabilities** and manage configuration (especially cloud).
- **Manage third-party risk** and apply least privilege plus DLP for insider/error.
- **Prioritize by root cause**: invest first where breaches actually originate.

## Explanation

If you read enough breach reports, a striking pattern emerges: despite endless headlines about sophisticated hackers, the *same handful of causes* show up over and over. At the top is credentials — stolen, reused, or weak passwords, especially where multi-factor authentication was missing — because once an attacker has a valid login, they often just walk in. Right beside it is phishing, the trick of fooling a person into clicking or handing over secrets. Then comes exploiting unpatched vulnerabilities — not exotic unknown flaws, but publicly known holes nobody got around to fixing. Misconfiguration is a fast-rising cause as everyone moves to the cloud: a storage bucket left open to the world, a default setting never hardened, permissions set too generously. Add breaches that come in through a trusted vendor (supply chain), and the very common problem of insiders — occasionally malicious, but far more often just a well-meaning employee who emailed the wrong person or fumbled sensitive data. Two themes run through all of it. First, people are central — human error and manipulation feature in a huge share of breaches, so this isn't a purely technical problem. Second, most breaches exploit *known, preventable* weaknesses, not clever zero-day magic. That's actually good news, because it means the fixes are known too, and they map neatly onto the causes: turn on MFA, train people against phishing, patch your systems, lock down your configurations, vet your vendors, and guard against mistakes with least privilege and data-loss prevention. Breaches feel chaotic and unstoppable from the outside, but from the inside they're remarkably repetitive — and focusing on the few dominant causes knocks out most of the real risk.
