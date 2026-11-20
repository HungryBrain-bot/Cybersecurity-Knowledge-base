# Cybersecurity Knowledge Base

A curated, source-verified reference of offensive and defensive cybersecurity concepts: how techniques work, how to detect them, how to defend against them, and how they map to [MITRE ATT&CK](https://attack.mitre.org/).

**374 concept notes across 8 domains and 65 categories**, every one written from primary sources and cross-checked before publishing. Accuracy is the priority: a claim that could not be confirmed against an authoritative source is removed or clearly marked, not guessed.

## Why this repo is worth your time

Most security notes online are either shallow definitions or walls of unverified copy-paste. This knowledge base is built differently, and that is the point:

- **Verified, not vibes.** Every technique ID, API name, event ID, registry path, command, and cloud behaviour is checked against MITRE ATT&CK and vendor primary documentation. Each file records exactly what it was checked against, so you can trust it or re-verify it yourself.
- **Attack *and* defence in one place.** A note doesn't just explain how an attack works — it pairs the technique with how to detect it and how to defend against it, and maps it to the ATT&CK tactic and technique IDs so it slots straight into real detection and threat-modelling work.
- **Breadth with depth.** Eight domains span endpoints (Windows, Linux, macOS), malware, network, web, and cloud, plus the cross-cutting fundamentals (cryptography, forensics, incident response, threat intel, identity) that tie them together.
- **Built to be navigated.** One concept per file, grouped by domain and category, each with a plain-language explanation and, where order or branching matters, a rendered flow diagram. Good for studying a topic, prepping for interviews or certs, or grabbing a quick, reliable refresher.
- **Original prose.** Written fresh — no scraped or copied text — so it reads as one coherent reference rather than a scrapbook.

Whether you are a blue-teamer building detections, a red-teamer or pentester mapping tradecraft, a student working toward OSCP/Security+/etc., or an engineer who just needs an accurate answer fast, this is meant to be a dependable starting point.

## How this is organised

Content is grouped into one folder per **domain**, then into subfolders by **category**, with one Markdown file per concept.

```
<domain>/<category>/<concept>.md
```

### Domains

| Folder | Concepts | Scope |
|---|---|---|
| [`windows/`](windows/) | 91 | Windows internals, authentication (Kerberos/NTLM), access control, auditing, process injection, persistence, platform security |
| [`linux/`](linux/) | 50 | Linux internals, privilege escalation, hardening, binary/ELF analysis, rootkits, scripting |
| [`macos/`](macos/) | 4 | macOS security topics |
| [`malware/`](malware/) | 37 | Malware analysis methodology, reverse engineering, capabilities, evasion and anti-analysis, rootkits |
| [`network/`](network/) | 24 | Network security, protocol analysis, C2, red-team infrastructure |
| [`web/`](web/) | 31 | Web application security, injection, XSS, authentication flaws |
| [`cloud/`](cloud/) | 26 | Cloud and container security (AWS, Azure, Kubernetes, Docker) |
| [`general/`](general/) | 112 | Cryptography, forensics, incident response, threat intelligence and hunting, identity and access, OPSEC, offensive-program management, and other cross-cutting topics |

Each domain folder has its own `README.md` indexing every concept in it, with ATT&CK IDs alongside each entry. A technique is filed under its **primary** domain and cross-referenced elsewhere rather than duplicated.

## What is in each file

Every concept file carries a verification front-matter block:

```yaml
verification: Verified | Needs review
attack_ids: [T1055.001]
checked_against: [<source URLs>]
open_issues: [<reasons, or empty>]
```

and, where the topic warrants it, sections for a summary, how it works, an attack/defence flow diagram, detection, defence, a plain-language explanation, and an ATT&CK mapping table. Diagrams are rendered and validated before they are committed, and they only show steps stated in the text.

## Sourcing and accuracy

- Technique IDs, names and tactics are checked against the current MITRE ATT&CK Enterprise dataset.
- Platform details (APIs, event IDs, registry paths, commands, cloud behaviour) are checked against vendor primary documentation (Microsoft Learn, AWS, Kubernetes, Docker, Linux man pages, NIST, IETF RFCs) and, where relevant, CVE records.
- Content is curated and written fresh from a personal cybersecurity knowledge base. It is original prose, not copied from any third-party text.
- Corrections found while verifying are logged in [`KB-CORRECTIONS.md`](KB-CORRECTIONS.md).

## How to use it

- **Browse by domain:** open a domain's `README.md` for a full, ATT&CK-tagged index, then jump to any concept.
- **Hunt by technique:** the ATT&CK IDs in each domain index and in the per-file mapping tables let you pivot from a technique ID to its explanation, detection, and defence.
- **Study a theme:** categories group related concepts (e.g. `general/cryptography/`, `windows/credential-access/`) so you can work through a subject end to end.

## Status

All 8 domains are published. The knowledge base will continue to be refined and extended; see each domain's `README.md` for its current contents.
