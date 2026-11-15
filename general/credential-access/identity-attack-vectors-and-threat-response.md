---
title: "Identity Attack Vectors and Identity-Based Threat Response"
domain: [General]
category: [Credential Access, Identity & Authentication]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/tactics/TA0006/
  - https://pages.nist.gov/800-63-3/sp800-63b.html
open_issues: []
---

# Identity Attack Vectors and Identity-Based Threat Response

## Summary

As organizations moved to cloud and remote work, **identity became the primary security perimeter**: with resources reachable from anywhere, whoever controls a valid identity controls its access, which is why credential and identity abuse now feature in a large share of breaches. The **attack vectors** against identity cluster into a few families: **credential theft** (phishing, infostealers, keyloggers, breach reuse), **credential cracking/guessing** (brute force, password spraying, stuffing), **authentication bypass and MFA defeat** (token theft, session-cookie theft, MFA fatigue/push bombing, SIM swap), **privilege abuse** (standing excessive permissions, privilege escalation, role misconfiguration), and **trust abuse** (federation and SSO manipulation, OAuth consent phishing). Because these attacks use *legitimate* credentials and flows, they often evade controls tuned for malware, so **identity-based threat response** centers on behavior and context: detecting anomalous sign-ins (impossible travel, new device/geo, atypical times), risky session and token activity, sudden privilege changes, and dormant-account use. The defensive model is **identity-centric**: strong phishing-resistant MFA, least privilege and just-in-time access, continuous monitoring of identity telemetry, rapid credential/session revocation during response, and conditional access that weighs risk at authentication time. The throughline is that protecting identities — not just networks — is the modern control point, and responding to identity threats means watching how identities *behave*, since the credentials themselves look valid.

## Identity attack vector families

| Family | Examples |
|---|---|
| **Credential theft** | Phishing, infostealers, breach reuse |
| **Cracking/guessing** | Brute force, spraying, stuffing |
| **Auth/MFA bypass** | Token/cookie theft, MFA fatigue, SIM swap |
| **Privilege abuse** | Standing excess rights, role misconfig |
| **Trust abuse** | SSO/federation manipulation, OAuth consent phishing |

Because the attacker wields valid credentials, detection shifts from "is this malware?" to "is this identity behaving normally?"

## Detection / value

- **Behavioral anomalies**: impossible travel, new device/geo, odd hours, and dormant-account reactivation flag misuse of valid credentials.
- **Session/token signals**: reused or exported session cookies and unusual token activity reveal bypass of MFA.
- **Privilege drift**: sudden role or permission changes indicate escalation or misconfiguration abuse.
- **Consent/federation events**: unexpected OAuth grants or federation changes signal trust abuse.

## Defense / practices

- **Phishing-resistant MFA** (FIDO2/WebAuthn) to blunt theft, fatigue, and interception.
- **Least privilege + just-in-time access**; eliminate standing excessive rights.
- **Monitor identity telemetry** and apply risk-based conditional access at sign-in.
- **Fast revocation in IR**: invalidate credentials, sessions, and tokens; rotate secrets.
- **Protect the identity provider** itself — it is the keys to the kingdom.

## Explanation

The walls of the old corporate network have largely dissolved — people work from home, data lives in the cloud, and apps are reachable from any coffee shop. In that world, the thing that actually decides who gets in is no longer a firewall but an identity: a username, its password, and its permissions. That's why attackers increasingly skip the hard work of hacking software and simply log in, using stolen or guessed credentials. They phish passwords, buy them from breaches, trick people into approving login prompts, steal the "remember me" session cookies that bypass passwords entirely, or quietly accumulate more permissions than they should have. The tricky part for defenders is that none of this looks like an attack at the moment it happens — it looks like a normal login, because it *is* one, just by the wrong person. So defending identities is less about spotting bad software and more about spotting bad behavior: a login from a country the user has never visited, a session cookie suddenly used from a new machine, an account that's been dormant for a year springing to life at 3 a.m. The strongest moves are to make credentials hard to steal in the first place (phishing-resistant MFA), to give every identity only the access it truly needs, and, when something goes wrong, to be able to yank an identity's credentials and active sessions instantly.
