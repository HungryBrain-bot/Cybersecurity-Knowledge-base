---
title: "Active Defense (Deny, Disrupt, Degrade, Deceive, Destroy)"
domain: [General]
category: [Incident Response, Threat Intelligence]
difficulty: Advanced
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/resources/engage/
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-160v2r1.pdf
open_issues: []
---

# Active Defense (Deny, Disrupt, Degrade, Deceive, Destroy)

## Summary

**Active defense** is a posture that goes beyond passive controls (firewalls, patching) to **engage the adversary within one's own environment** — not "hacking back," which is generally illegal, but actively frustrating, misleading, and learning from attackers on systems you own. A common framework borrows the military "D" verbs: **Deny** (prevent the adversary from reaching objectives — access controls, segmentation), **Disrupt** (interrupt their operations mid-attack — kill a C2 channel, reset sessions), **Degrade** (reduce the effectiveness/speed of their actions — rate-limiting, friction), **Deceive** (feed them false information and decoys — honeypots, honeytokens, fake data), and **Destroy** (rendered sparingly and lawfully — remove the adversary's foothold within your environment). The emphasis in practice is on **deception and engagement**: deploying decoys and tripwires that detect and study intruders, and using the intelligence gained to improve defenses and attribution. MITRE's **Engage** framework formalizes adversary engagement and deception as a complement to ATT&CK. The key boundaries are **legal and ethical**: active defense is conducted on your own assets and data, not on attacker infrastructure; "hacking back" against external systems is unlawful in most jurisdictions and risks collateral damage and misattribution. Done correctly, active defense raises attacker cost, increases detection, buys defenders time and intelligence, and shifts the asymmetry — making your environment a hostile, confusing place for an intruder — without crossing into vigilante action.

## The five D's

| Verb | Defensive action |
|---|---|
| **Deny** | Block access to objectives (segmentation, least privilege) |
| **Disrupt** | Interrupt active operations (cut C2, reset sessions) |
| **Degrade** | Slow/reduce attacker effectiveness (friction, rate limits) |
| **Deceive** | Mislead with decoys (honeypots, honeytokens, fake data) |
| **Destroy** | Remove the foothold in your own environment (lawfully) |

The center of gravity is deception and engagement on your own assets — legal, intelligence-rich, and cost-imposing — not retaliation against external infrastructure.

## Security relevance

- **Cost imposition**: friction, decoys, and disruption make attacks slower and more expensive.
- **Detection and intelligence**: engaging intruders on decoys reveals their tools and TTPs.
- **Legal boundary**: active defense is on owned assets; "hacking back" is unlawful and risks misattribution/collateral harm.
- **Asymmetry shift**: a deceptive, hostile environment erodes the attacker's usual advantage of stealth.

## Practices

- **Deploy deception** (honeypots, honeytokens, canaries) to detect and study intruders.
- **Build disruption capability**: rapid session/credential revocation and C2 blocking.
- **Add friction** for attacker actions (segmentation, rate limits, approvals).
- **Stay within legal limits**: act only on your own systems; coordinate with legal and, where appropriate, law enforcement.
- **Feed engagement intelligence** back into detection, attribution, and hardening.

## Explanation

Most defense is passive — you build walls and hope they hold. Active defense is the idea that, once an intruder is inside *your* house, you don't have to sit still; you can actively make their life difficult, mislead them, and learn from them, all on your own property. A handy way to think about it uses five "D" words. Deny: lock the doors so they can't reach what they came for. Disrupt: cut their communications line mid-burglary. Degrade: make everything slow and frustrating for them. Deceive — the real star — fill rooms with convincing fakes, so they waste time stealing decoy documents and trip hidden alarms that tell you exactly where they are and how they work. And Destroy, used carefully and legally, means kicking their foothold out of your environment. The crucial line, which people love to blur, is that all of this happens on systems *you* own. "Hacking back" — breaking into the attacker's computers to retaliate — is a different and generally illegal thing; it risks hitting an innocent bystander whose machine was hijacked, and it can land the defender in legal trouble. Real active defense keeps the fight on your home turf, where it's lawful and where you hold the advantage: you can turn your own network into a confusing, booby-trapped maze that bleeds the attacker's time, blows their cover, and hands you the intelligence to shut them down.
