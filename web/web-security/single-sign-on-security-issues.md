---
title: Single Sign-On Security Issues
domain: [Web]
category: [Identity & Authentication, Web Security]
difficulty: Intermediate
verification: Verified
attack_ids: [T1606, T1550.001]
checked_against:
  - https://attack.mitre.org/techniques/T1606/
  - https://attack.mitre.org/techniques/T1550/001/
  - https://cheatsheetseries.owasp.org/cheatsheets/SAML_Security_Cheat_Sheet.html
  - https://datatracker.ietf.org/doc/html/rfc6819
open_issues: []
---

# Single Sign-On Security Issues

## Summary

Single sign-on (SSO) lets a user authenticate once with an identity provider (IdP) and access many applications (relying parties) using tokens, built on protocols like SAML, OAuth 2.0 and OpenID Connect (OIDC). SSO concentrates trust in those tokens and the flows that issue them, so the security issues are about token validation, flow integrity, and trust configuration. A forged or mishandled token can impersonate any user (ATT&CK T1606, Forge Web Credentials; T1550.001, Application Access Token).

## Where SSO goes wrong

| Issue | What happens |
|---|---|
| **Weak token validation** | A relying party does not properly verify the token's signature, issuer, audience or expiry, so a forged or altered token is accepted |
| **SAML signature flaws** | XML signature wrapping or not verifying the assertion signature lets an attacker change the asserted identity |
| **Audience/issuer confusion** | A token issued for one app or IdP is accepted by another (token reuse across trust boundaries) |
| **Loose `redirect_uri`** | An open or wildcard OAuth redirect lets an attacker capture authorization codes or tokens (see [Open Redirects](open-redirects.md)) |
| **Missing state/PKCE** | No CSRF protection (`state`) or no PKCE on public clients enables code interception and login CSRF |
| **Replay** | A token or assertion without proper expiry/one-time use is replayed |
| **IdP-mix / confused deputy** | Trusting an attacker-controlled IdP, or mixing up which IdP a response came from |

## Attack flow

```mermaid
flowchart TD
  A1["Target the SSO flow or token"]:::attack --> A2{"Weak point?"}
  A2 -- "Unverified signature/claims" --> A3["Forge or alter assertion: T1606"]:::attack
  A2 -- "Loose redirect_uri / no state" --> A4["Steal authorization code or token"]:::attack
  A2 -- "No expiry/one-time use" --> A5["Replay token"]:::attack
  A3 --> A6["Authenticate as any user / use token: T1550.001"]:::attack
  A4 --> A6
  A5 --> A6
  M1["Verify signature, issuer, audience, expiry; exact redirect_uri; PKCE+state"]:::defend -.-> A2
  classDef attack fill:#fde2e2,stroke:#c0392b
  classDef defend fill:#e2fde8,stroke:#1e8449
```

## Detection

- Tokens accepted with invalid or missing signatures, wrong audience/issuer, or past expiry (instrument the relying party's validation).
- OAuth authorization requests with unexpected `redirect_uri` values, or missing `state`/PKCE.
- Logins for a user from an IdP or assertion that does not match expected configuration.
- Replays: the same assertion/code presented more than once.

## Defense

- **Validate tokens fully** at every relying party: signature against the IdP's current keys, plus `issuer`, `audience`, `expiry` and `nonce` where applicable. For SAML, verify the assertion signature and guard against XML signature wrapping.
- **Exact-match `redirect_uri`** against pre-registered values; no wildcards or open redirects.
- **Use `state` and PKCE** for OAuth/OIDC authorization-code flows; prefer authorization code over implicit.
- **Enforce short expiry and one-time use** for assertions/codes to prevent replay.
- **Pin trusted IdPs and keys**; rotate and validate signing keys.
- Protect the IdP heavily, since compromising it compromises every connected app.

## ATT&CK mapping

| ID | Name | Tactic |
|---|---|---|
| T1606 | Forge Web Credentials | Credential Access |
| T1550.001 | Use Alternate Authentication Material: Application Access Token | Lateral Movement |

## Explanation

SSO replaces many passwords with one trusted stamp of identity. The risk moves to that stamp: if an app does not check the stamp carefully — its signature, who issued it, who it is for, whether it has expired — an attacker who can forge or steal one walks into every app as anyone. Good SSO is relentless about verifying the stamp and about never sending it to the wrong address.
