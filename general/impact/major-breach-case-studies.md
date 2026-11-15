---
title: "Major Breach Case Studies (Capital One, Marriott, Equifax, OPM, Yahoo, Target, JPMorgan)"
domain: [General]
category: [Impact]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://www.cisa.gov/news-events/news
  - https://nvd.nist.gov/vuln/detail/CVE-2017-5638
open_issues: []
---

# Major Breach Case Studies (Capital One, Marriott, Equifax, OPM, Yahoo, Target, JPMorgan)

## Summary

Studying landmark breaches is valuable because each one distills a **recurring root cause** into a concrete lesson, and the same handful of causes repeat across very different organizations. **Equifax (2017, ~147M people)** stemmed from an **unpatched known vulnerability** in Apache Struts (CVE-2017-5638) — a lesson in vulnerability management and patch timeliness. **Target (2013, ~40M payment cards)** began with **stolen third-party vendor credentials** (an HVAC supplier) that led to point-of-sale malware — a lesson in supply-chain/third-party risk and network segmentation. **Capital One (2019, ~100M+ records)** resulted from a **misconfigured cloud resource** (a web application firewall abused via SSRF to reach cloud metadata and storage) — a lesson in cloud configuration and least privilege. **Marriott/Starwood (disclosed 2018, hundreds of millions of guest records)** involved a long, **undetected dwell time** in an acquired company's systems — lessons in detection, dwell time, and M&A security due diligence. **OPM (2015, ~21.5M background-investigation records)** exposed extremely sensitive government personnel data — lessons in protecting high-value data and timely detection. **Yahoo (2013–2014, up to ~3B accounts)** showed the scale of account data exposure and delayed disclosure. **JPMorgan (2014, ~76M households)** underscored that even well-resourced firms fall to gaps (reportedly a server missing MFA). Across all of them the themes converge: **patch known vulnerabilities, manage third-party risk, configure cloud and access correctly, detect intrusions faster, protect high-value data, and enforce MFA**. The case studies matter because they turn abstract controls into memorable, consequential stories.

## Breaches and lessons

| Breach | Root-cause theme | Lesson |
|---|---|---|
| **Equifax (2017)** | Unpatched known CVE (Struts) | Timely patching / vuln management |
| **Target (2013)** | Third-party vendor credentials | Supply-chain risk, segmentation |
| **Capital One (2019)** | Cloud misconfiguration (SSRF) | Cloud config, least privilege |
| **Marriott/Starwood (2018)** | Long undetected dwell time | Detection, M&A due diligence |
| **OPM (2015)** | High-value data, slow detection | Protect crown jewels, detect fast |
| **Yahoo (2013–14)** | Massive account exposure | Scale, timely disclosure |
| **JPMorgan (2014)** | Server lacking MFA | Enforce MFA everywhere |

Different companies, the same short list of causes — which is exactly why the lessons generalize.

## Security relevance

- **Known vulnerabilities dominate**: Equifax shows that unpatched, publicly known flaws remain a top breach cause.
- **Third parties are an entry point**: Target's vendor path is the archetype of supply-chain compromise.
- **Cloud misconfiguration is the new frontier**: Capital One illustrates SSRF and over-permissioned cloud roles.
- **Dwell time matters**: Marriott and OPM show that slow detection magnifies impact enormously.
- **MFA gaps are exploited**: JPMorgan highlights that a single un-MFA'd server can be the way in.

## Practices

- **Patch known vulnerabilities promptly**; track exposure to disclosed CVEs.
- **Govern third-party access** and segment vendor connectivity.
- **Audit cloud configuration** and enforce least-privilege roles and metadata protections.
- **Invest in detection** to shrink dwell time; include acquisitions in security scope.
- **Enforce MFA universally** and protect high-value data with extra controls.

## Explanation

The most striking thing about history's biggest breaches is how *unexotic* their causes usually are. Equifax lost 147 million people's data not to some genius hack but because they didn't install a security patch that had been available for weeks. Target's card-stealing nightmare started with a heating-and-air-conditioning contractor whose stolen login became the attacker's front door. Capital One's cloud breach came down to a misconfigured security setting that let an attacker trick a server into handing over the keys to its cloud storage. Marriott inherited a breach from a hotel chain it acquired, where intruders had been quietly camped for years before anyone noticed. The OPM breach spilled the deeply sensitive background-check files of millions of federal employees. Yahoo managed to expose essentially every account it had, and JPMorgan — a bank with enormous security budgets — was undone reportedly by a single server that someone forgot to put multi-factor authentication on. Read together, they're almost repetitive, and that's the point: patch your known holes, watch your vendors, configure your cloud carefully, notice intruders faster, guard your most sensitive data, and turn on MFA everywhere. These case studies endure because they take dry best-practice advice and attach it to a story — and a price tag — you don't forget.
