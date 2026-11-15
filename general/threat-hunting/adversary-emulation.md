---
title: "Adversary Emulation (Atomic Red Team, CALDERA, Mordor)"
domain: [General]
category: [Threat Hunting, Detection, Tooling & Scripting]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://github.com/redcanaryco/atomic-red-team
  - https://caldera.mitre.org/
open_issues: []
---

# Adversary Emulation (Atomic Red Team, CALDERA, Mordor)

## Summary

**Adversary emulation** is the practice of safely reproducing real adversary **techniques** in your own environment to test whether your detections and defenses actually work — closing the gap between "we think we'd catch that" and "we proved we catch that." Unlike a full red-team engagement (which emulates an adversary's whole campaign with stealth and objectives), emulation is often **atomic and technique-focused**, executing specific ATT&CK techniques to validate specific detections. Key tools: **Atomic Red Team** (Red Canary) is a library of small, portable tests, each mapped to an ATT&CK technique, that you run to generate the telemetry a real attack would, then check whether your detection fired; **MITRE CALDERA** is an automated adversary-emulation platform that chains techniques into autonomous operations driven by ATT&CK; and **Mordor / Security Datasets** provide pre-recorded datasets of attack telemetry so defenders can develop and test detections without running the attack live. The workflow is **detection validation**: pick techniques (ideally those relevant to your threat model), emulate them, confirm whether logging captured the activity and whether detections alerted, and fix the gaps — this is the heart of **purple teaming**. The value is measurable, evidence-based assurance: adversary emulation turns ATT&CK from a checklist into a tested reality, reveals blind spots in telemetry and detection before a real attacker exploits them, and produces data for building and tuning detections. Emulation must be run safely (controlled scope, non-destructive tests, authorization) since it executes real attack techniques.

## Emulation tools and roles

| Tool | Role |
|---|---|
| **Atomic Red Team** | Small per-technique tests mapped to ATT&CK |
| **MITRE CALDERA** | Automated, chained adversary emulation |
| **Mordor / datasets** | Pre-recorded attack telemetry for detection dev |
| **Purple teaming** | Emulate + validate detection collaboratively |

Emulation proves, rather than assumes, detection coverage — turning ATT&CK into tested reality and exposing blind spots before attackers do.

## Detection / value

- **Detection validation**: confirms whether telemetry and alerts actually fire for a technique.
- **Blind-spot discovery**: reveals missing logging and detection gaps proactively.
- **Repeatable and mapped**: ATT&CK-aligned tests measure coverage technique by technique.
- **Detection development**: emulation (or recorded datasets) produces data to build and tune detections.

## Practices

- **Emulate relevant techniques** from your threat model; map tests to ATT&CK IDs.
- **Validate the full path**: was it logged, did the detection alert, was it actionable?
- **Run safely**: authorized, scoped, non-destructive tests in controlled conditions.
- **Close gaps iteratively** (purple teaming) and re-test to confirm fixes.
- **Use recorded datasets** (Mordor) when live execution is impractical.

## Explanation

It's one thing to believe your security tools would catch an attacker; it's another to prove it. Adversary emulation is how you prove it — by safely performing the same techniques a real attacker would and then checking whether your defenses actually noticed. The trick is doing this in small, controlled bites. Atomic Red Team is essentially a cookbook of tiny attack "tests," each tied to a specific technique in the MITRE ATT&CK catalog; you run one, it harmlessly mimics what an attacker would do, and then you look at your monitoring to see whether an alert lit up. If it did, great — you've confirmed real coverage. If it didn't, you've just found a blind spot before a criminal did. CALDERA automates this further, stringing techniques together to emulate a whole attack sequence on its own, while projects like Mordor hand you pre-recorded attack data so you can build and test detections without running anything live. This back-and-forth — attackers (or their stand-ins) perform, defenders check and improve — is the essence of "purple teaming," blending the red (offense) and blue (defense) teams into one feedback loop. The result is assurance you can actually measure: instead of a wishful checklist of "techniques we defend against," you get tested evidence of what you'd truly catch, plus the data to fix whatever you'd miss. The one rule is to do it responsibly — with permission, careful scope, and non-destructive tests — because you are, after all, running real attack techniques on purpose.
