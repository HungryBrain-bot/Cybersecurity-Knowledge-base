---
title: Web Server HTTPS and TLS Hardening
domain: [Web]
category: [Web Security, Hardening]
difficulty: Intermediate
verification: Verified
attack_ids: [T1557, T1040]
checked_against:
  - https://attack.mitre.org/techniques/T1557/
  - https://attack.mitre.org/techniques/T1040/
  - https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html
  - https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html
open_issues: []
---

# Web Server HTTPS and TLS Hardening

## Summary

TLS (Transport Layer Security) protects web traffic in transit: it gives confidentiality, integrity and server authentication so a network attacker cannot read or tamper with requests and responses. Hardening TLS means removing the weak options a server will otherwise accept. A misconfigured HTTPS endpoint can still be downgraded, intercepted or impersonated, so "has a padlock" is not the same as "configured safely". The threat it defends against is a network position between client and server used for sniffing (T1040) or adversary-in-the-middle (T1557).

## What weak TLS lets an attacker do

| Weakness | Consequence |
|---|---|
| **Obsolete protocols** (SSLv3, TLS 1.0/1.1) | Known attacks (POODLE, BEAST) and no modern cipher guarantees |
| **Weak ciphers** (RC4, 3DES, export, NULL, anonymous) | Traffic decryptable or unauthenticated |
| **No forward secrecy** | One stolen private key decrypts all past captured sessions |
| **No HSTS** | First request can be stripped to HTTP and intercepted (T1557) |
| **Mixed content** | HTTPS page loads scripts over HTTP, reopening the channel |
| **Bad certificate** (expired, wrong name, weak key, untrusted CA) | Users trained to click through warnings; impersonation easier |

## Hardened configuration

```mermaid
flowchart TD
  A1["Attacker on the network path"]:::attack --> A2{"Can they weaken the channel?"}
  A2 -- "Downgrade to HTTP" --> A3["Strip TLS, sniff/alter: T1557"]:::attack
  A2 -- "Force weak cipher/protocol" --> A4["Decrypt captured traffic: T1040"]:::attack
  M1["TLS 1.2+ only, strong ciphers, forward secrecy"]:::defend -.-> A4
  M2["HSTS with preload, redirect all HTTP to HTTPS"]:::defend -.-> A3
  M3["Valid cert, strong key, OCSP stapling"]:::defend -.-> A2
  classDef attack fill:#fde2e2,stroke:#c0392b
  classDef defend fill:#e2fde8,stroke:#1e8449
```

## Detection

- TLS scanners (such as `testssl.sh` or `sslyze`) reporting enabled legacy protocols, weak ciphers or missing forward secrecy.
- Clients negotiating TLS 1.0/1.1 in server logs, indicating old or attacker-forced sessions.
- Certificate monitoring: upcoming expiry, unexpected issuance (certificate transparency logs), or name mismatches.
- Missing `Strict-Transport-Security` header in responses.

## Defense

- **Enable only TLS 1.2 and TLS 1.3**; disable SSLv2/v3 and TLS 1.0/1.1.
- **Use strong cipher suites with forward secrecy** (ECDHE key exchange, AEAD ciphers such as AES-GCM or ChaCha20-Poly1305); disable RC4, 3DES, export, NULL and anonymous suites.
- **Send HSTS**: `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`, and redirect all HTTP to HTTPS.
- **Use a valid certificate** from a trusted CA with a strong key (RSA 2048+ or ECDSA P-256+), automate renewal, and enable OCSP stapling.
- **Eliminate mixed content**; serve every sub-resource over HTTPS.
- **Re-scan after changes** and track the configuration so it does not drift.

## ATT&CK mapping

| ID | Name | Tactic |
|---|---|---|
| T1557 | Adversary-in-the-Middle | Credential Access, Collection |
| T1040 | Network Sniffing | Credential Access, Discovery |

## Explanation

HTTPS is an armoured pipe between the browser and the server. Just fitting the pipe is not enough: if the server still accepts rusted old sections (obsolete protocols), thin walls (weak ciphers), or lets a caller start in plain pipe before switching (no HSTS), someone standing on the line can still listen in or splice themselves in. Hardening is insisting on one modern, thick-walled pipe, always, with a verified nameplate on the far end.
