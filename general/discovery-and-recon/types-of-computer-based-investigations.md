---
title: "Types of Computer-Based Investigations"
domain: [General]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf
  - https://csrc.nist.gov/glossary
open_issues: []
---

# Types of Computer-Based Investigations

## Summary

Digital investigations come in several **types**, each with different goals, standards of proof, and legal constraints, and knowing the distinctions shapes how evidence must be handled. The major categories: **Criminal investigations** (law enforcement pursuing crimes; the highest evidentiary bar — "beyond a reasonable doubt" — strict chain of custody, and admissibility rules, since results may convict). **Civil investigations** (disputes between parties — litigation, e-discovery; a "preponderance of evidence" standard, with evidence handling still rigorous). **Internal/corporate investigations** (an organization examining policy violations, misconduct, fraud, or HR matters; governed by internal policy and employment law, often more flexible but still requiring documentation and care, especially if it may escalate to legal action). **Incident response / security investigations** (determining what happened in a breach — scope, root cause, remediation; speed and operational recovery are priorities, though evidence should be preserved in case of escalation). **Administrative/regulatory investigations** (compliance or regulatory inquiries). A key cross-cutting distinction is whether an investigation is or might become **legal** (criminal/civil), because that dictates the standard of evidence handling: investigations that could end in court demand strict forensic discipline (chain of custody, integrity verification, working on copies) from the start, since you cannot retroactively make sloppily-collected evidence admissible. Another is the **standard of proof** (criminal's high bar vs. civil's lower one). Understanding the type up front determines the methodology: the same technical forensics apply, but the rigor, documentation, legal authority, and privacy/consent requirements differ — and treating any investigation as potentially legal protects the option to escalate.

## Investigation types

| Type | Goal | Standard / constraint |
|---|---|---|
| **Criminal** | Prosecute a crime | Beyond reasonable doubt; strict admissibility |
| **Civil** | Resolve a dispute | Preponderance of evidence; e-discovery |
| **Internal/corporate** | Policy/misconduct/fraud | Internal policy, employment law |
| **Incident response** | Scope, root cause, recovery | Speed + preserve evidence |
| **Regulatory** | Compliance inquiry | Regulatory requirements |

Whether an investigation could become legal dictates evidence rigor from the outset — you cannot retroactively make careless collection admissible.

## Security relevance

- **Standard of proof drives rigor**: criminal cases demand the strictest chain of custody and integrity.
- **Preserve the option to escalate**: treat investigations as potentially legal to keep evidence admissible.
- **Legal authority and privacy**: consent, employment law, and jurisdiction constrain what you may collect.
- **IR vs. legal tension**: security investigations prioritize recovery but should not destroy evidence.

## Practices

- **Classify the investigation type** up front to set the right methodology and legal footing.
- **Apply forensic discipline by default** (chain of custody, hashing, copies) so evidence stays admissible.
- **Involve legal/HR early** for internal and potentially-legal matters; confirm authority and consent.
- **Document thoroughly** regardless of type; it supports escalation and defensibility.

## Explanation

Not all digital investigations are the same, and mistaking one type for another can ruin a case before it starts. The most demanding is a criminal investigation, where police are building a case to put someone in prison — so the standard of proof is "beyond a reasonable doubt," and every piece of evidence must be handled with airtight discipline, because a single break in the chain of custody can get it thrown out of court. A civil investigation, like a lawsuit between two companies, has a lower bar ("more likely than not") but still needs careful evidence handling. Then there are internal corporate investigations — a company looking into an employee's policy violation, suspected fraud, or an HR complaint — which are more flexible and governed by workplace rules, though they still demand documentation, especially since they can suddenly turn into a legal matter. Incident response is its own flavor: here the priority is figuring out what the attacker did and getting the business running again fast, but you still want to preserve evidence in case the breach becomes a legal case. The practical thread connecting all of them is a simple rule of thumb: if there's any chance an investigation could end up in front of a judge, treat it that way from the very first step, with full forensic rigor — because you can never go back and retroactively make carelessly gathered evidence trustworthy. The same technical skills apply across all these types; what changes is how strict, how documented, and how legally careful you must be.
