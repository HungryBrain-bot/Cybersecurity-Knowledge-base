---
title: "Incident Response Methodology and Process"
domain: [General]
category: [Incident Response]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf
  - https://csrc.nist.gov/pubs/sp/800/86/final
open_issues: []
---

# Incident Response Methodology and Process

## Summary

Where the IR cycle names the phases, an **IR methodology** is the disciplined process that makes response repeatable, evidence-driven, and defensible across an investigation. A sound methodology emphasizes several cross-cutting practices. **Investigative rigor**: form hypotheses, collect evidence to confirm or refute them, and avoid jumping to conclusions — track **indicators of compromise (IOCs)** and pivot from one lead to the next systematically. **Evidence preservation and forensics discipline** throughout (chain of custody, working on copies, order of volatility), because an incident may become a legal matter or require deep root-cause analysis. **Scoping and iteration**: an investigation rarely reveals the full picture at once, so responders iterate — each discovered host or account expands scope until the boundary of compromise is understood, which must happen before full eradication. **Coordination and communication**: clear roles (incident commander, analysts, comms, legal, management), documented decisions, and stakeholder updates. **Playbooks** for common scenarios (ransomware, BEC, data exfiltration) to standardize response. A central methodological tension is **containment timing**: contain too early and you lose visibility into scope; contain too late and damage spreads — mature teams scope enough to contain comprehensively rather than whack-a-mole. The outcome of a good methodology is an investigation whose findings are accurate, whose actions are coordinated and documented, and whose root cause is actually fixed, as opposed to a frantic cleanup that leaves the attacker a way back in.

## Methodology pillars

| Pillar | Practice |
|---|---|
| **Investigative rigor** | Hypotheses, IOCs, systematic pivoting |
| **Forensic discipline** | Chain of custody, copies, volatility order |
| **Iterative scoping** | Expand scope until compromise boundary is known |
| **Coordination** | Defined roles, documentation, comms |
| **Playbooks** | Standardized response per scenario |

The hardest judgment is when to contain: scope broadly enough to contain comprehensively, rather than tipping off the attacker with piecemeal actions.

## Detection / value

- **Systematic pivoting**: following IOCs lead-to-lead uncovers the full footprint, not just the first alert.
- **Comprehensive scoping**: knowing every compromised host/account before eradication prevents attacker persistence.
- **Documented decisions**: a clear record supports legal defensibility and post-incident learning.
- **Role clarity**: an incident commander and defined roles prevent duplicated or missed actions under stress.

## Practices

- **Investigate hypothesis-first**: collect evidence to confirm or deny, track and pivot on IOCs.
- **Preserve evidence** with forensic discipline in case of escalation.
- **Scope fully before eradicating**; avoid alerting the attacker with partial actions.
- **Assign clear roles** (commander, analysts, comms, legal) and document every decision.
- **Use and refine playbooks** for recurring incident types.

## Explanation

Knowing the phases of incident response is like knowing a recipe's steps; the methodology is the cooking skill that makes it actually work. A good responder behaves like a detective, not a firefighter. They form a theory — "the attacker got in through this server" — and then hunt for evidence that proves or disproves it, following each clue (a suspicious file, a strange login, a known-bad address) to the next, carefully mapping out everywhere the intruder has been. This matters enormously because attackers rarely touch just one machine; if you clean up the first computer you find and declare victory, you've likely left the attacker sitting on three others, ready to walk back in. So the methodology insists on *scoping* the whole breach before slamming the door — otherwise you play an exhausting game of whack-a-mole. There's a real tension in the timing: act too soon and you tip off the attacker before you understand how deep they are; wait too long and the damage grows. Experienced teams walk that line deliberately. Around all of this sits structure: someone clearly in charge, defined roles, careful documentation (both for learning and in case lawyers get involved), and pre-written playbooks for the common nightmares, so the team executes smoothly instead of arguing about who does what while the clock ticks.
