---
title: "Purple Teaming"
domain: [General]
category: [Discovery & Recon]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/
  - https://github.com/redcanaryco/atomic-red-team
open_issues: []
---

# Purple Teaming

## Summary

**Purple teaming** is a collaborative approach that merges the **red team** (offense) and **blue team** (defense) into a single feedback loop, rather than treating them as adversaries. In a traditional red-team engagement, offense operates covertly and the results are delivered at the end; in purple teaming, red and blue work **together, often in real time**: the red team executes a technique, the blue team checks whether they detected it, and together they diagnose and fix the gap immediately — then re-test to confirm the fix. The goal is not to "win" but to **measurably improve detection and response**. Purple teaming is typically **ATT&CK-driven**: techniques are selected from the ATT&CK matrix (often prioritized by threat relevance), emulated (using tools like Atomic Red Team or CALDERA), and the blue team validates whether telemetry captured the activity and whether detections alerted — producing a coverage map of what the organization can and cannot see. The benefits are faster, more direct improvement than siloed exercises: every technique tested either confirms a working detection or produces a new one, blind spots are found collaboratively, and knowledge transfers between teams. Purple teaming can be a dedicated function, a periodic exercise, or an operating mode. The core insight is that making offense and defense **cooperate instead of compete** turns security testing into continuous, measurable detection-engineering — each exercise directly strengthens the blue team's visibility and the organization's resilience, with the red team's skills used to build defenses rather than merely to prove they can be beaten.

## Purple vs. traditional

| Aspect | Traditional red team | Purple teaming |
|---|---|---|
| **Interaction** | Covert; report at end | Collaborative, often real-time |
| **Goal** | Test defenses / achieve objectives | Measurably improve detection |
| **Output** | Findings report | Validated detections, coverage map |
| **Framework** | Varies | Typically ATT&CK-driven |

Offense and defense cooperating turns testing into continuous detection-engineering: each technique confirms a detection or creates one.

## Detection / value

- **Immediate improvement**: gaps are found and fixed in the same session, then re-tested.
- **Coverage mapping**: ATT&CK-driven testing reveals exactly what is and isn't detected.
- **Knowledge transfer**: red and blue learn each other's craft, strengthening both.
- **Measurable outcomes**: detections built and visibility gained quantify value.

## Practices

- **Run ATT&CK-driven exercises**: select relevant techniques, emulate, validate detection, fix, re-test.
- **Collaborate in real time** between red and blue; diagnose gaps together.
- **Build detections from every test**; track coverage against ATT&CK.
- **Prioritize by threat relevance** to your organization's threat model.
- **Repeat regularly** so coverage keeps pace with evolving techniques.

## Explanation

Traditionally, security testing is a contest: a red team quietly tries to break in, the blue team tries to catch them, and at the end the red team reveals how they won. It's useful, but adversarial and slow — the blue team often learns what they missed only weeks later in a report. Purple teaming tears down that wall and puts both teams in the same room, working together. The red team performs an attacker technique out in the open; the blue team immediately checks their monitoring to see if it was caught. If it was, great — they've confirmed a real detection. If it wasn't, they don't wait for a report; they figure out why together on the spot, build a detection to cover it, and then the red team runs the technique again to prove the fix works. The whole thing is usually organized around the MITRE ATT&CK catalog, so the team methodically works through the techniques that matter most to them and ends up with a clear map of what they can and can't see. The beauty is that nobody's trying to beat anybody — the red team's offensive skills are pointed at *building* defenses rather than just embarrassing them. Every single technique tested leaves the organization measurably stronger: either it confirmed something works, or it closed a blind spot. It turns a once-a-year "gotcha" exercise into a steady engine of improvement.
