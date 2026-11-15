---
title: "Threat Hunting Maturity Model and Team Structure"
domain: [General]
category: [Discovery & Recon]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/
  - https://www.sans.org/white-papers/
open_issues: []
---

# Threat Hunting Maturity Model and Team Structure

## Summary

The **Hunting Maturity Model (HMM)** describes how a threat-hunting capability develops, giving organizations a way to assess where they stand and what to build next. The commonly referenced levels run from **HM0 (Initial)** — reliant purely on automated alerting with little or no hunting — through **HM1 (Minimal)** (incorporating threat-intelligence indicators and some data collection), **HM2 (Procedural)** (applying others' hunting procedures and more data), **HM3 (Innovative)** (creating new hunting procedures and analytics), to **HM4 (Leading)** (highly automated, where successful hunts are operationalized into detection and the team focuses on novel analysis). Progression up the model depends on three things: **data** (richer telemetry and retention), **tooling/analytics** (the ability to query and analyze at scale), and **skills/process** (analysts who can hypothesize and investigate, following a repeatable methodology). **Team structure** varies by maturity and size: hunting may be a part-time duty of SOC analysts, a dedicated hunt team, or a function embedded across detection engineering and incident response. Effective teams blend skills — deep knowledge of adversary TTPs (ATT&CK), the environment, data analysis, and detection engineering — and crucially **feed results back** into automated detection so hunting doesn't just find threats but permanently improves coverage. The model's value is as a **roadmap**: it clarifies that maturity is less about buying tools and more about data quality, repeatable process, skilled people, and the discipline of turning hunts into detections. Organizations use it to benchmark honestly (most start low), prioritize investments (often data first), and grow deliberately toward automated, continuously improving hunting.

## Maturity levels

| Level | Characteristic |
|---|---|
| **HM0 Initial** | Automated alerts only; little hunting |
| **HM1 Minimal** | Threat-intel indicators; some data |
| **HM2 Procedural** | Uses others' procedures; more data |
| **HM3 Innovative** | Creates new procedures/analytics |
| **HM4 Leading** | Automated; hunts become detections |

Climbing the model depends on data, analytics, and skilled repeatable process — not on tools alone.

## Security relevance

- **Honest benchmarking**: the model shows where a program really stands (usually low to start).
- **Investment priority**: data quality and skills typically matter more than another tool.
- **Feedback discipline**: mature teams convert hunts into automated detections, compounding coverage.
- **Team fit**: structure (part-time, dedicated, embedded) should match maturity and size.

## Practices

- **Assess maturity** honestly against the HMM; identify the next achievable level.
- **Invest in data and skills** before advanced tooling; both gate progression.
- **Adopt a repeatable methodology** (hypothesis-driven, ATT&CK-aligned).
- **Operationalize hunts** into detections to reach higher maturity.
- **Structure the team** to fit size and maturity; blend TTP, data, and detection-engineering skills.

## Explanation

Threat hunting doesn't spring into existence fully formed; it grows through stages, and the Hunting Maturity Model is a ladder that lets a team honestly see which rung they're on. At the bottom, an organization does no real hunting — it just waits for automated alarms to go off. One rung up, they start weaving in threat-intelligence tips and collecting a bit more data. Higher still, they borrow proven hunting recipes from the community, then begin inventing their own, and at the top they run a slick operation where every successful hunt gets converted into an automatic detection and the humans spend their time on genuinely new problems. What actually moves a team up this ladder isn't buying a shiny hunting platform; it's three less glamorous things — having rich enough data to hunt in, having the tools and skills to analyze it, and following a repeatable method instead of poking around randomly. How the team is organized depends on size and maturity: in a small shop, hunting might be a hat the SOC analysts put on a few hours a week; in a larger one, it's a dedicated squad blending people who understand attacker behavior, the home environment, and how to build detections. The model's real gift is clarity and honesty. Most organizations discover they're near the bottom, which is fine — the point is to see it clearly, invest in data and people first, and climb deliberately toward hunting that's automated, repeatable, and always making the defenses a little smarter.
