---
title: "MITRE ATT&CK Framework"
domain: [General]
category: [Threat Intelligence, Detection, Threat Hunting]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/
  - https://attack.mitre.org/resources/
open_issues: []
---

# MITRE ATT&CK Framework

## Summary

**MITRE ATT&CK** (Adversarial Tactics, Techniques, and Common Knowledge) is a continuously updated, publicly available knowledge base of real-world adversary behavior, organized so defenders can describe, detect, and reason about *how* attacks work rather than just *what* malware is involved. Its structure is hierarchical: **tactics** are the adversary's goals (the "why" — e.g. Initial Access, Persistence, Credential Access, Exfiltration), **techniques** are the general methods to achieve a tactic (the "how" — e.g. Phishing, Process Injection), and **sub-techniques** are more specific variants. Each technique has a stable ID (like T1055) and documents procedures, platforms, data sources, detections, and mitigations. ATT&CK spans matrices for **Enterprise** (Windows, Linux, macOS, cloud, containers, network), **Mobile**, and **ICS**. Its enormous value is as a **common language**: it lets threat intelligence describe adversary TTPs unambiguously, lets detection teams map coverage ("which techniques can we see?"), lets red teams structure emulation, and lets organizations prioritize defenses by the techniques real adversaries actually use. ATT&CK is **behavior-focused**, which is powerful because behaviors are harder for adversaries to change than tools or indicators (reflecting the "Pyramid of Pain": TTPs are the most costly for an attacker to alter). Because ATT&CK evolves — techniques are added, revoked, deprecated, and remapped — using it accurately requires referencing the current version. It is arguably the connective tissue of modern defensive security, underpinning threat intelligence, detection engineering, hunting, and purple teaming.

## ATT&CK structure

| Level | Meaning | Example |
|---|---|---|
| **Tactic** | Adversary goal (why) | Credential Access |
| **Technique** | Method (how) | Process Injection (T1055) |
| **Sub-technique** | Specific variant | DLL Injection (T1055.001) |
| **Matrix** | Platform scope | Enterprise, Mobile, ICS |

ATT&CK is a shared, behavior-focused vocabulary that ties threat intel, detection, hunting, and emulation together.

## Detection / value

- **Common language**: unambiguous description of adversary TTPs across teams and vendors.
- **Coverage mapping**: detections mapped to techniques reveal visibility gaps.
- **Prioritization**: defend against the techniques real, relevant adversaries use.
- **Behavior focus**: TTPs are costlier for attackers to change than hashes or domains (Pyramid of Pain).

## Practices

- **Map detections and intel to technique IDs** to measure and communicate coverage.
- **Prioritize by threat relevance**: focus on techniques used by adversaries targeting your sector.
- **Use current ATT&CK**: account for added/revoked/deprecated techniques and remappings.
- **Drive purple teaming**: emulate techniques and validate detections against them.

## Explanation

Before ATT&CK, defenders talked about attacks in a fuzzy, inconsistent way — one team's "advanced malware" was another's "trojan," and nobody could cleanly say *what the attacker actually did*. MITRE ATT&CK gave everyone a shared, detailed map of attacker behavior, built from observing real intrusions. It's organized like a grid. Across the top are the attacker's goals — get in, stay in, steal credentials, move around, steal data — called tactics. Under each goal are the specific methods attackers use to achieve it, called techniques, each with a stable code like T1055, and many broken down into even finer variants. The reason this is so powerful is that it shifts attention from fragile clues to durable behaviors. An attacker can trivially swap out a malware file or a web address — those change daily — but the fundamental techniques they rely on, like injecting code into another process, are much harder to abandon. So if you can detect the *behavior*, you catch the attacker even when they change their tools. ATT&CK has become the backbone of modern defense: threat intelligence uses it to describe who's doing what, detection teams use it to measure what they can and can't see, and red teams use it to test defenses systematically. One caveat worth remembering is that it's a living document — techniques get added and retired — so you always want to check against the current version.
