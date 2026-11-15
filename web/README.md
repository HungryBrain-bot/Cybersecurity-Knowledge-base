# Web

Web application security: how web apps are built, how they are attacked, and how they are defended. Content is curated and written fresh from the knowledge base, cross-checked against [MITRE ATT&CK](https://attack.mitre.org/), the [OWASP Top 10 (2025)](https://owasp.org/Top10/2025/), the [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/), the [OWASP API Security Top 10](https://owasp.org/API-Security/) and the [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/).

Each file carries a `verification` front-matter block listing the primary sources it was checked against.

## Foundations and methodology

| Concept | ATT&CK |
|---|---|
| [Web Application Core Defense Mechanisms and Technologies](web-security/web-application-core-defense-mechanisms.md) | — |
| [Web Application Architecture and Web Server Attacks](web-security/web-application-architecture-and-web-server-attacks.md) | T1190, T1505.003 |
| [Web Application Penetration Testing Methodology](web-security/web-application-penetration-testing-methodology.md) | — |

## Discovery and recon

| Concept | ATT&CK |
|---|---|
| [Web Application Mapping and Content Discovery](discovery/web-application-mapping-and-content-discovery.md) | T1595.003, T1594, T1596 |

## Injection and server-side attacks

| Concept | ATT&CK |
|---|---|
| [SQL Injection](exploitation/sql-injection.md) | T1190 |
| [Other Injection Attacks (NoSQL, XPath, LDAP, OS Command, File Inclusion)](exploitation/other-injection-attacks.md) | T1190, T1059, T1505.003 |
| [XML External Entity (XXE) Injection](exploitation/xml-external-entity-xxe-injection.md) | T1190, T1552.005 |
| [Server-Side Template Injection](exploitation/server-side-template-injection.md) | T1190, T1059 |
| [Insecure Deserialization](exploitation/insecure-deserialization.md) | T1190 |
| [Out-of-Band Exploitation and Blind Vulnerability Detection](exploitation/out-of-band-exploitation-and-blind-vulnerability-detection.md) | T1190, T1048, T1071.004 |

## Client-side attacks

| Concept | ATT&CK |
|---|---|
| [Cross-Site Scripting (XSS)](exploitation/cross-site-scripting-xss.md) | T1059.007, T1189 |
| [Other Client-Side Attacks (CSRF, Clickjacking, Same-Origin Policy)](exploitation/other-client-side-attacks-csrf-clickjacking-sop.md) | T1185 |
| [Open Redirects](web-security/open-redirects.md) | T1566.002 |

## Authentication, sessions and access control

| Concept | ATT&CK |
|---|---|
| [Authentication Attacks (Web)](web-security/authentication-attacks-web.md) | T1110, T1110.003, T1110.004 |
| [Web Session Management Attacks](web-security/web-session-management-attacks.md) | T1539, T1185 |
| [Broken Access Control](web-security/broken-access-control.md) | — |
| [Single Sign-On Security Issues](web-security/single-sign-on-security-issues.md) | T1606, T1550.001 |
| [API Hacking (REST, GraphQL and Authorization Flaws)](web-security/api-hacking-rest-graphql-authorization.md) | T1190 |

## Logic, configuration and disclosure

| Concept | ATT&CK |
|---|---|
| [Application Logic Flaws](web-security/application-logic-flaws.md) | — |
| [Race Conditions in Web Applications](exploitation/race-conditions-in-web-applications.md) | — |
| [Client-Side Control Bypass](web-security/client-side-control-bypass.md) | — |
| [Information Disclosure and Error Handling Exploitation](web-security/information-disclosure-and-error-handling.md) | T1592, T1589 |

## Platforms and native targets

| Concept | ATT&CK |
|---|---|
| [Attacking Content Management Systems (CMS)](exploitation/attacking-content-management-systems.md) | T1190, T1505.003 |
| [Attacking Native Compiled Web Applications](exploitation/attacking-native-compiled-web-applications.md) | T1190 |
| [Modern Web Application Attack Techniques for Red Teams](exploitation/modern-web-application-attack-techniques-for-red-teams.md) | T1190, T1557 |

## Tooling and testing technique

| Concept | ATT&CK |
|---|---|
| [Automating Customized Web Attacks (Fuzzing with Burp)](web-security/automating-customized-web-attacks-fuzzing-with-burp.md) | T1595 |
| [Python Web Attack Tooling and Burp Extensions](web-security/python-web-attack-tooling-and-burp-extensions.md) | — |
| [Source Code Review for Web Vulnerabilities](web-security/source-code-review-for-web-vulnerabilities.md) | — |
| [Bug Bounty Programs and Vulnerability Reporting](web-security/bug-bounty-programs-and-vulnerability-reporting.md) | — |

## Defensive hardening

| Concept | ATT&CK |
|---|---|
| [SQL Injection Prevention and Defensive Solutions](web-security/sql-injection-prevention-and-defensive-solutions.md) | — |
| [Web Server HTTPS and TLS Hardening](web-security/web-server-https-and-tls-hardening.md) | T1557, T1040 |
