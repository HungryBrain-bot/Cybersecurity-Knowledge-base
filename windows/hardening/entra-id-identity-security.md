---
title: "Azure AD Identity Security (SSO, MFA, Windows Hello, Conditional Access)"
domain: [Windows, Cloud]
category: [Identity & Authentication]
difficulty: Intermediate
verification: Verified
attack_ids: [T1078.004, T1110, T1621, T1556.006]
checked_against:
  - https://learn.microsoft.com/en-us/entra/fundamentals/new-name
  - https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview
  - https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/
  - https://attack.mitre.org/techniques/T1078/004/
  - https://attack.mitre.org/techniques/T1110/
  - https://attack.mitre.org/techniques/T1621/
  - https://attack.mitre.org/techniques/T1556/006/
open_issues: []
---

# Azure AD Identity Security (SSO, MFA, Windows Hello, Conditional Access)

## Summary

In hybrid Windows and cloud environments, the identity service is the real control plane: whoever controls an account controls the mail, files and admin portals behind it. Microsoft renamed **Azure Active Directory to Microsoft Entra ID**; the concepts here apply to both names. The core defences are **single sign-on** so there is one strong credential instead of many weak ones, **multifactor authentication**, **passwordless** sign-in with **Windows Hello for Business**, and **Conditional Access**, which decides per sign-in whether to allow, challenge or block based on who, from where, on what device and at what risk.

## Building blocks

| Control | What it does | Attack it blunts |
|---|---|---|
| **Directory integration and lifecycle** | Accounts created and removed from an authoritative HR source; guest accounts governed | Orphaned and stale accounts used as valid credentials (T1078.004) |
| **Single sign-on** | One identity across apps | Password reuse and sprawl |
| **MFA** | Second factor at sign-in | Password spraying and guessing (T1110); weak factors remain vulnerable to push fatigue (T1621) |
| **Windows Hello for Business** | Replaces passwords with key- or certificate-based authentication; private key protected by the device's security module and unlocked by PIN or biometric | Phishing and server-side password theft; there is no shared secret to steal |
| **Conditional Access** | If-then policies combining signals (user or group, IP location, device, real-time and calculated risk) to block access or require MFA | Sign-ins from unmanaged devices, risky locations or compromised sessions |
| **Admin role protection** | Few permanent admins; separate admin accounts; strong MFA for roles | Takeover of privileged cloud roles |

## Practical policy set

1. Require MFA for all users, with phishing-resistant methods (Windows Hello for Business, FIDO2) for administrators.
2. Block legacy authentication protocols that cannot do MFA.
3. Require compliant or hybrid-joined devices for access to sensitive apps.
4. Use risk-based policies: force password change or block on high user or sign-in risk.
5. Exclude emergency ("break-glass") accounts from policies deliberately, and monitor them closely.
6. Review guest access and admin role assignments regularly.

## Attacker view

- Password spraying against cloud sign-in endpoints (T1110).
- MFA fatigue: repeated push prompts until a user approves (T1621).
- Modifying the authentication process, for example registering attacker-controlled MFA methods or altering hybrid identity configuration (T1556.006 is the ATT&CK sub-technique for MFA tampering).
- Abusing stale cloud accounts (T1078.004).

## ATT&CK mapping

- **T1078.004 Cloud Accounts**
- **T1110 Brute Force**
- **T1621 Multi-Factor Authentication Request Generation**
- **T1556.006 Multi-Factor Authentication**

## Explanation

Identity in the cloud is the company pass. Single sign-on means each person carries one pass instead of a ring of keys; MFA means the pass only works with a fingerprint too; Windows Hello makes the pass a chip that never leaves the holder's phone or laptop, so there is nothing to copy. Conditional Access is the guard at each door who looks at the pass, the person, the time and where they came from before deciding whether to wave them in, ask for more proof, or call security.
