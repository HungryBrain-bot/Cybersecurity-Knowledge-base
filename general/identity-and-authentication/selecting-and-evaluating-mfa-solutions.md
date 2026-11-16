---
title: "Selecting and Evaluating MFA Solutions"
domain: [General]
category: [Identity & Authentication]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://pages.nist.gov/800-63-3/sp800-63b.html
  - https://fidoalliance.org/specifications/
open_issues: []
---

# Selecting and Evaluating MFA Solutions

## Summary

Not all **multi-factor authentication** is equally strong, so choosing an MFA solution means evaluating **factor type, resistance to known attacks, usability, coverage, and recoverability**. The factor hierarchy, from weakest to strongest: **SMS/voice OTP** (vulnerable to SIM swap and interception; NIST discourages it for high assurance), **TOTP authenticator apps** (better — not tied to the phone number — but still phishable because the code can be relayed), **push approval** (convenient but prone to MFA fatigue unless hardened with number matching), and **phishing-resistant** factors: **FIDO2/WebAuthn passkeys and hardware security keys**, which cryptographically bind the credential to the legitimate site's origin so a proxy phishing page cannot relay them. Evaluation criteria therefore include: **phishing resistance** (the decisive security property — prefer FIDO2/WebAuthn, especially for admins and high-value accounts), **assurance level** (aligning to NIST SP 800-63B AAL where applicable), **usability and adoption** (friction drives workarounds), **coverage** (does it protect all critical apps, including legacy and VPN?), **account recovery** (a weak recovery/reset path undermines strong MFA — the recovery flow is part of the threat model), **cost and manageability**, and **resistance to the specific attacks** the organization faces (AiTM, fatigue, SIM swap). The guiding conclusion is to deploy MFA broadly for breadth and phishing-resistant factors for depth on the highest-value identities, while ensuring the recovery and enrollment paths are as strong as the primary factor.

## Evaluation criteria

| Criterion | What to assess |
|---|---|
| **Phishing resistance** | FIDO2/WebAuthn > push > TOTP > SMS |
| **Assurance level** | NIST SP 800-63B AAL alignment |
| **Usability/adoption** | Friction vs. workaround risk |
| **Coverage** | All critical apps, VPN, legacy included |
| **Recovery/enrollment** | As strong as the primary factor? |
| **Cost/manageability** | Scale, support, lifecycle |

The decisive axis is phishing resistance; a strong primary factor is undermined by a weak recovery path, so evaluate the whole lifecycle.

## Security relevance

- **SMS is the weakest**: SIM swap and interception make it unsuitable for high-value accounts.
- **Phishable factors fall to AiTM**: TOTP and basic push can be relayed by proxy phishing; FIDO2 cannot.
- **Recovery is an attack path**: a help-desk reset or email recovery can bypass the strongest factor.
- **Coverage gaps**: unprotected legacy protocols and VPNs are the holes attackers seek.

## Practices

- **Deploy MFA everywhere**; prioritize phishing-resistant FIDO2/WebAuthn for admins and crown-jewel accounts.
- **Phase out SMS** for sensitive access; prefer app or hardware factors at minimum.
- **Harden push** with number matching and limited attempts.
- **Secure recovery/enrollment** to the same assurance as the primary factor; verify identity on resets.
- **Close coverage gaps**: protect VPN, email, and legacy auth; monitor for MFA bypass.

## Explanation

"Turn on MFA" is good advice, but it hides an important detail: some second factors are far tougher than others, and picking the right one is a real decision. At the weak end are text-message codes — convenient, but an attacker can hijack your phone number from your carrier or intercept the code, so security experts now steer important accounts away from SMS. Authenticator apps are a step up because they don't depend on your phone number, but the code on the screen can still be tricked out of you by a convincing fake site. Push notifications are easy to use but can be abused by bombarding you with approval prompts until you tap "yes." At the strong end are passkeys and hardware keys (the FIDO2/WebAuthn standard), which are special because they're cryptographically glued to the real website — show them a lookalike phishing page and they simply won't respond, which shuts down the most common modern attacks. When choosing, the top question is "can this be phished?", but it's not the only one: will people actually use it without finding workarounds, does it cover every important app and not just the easy ones, and — a frequently forgotten trap — is the "I lost my device" recovery process as strong as the login itself? A fortress front door means little if the back door opens with a phone call to the help desk.
