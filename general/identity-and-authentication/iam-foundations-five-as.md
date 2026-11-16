---
title: "Identity and Access Management Foundations (Five A's)"
domain: [General]
category: [Identity & Authentication]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://pages.nist.gov/800-63-3/
  - https://csrc.nist.gov/pubs/sp/800/207/final
open_issues: []
---

# Identity and Access Management Foundations (Five A's)

## Summary

**Identity and Access Management (IAM)** is the discipline of ensuring the right identities have the right access to the right resources at the right time — and nothing more. Its foundations are often framed as the **"Five A's"**: **Identification** (claiming an identity, e.g. a username), **Authentication** (proving the claim, via something you know/have/are), **Authorization** (deciding what that authenticated identity may do), **Accountability/Auditing** (recording who did what, so actions are attributable and reviewable), and **Administration** (the lifecycle of provisioning, modifying, and de-provisioning identities and entitlements). Some models phrase these as Authentication, Authorization, Accounting, plus Administration and Auditing; the key ideas are the same. Two principles govern good IAM: **least privilege** (grant only the access needed) and **separation of duties** (no single identity can abuse a critical process alone). IAM is foundational to security because identity is the modern control point — strong authentication stops impersonation, correct authorization limits blast radius, auditing enables detection and forensics, and disciplined administration prevents the slow accumulation of excess access ("privilege creep") and orphaned accounts that attackers exploit. Getting these fundamentals right underlies more advanced identity topics (MFA, SSO, governance, just-in-time access) and aligns with Zero Trust, where every access request is continuously verified rather than trusted by network location.

## The Five A's

| Function | Question it answers |
|---|---|
| **Identification** | Who are you claiming to be? |
| **Authentication** | Can you prove it? |
| **Authorization** | What are you allowed to do? |
| **Accountability/Audit** | What did you actually do? |
| **Administration** | How are identities and rights managed over time? |

Least privilege and separation of duties run through all five: grant the minimum, and ensure no one identity can complete a sensitive action unchecked.

## Security relevance

- **Authentication stops impersonation**: weak auth undermines everything downstream.
- **Authorization limits blast radius**: least privilege means a compromised identity reaches less.
- **Auditing enables detection/forensics**: without accountability, misuse is invisible and unprovable.
- **Administration prevents drift**: privilege creep and orphaned accounts are prime attacker targets.

## Practices

- **Enforce least privilege and separation of duties** across all entitlements.
- **Centralize identity** (SSO/IdP) with strong, ideally phishing-resistant authentication.
- **Review access regularly** (recertification) and de-provision promptly on role change or departure.
- **Log and monitor identity events** for accountability and anomaly detection.
- **Adopt Zero Trust**: verify every request by identity and context, not network position.

## Explanation

Identity and Access Management is really just answering a few simple questions, carefully, every time someone tries to use a system. Who are you (identification)? Prove it (authentication). Okay, what are you allowed to touch (authorization)? We'll write down what you actually did (accountability), and someone has to manage your account as you join, change roles, and leave (administration). These "Five A's" sound obvious, but almost every identity-related breach is a failure of one of them: a password that was too easy to steal, permissions that were far broader than the job required, no logs to notice the misuse, or an account that should have been shut off months ago when the employee left. Two guardrails keep it healthy. Least privilege means giving each person only the keys they actually need, so a stolen account can't open every door. Separation of duties means splitting critical tasks so no single person — or hijacked account — can, say, both create a payment and approve it. Get these basics right and the fancier stuff (multi-factor, single sign-on, automated reviews) has a solid foundation to build on; get them wrong and no amount of advanced tooling will save you.
