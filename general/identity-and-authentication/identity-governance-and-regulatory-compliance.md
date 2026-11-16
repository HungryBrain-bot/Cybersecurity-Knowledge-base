---
title: "Identity Governance and Regulatory Compliance"
domain: [General]
category: [Identity & Authentication]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://pages.nist.gov/800-63-3/
  - https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
open_issues: []
---

# Identity Governance and Regulatory Compliance

## Summary

**Identity Governance and Administration (IGA)** is the policy, process, and oversight layer that ensures access rights are appropriate, visible, and provably controlled over time — not just technically enforced at login. Where IAM authenticates and authorizes, **governance answers "who has access to what, why, and is it still appropriate?"** Its core activities are **access certification/recertification** (periodic manager/owner review and attestation of entitlements), **role management** (defining roles so access is granted by job function rather than ad hoc), **segregation-of-duties (SoD) enforcement** (preventing toxic combinations like create-and-approve-payment), **access request and approval workflows**, **provisioning/de-provisioning lifecycle** (joiner-mover-leaver), and **analytics/reporting** to find excess, dormant, and orphaned access. Governance is tightly coupled to **regulatory compliance** because many frameworks mandate exactly these controls: SOX (financial access and SoD), HIPAA (healthcare data access), PCI DSS (cardholder-data access), GDPR (personal-data access and minimization), and general frameworks (NIST SP 800-53, ISO 27001) require least privilege, access reviews, and auditable records. The security value is direct: governance combats **privilege creep** (access accumulating as people change roles), **orphaned accounts** (departed users still enabled), and **standing excessive access** — all prime targets for attackers and insiders — while producing the audit evidence regulators require. In short, IGA turns access control from a one-time grant into an ongoing, reviewable, and compliant process.

## Governance activities

| Activity | Purpose |
|---|---|
| **Access certification** | Periodic review/attestation of who has what |
| **Role management** | Access by job function, not ad hoc |
| **SoD enforcement** | Block toxic permission combinations |
| **Request/approval workflow** | Controlled, auditable grants |
| **Joiner-mover-leaver** | Lifecycle provisioning/de-provisioning |
| **Analytics/reporting** | Surface excess, dormant, orphaned access |

Governance is what keeps least privilege true over time and generates the evidence compliance frameworks demand.

## Security relevance

- **Privilege creep**: without recertification, access accumulates until accounts are over-entitled.
- **Orphaned accounts**: ungoverned leaver processes leave live credentials for attackers.
- **SoD violations**: ungoverned combinations enable fraud and single-actor abuse.
- **Audit failure**: without governance records, organizations can't prove compliance or reconstruct who had access.

## Practices

- **Run regular access recertifications** with accountable owners; remove unneeded access.
- **Define roles** and grant by function; minimize direct, ad hoc entitlements.
- **Automate joiner-mover-leaver** so access tracks employment and role changes.
- **Enforce SoD** rules and alert on violations.
- **Map controls to frameworks** (SOX/HIPAA/PCI/GDPR/ISO 27001) and keep auditable evidence.

## Explanation

Giving someone access is easy; the hard part is making sure that, months and years later, they still *should* have it — and being able to prove it to an auditor. That ongoing discipline is identity governance. People change jobs within a company and quietly keep the permissions from every previous role, a pile-up called "privilege creep" that turns ordinary accounts into skeleton keys. People leave, and if the offboarding is sloppy, their accounts stay alive — a gift to anyone who wants a way in. Governance fixes this with routines: every so often, managers must actually review and sign off on who has access to what, roles bundle permissions by job so grants aren't random, and automated processes add and remove access the moment someone joins, moves, or leaves. It also blocks dangerous combinations — the classic example being one person who can both create a payment and approve it, which is an invitation to fraud. All of this does double duty: it shrinks the access an attacker or insider could abuse, and it produces the paper trail that laws and standards like SOX, HIPAA, PCI, and GDPR require. Governance is simply the difference between handing out keys and actually running a key-control program.
