---
title: "Disaster Recovery and Business Continuity"
domain: [General]
category: [Impact]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://csrc.nist.gov/pubs/sp/800/34/r1/final
  - https://www.iso.org/standard/75106.html
open_issues: []
---

# Disaster Recovery and Business Continuity

## Summary

**Business Continuity (BC)** and **Disaster Recovery (DR)** are the planning disciplines that keep an organization operating through, and recovering from, disruptive events — including cyberattacks such as ransomware, which have made DR a frontline security control rather than just an IT-operations concern. **Business continuity** is the broader goal: keeping critical business functions running during a disruption, via plans, alternate processes, and resilience. **Disaster recovery** is the technology-focused subset: restoring IT systems and data after an outage. The planning starts with a **Business Impact Analysis (BIA)** that identifies critical functions and the cost of their downtime, which sets two key metrics: **RTO (Recovery Time Objective** — how quickly a system must be restored) and **RPO (Recovery Point Objective** — how much data loss is tolerable, i.e. how recent the last good backup must be). From these flow strategies: **backups** (ideally following the **3-2-1 rule** — three copies, two media, one offsite — with **immutable/offline copies** that ransomware cannot encrypt), redundancy and failover, alternate sites (hot/warm/cold), and documented, **tested** recovery procedures. The security angle is sharp: ransomware's entire leverage is denying availability, so **tested, isolated, immutable backups are the single most effective ransomware countermeasure** — they let an organization restore rather than pay. The recurring failure is untested plans and backups that turn out to be incomplete, inaccessible, or themselves encrypted when the disaster actually strikes.

## Core concepts

| Concept | Meaning |
|---|---|
| **BIA** | Identifies critical functions and downtime cost |
| **RTO** | Max tolerable time to restore a system |
| **RPO** | Max tolerable data loss (backup recency) |
| **3-2-1 backups** | 3 copies, 2 media, 1 offsite |
| **Immutable/offline backup** | Copy ransomware can't encrypt |
| **Testing** | Proves the plan actually works |

Against ransomware, whose whole game is denying availability, tested and immutable backups are the control that lets you restore instead of pay.

## Security relevance

- **Ransomware resilience**: immutable/offline backups defeat the extortion by enabling restore.
- **Backups are a target**: attackers delete or encrypt backups first; isolation and immutability are essential.
- **Untested plans fail**: recovery that was never rehearsed routinely breaks when it is needed.
- **RTO/RPO align effort to risk**: critical systems get faster, more frequent protection.

## Practices

- **Run a BIA** and set RTO/RPO per system based on business impact.
- **Follow 3-2-1 with immutable/offline copies**; keep at least one backup out of attacker reach.
- **Test restores regularly** — recovery drills, not just backup jobs — and fix what fails.
- **Protect and monitor backup infrastructure**; separate its credentials from production.
- **Document and maintain plans**, including communications and roles, and update after changes.

## Explanation

Every organization eventually has a very bad day — a fire, a flood, a failed server, or, increasingly, ransomware that locks up everything at once. Business continuity and disaster recovery are the plans for surviving that day. Continuity is the big-picture question: how do we keep serving customers and doing essential work while things are broken? Disaster recovery is the nuts-and-bolts subset: how do we get our systems and data back? Good planning starts by asking which functions truly can't stop and how much an outage costs, which produces two numbers that drive everything else: how fast must this be back (recovery time), and how much recent data can we afford to lose (recovery point). The answers shape your backup and redundancy strategy. In the ransomware era, backups have become the star of the show, because ransomware's only real power is making your data unavailable — and if you can simply restore from a clean copy, the extortion collapses. But there's a brutal catch attackers have learned: they hunt down and encrypt or delete the backups first. So the backups that actually save you are the ones kept offline or made immutable, beyond the attacker's reach, and — the part everyone skips — *tested*, because a backup you've never tried to restore is just a hope, not a plan.
