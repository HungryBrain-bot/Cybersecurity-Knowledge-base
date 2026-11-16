---
title: "Computer Security Concepts and Principles"
domain: [General]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://csrc.nist.gov/pubs/sp/800/12/r1/final
  - https://csrc.nist.gov/glossary
open_issues: []
---

# Computer Security Concepts and Principles

## Summary

The foundation of information security is the **CIA triad**: **Confidentiality** (only authorized parties can read data), **Integrity** (data and systems are not altered without authorization), and **Availability** (systems and data are accessible when needed). Most controls and most attacks map to protecting or violating one of these three. Supporting concepts round out the model: **authentication** (proving identity), **authorization** (granting appropriate access), **non-repudiation** (an actor cannot deny an action, via signatures and logs), and **accountability** (actions are attributable). Underlying good design are durable **security principles**, many from Saltzer and Schroeder: **least privilege** (minimum necessary access), **defense in depth** (layered controls so one failure isn't fatal), **fail-safe defaults** (deny by default), **economy of mechanism** (keep designs simple), **complete mediation** (check every access), **separation of privilege** and **separation of duties**, **least common mechanism**, and **psychological acceptability** (security people will actually use). Related framing includes **risk** (threat × vulnerability × impact), the distinction between **threats, vulnerabilities, and risk**, and the reality that security is about **managing risk**, not achieving perfection. These concepts are foundational because every specific technique — encryption, access control, monitoring, patching — exists to uphold the CIA triad through these principles, and every attack can be understood as subverting one of them. Mastering the vocabulary and principles is what lets practitioners reason clearly about any new technology or threat.

## Core concepts

| Concept | Meaning |
|---|---|
| **Confidentiality** | Only authorized parties can read data |
| **Integrity** | Data/systems not altered without authorization |
| **Availability** | Accessible when needed |
| **Authentication/Authorization** | Prove identity; grant appropriate access |
| **Non-repudiation/Accountability** | Actions are undeniable and attributable |

Durable design principles — least privilege, defense in depth, fail-safe defaults, complete mediation, economy of mechanism — are how the triad is upheld in practice.

## Security relevance

- **Everything maps to CIA**: controls protect, and attacks violate, confidentiality, integrity, or availability.
- **Least privilege + defense in depth**: the two most load-bearing principles for limiting blast radius.
- **Risk management, not perfection**: security is prioritizing finite resources against likely, impactful threats.
- **Simplicity aids security**: economy of mechanism reduces the bugs and misconfigurations attackers exploit.

## Practices

- **Classify by CIA needs** and apply controls accordingly.
- **Design with least privilege, fail-safe defaults, and defense in depth**.
- **Mediate every access** and separate duties for sensitive actions.
- **Frame decisions as risk management**: threat × vulnerability × impact.
- **Favor simple, acceptable designs** users won't bypass.

## Explanation

Strip away the jargon and almost all of security comes down to protecting three things, known as the CIA triad. Confidentiality is keeping secrets secret — only the right people can read the data. Integrity is trustworthiness — nobody has secretly tampered with your data or systems. Availability is being able to use things when you need them — the website is up, the files open. Every security tool you've heard of is really just defending one of these three, and every attack is an attempt to break one: stealing data breaks confidentiality, altering records breaks integrity, and ransomware or a denial-of-service attack breaks availability. On top of the triad sit a handful of timeless design principles that good engineers lean on. The two most important are "least privilege" — give every person and program only the access they truly need, so a compromise can't reach far — and "defense in depth" — stack multiple layers of protection so that one failure doesn't sink you. Others include defaulting to "deny" rather than "allow," keeping designs simple enough to actually get right, and making security usable so people don't route around it. Finally, a mature mindset accepts that perfect security is impossible; the real job is managing risk — spending your limited time and money on the threats most likely to hurt you most.
