---
title: "Patch and Configuration Management"
domain: [General]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://csrc.nist.gov/pubs/sp/800/40/r4/final
  - https://csrc.nist.gov/pubs/sp/800/128/final
open_issues: []
---

# Patch and Configuration Management

## Summary

**Patch management** and **configuration management** are two of the highest-leverage, least glamorous controls in security, because unpatched software and insecure configurations are among the most common root causes of breaches. **Patch management** (NIST SP 800-40) is the disciplined process of identifying, acquiring, testing, and deploying updates that fix vulnerabilities, across the whole estate (OS, applications, firmware, libraries). Its hard parts are **timeliness** (attackers weaponize disclosed vulnerabilities quickly, so the window between patch release and deployment is a race), **coverage** (knowing every asset that needs patching — you can't patch what you don't know you have), **testing vs. speed** (balancing stability against urgency), and **prioritization** (patching the most exploitable, exposed, and critical first, informed by severity and real-world exploitation). **Configuration management** (NIST SP 800-128) ensures systems are set up and *stay* in a secure, known state: establishing **secure baselines** (hardening guides like CIS Benchmarks), controlling changes, detecting **configuration drift**, and remediating misconfigurations (open ports, default credentials, excessive permissions, disabled security features). The two are linked: both are about maintaining a known, secure state against the entropy of change and newly discovered flaws. Their security value is enormous and well-evidenced — a large share of successful attacks exploit vulnerabilities for which a patch existed, or misconfigurations that a baseline would have prevented (as in the Equifax and many cloud breaches). The lesson is that doing these basics consistently, at scale, with good asset inventory, prevents more incidents than almost any advanced tool.

## Core activities

| Activity | Goal |
|---|---|
| **Asset inventory** | Know everything that needs patching/config |
| **Patch deployment** | Fix vulnerabilities promptly across the estate |
| **Prioritization** | Patch exploitable, exposed, critical first |
| **Secure baselines** | Harden to known-good config (CIS Benchmarks) |
| **Drift detection** | Catch and fix deviations from baseline |

Both disciplines fight entropy: keep systems in a known, secure state despite constant change and newly found flaws — the basics that prevent most breaches.

## Security relevance

- **Known vulnerabilities dominate breaches**: unpatched, publicly disclosed flaws are a top attack vector.
- **You can't patch what you can't see**: asset inventory is the prerequisite for coverage.
- **Misconfiguration is pervasive**: default creds, open ports, and over-permissions cause major breaches.
- **Speed matters**: the patch-to-exploit window is short; prioritization by real exploitation is key.

## Practices

- **Maintain a complete asset inventory** as the foundation.
- **Patch promptly, prioritized** by severity, exposure, and known exploitation; test proportionally to risk.
- **Apply secure baselines** (CIS Benchmarks) and enforce them.
- **Detect and remediate drift** continuously; manage changes.
- **Measure coverage and timeliness** to find gaps before attackers do.

## Explanation

If you could only do a few things to prevent breaches, patching and configuration management would be near the top of the list — which is ironic, because they're the chores everyone finds boring. Patching is simply keeping your software up to date: when a vendor releases a fix for a security hole, you need to install it everywhere, quickly. "Quickly" is the hard part, because the moment a fix is announced, attackers study it to figure out the hole it plugs and race to exploit anyone who hasn't patched yet — so every day of delay is a day of exposure. The catch is you can only patch things you know you own, which is why a solid inventory of all your computers, apps, and devices is the unglamorous foundation of the whole effort. Configuration management is the sibling task: making sure systems are set up securely in the first place and *stay* that way. Fresh installs often ship with risky defaults — open doors, factory passwords, security features switched off — and over time, as people tinker, systems "drift" away from their safe settings. So you define a hardened baseline, lock systems to it, and watch for drift. None of this is exciting, but history is blunt: a huge fraction of real-world breaches, including some of the biggest ever, happened because someone didn't install an available patch or left something misconfigured. Doing the basics well, everywhere, consistently, beats almost any shiny tool.
