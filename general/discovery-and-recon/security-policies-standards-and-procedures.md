---
title: "Security Policies, Standards and Procedures"
domain: [General]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://csrc.nist.gov/pubs/sp/800/12/r1/final
  - https://www.iso.org/standard/27001
open_issues: []
---

# Security Policies, Standards and Procedures

## Summary

Security **governance documents** form a hierarchy that translates high-level intent into day-to-day action, and understanding the distinctions keeps a program coherent and enforceable. A **policy** is a high-level statement of management intent and rules — *what* must be done and why (e.g. "all sensitive data must be encrypted"); policies are mandatory, approved by leadership, and relatively stable. **Standards** specify mandatory, measurable requirements that implement policy — the *specific* choices (e.g. "use AES-256" or "passwords must be ≥ 14 characters"). **Procedures** are step-by-step instructions for *how* to perform a task (e.g. how to provision an account). **Guidelines** are recommended, non-mandatory best practices offering flexibility. **Baselines** define a minimum level of security (e.g. a hardened configuration). The relationship: policy sets direction, standards make it concrete and measurable, procedures make it executable, and guidelines/baselines fill in recommendations and minimums. This structure matters because it is what makes security **enforceable, auditable, and consistent**: without written policy, rules are arbitrary and unaccountable; without standards, "encrypt data" is too vague to implement uniformly; without procedures, execution varies by individual. Good governance also ties into **accountability** (who owns and approves each document), **compliance** (policies map to regulatory requirements), and **culture** (policies communicate expectations and support enforcement). The practical insight is that documentation is not bureaucracy for its own sake — it is the backbone that lets an organization apply security uniformly, hold people accountable, demonstrate compliance, and respond consistently rather than improvising.

## The governance hierarchy

| Document | Role | Mandatory? |
|---|---|---|
| **Policy** | High-level intent and rules (what/why) | Yes |
| **Standard** | Specific, measurable requirements | Yes |
| **Procedure** | Step-by-step how-to | Yes |
| **Guideline** | Recommended best practice | No |
| **Baseline** | Minimum security level | Yes |

Policy sets direction, standards make it concrete, procedures make it executable — together they make security enforceable, auditable, and consistent.

## Security relevance

- **Enforceability**: written, approved policy is the basis for consistent enforcement and discipline.
- **Consistency**: standards and procedures prevent per-person variation in security decisions.
- **Auditability and compliance**: documentation maps to regulatory requirements and proves control operation.
- **Clarity of expectation**: policies communicate what's required, supporting a security culture.

## Practices

- **Build the hierarchy**: policies, then standards, procedures, guidelines, and baselines.
- **Assign ownership and approval** for each document; keep them current.
- **Make standards measurable** so compliance can be verified.
- **Map policies to regulations** and retain evidence.
- **Communicate and train** so people know and follow the documents.

## Explanation

Security documents often get dismissed as paperwork, but they're actually the skeleton that holds a security program upright, and they come in a deliberate hierarchy. At the top are policies — short, high-level statements of what the organization requires and why, signed off by leadership. "We encrypt sensitive data" is a policy: it sets direction but doesn't tell you how. That's where standards come in: they turn the vague intent into something specific and checkable — "use AES-256 encryption," "passwords must be at least 14 characters." Below those are procedures, the step-by-step recipes for actually doing a task, so that whoever sets up a new account does it the same correct way every time. Rounding it out are guidelines (helpful suggestions you're not forced to follow) and baselines (the minimum acceptable setup). Why bother with all these layers? Because without them, security becomes a matter of opinion and mood. A written, approved policy is what lets you actually enforce a rule and hold someone accountable for breaking it. Measurable standards are what make "encrypt the data" something an auditor can verify. Procedures are what keep ten different admins from doing the same job ten different, inconsistent ways. Far from being red tape, this documentation is what lets an organization apply security uniformly, prove to regulators that it does, and react to problems with a plan instead of improvisation.
