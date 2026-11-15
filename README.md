# Cybersecurity Knowledge Base

A curated, source-verified reference of offensive and defensive cybersecurity concepts: how techniques work, how to detect them, how to defend against them, and how they map to [MITRE ATT&CK](https://attack.mitre.org/).

Every file is written from primary sources and cross-checked before publishing. Accuracy is the priority: a claim that could not be confirmed against an authoritative source is removed or clearly marked, not guessed.

## How this is organised

Content is grouped into one folder per **domain**, then into subfolders by **category**, with one Markdown file per concept.

```
<domain>/<category>/<concept>.md
```

### Domains

| Folder | Scope |
|---|---|
| [`windows/`](windows/) | Windows internals, authentication, access control, auditing, platform security |
| [`linux/`](linux/) | Linux internals, hardening, binary analysis, scripting |
| [`macos/`](macos/) | macOS security topics |
| [`malware/`](malware/) | Malware analysis, reverse engineering, capabilities, evasion, rootkits |
| [`network/`](network/) | Network security, protocol analysis, C2, red-team infrastructure |
| [`web/`](web/) | Web application security, injection, XSS |
| [`cloud/`](cloud/) | Cloud and container security (AWS, Azure, Kubernetes, Docker) |
| [`general/`](general/) | Cryptography, forensics, incident response, threat intelligence, identity, OPSEC, and cross-cutting topics |

Each domain folder has its own `README.md` listing the concepts in it.

## What is in each file

Every concept file carries a verification front-matter block:

```yaml
verification: Verified | Needs review
attack_ids: [T1055.001]
checked_against: [<source URLs>]
open_issues: [<reasons, or empty>]
```

and, where the topic warrants it, sections for a summary, how it works, an attack/defence flow diagram, detection, defence, and an ATT&CK mapping table. Diagrams are rendered and validated before they are committed, and they only show steps stated in the text.

## Sourcing and accuracy

- Technique IDs, names and tactics are checked against the current MITRE ATT&CK Enterprise dataset.
- Platform details (APIs, event IDs, registry paths, commands, cloud behaviour) are checked against vendor primary documentation (Microsoft Learn, AWS, Kubernetes, Docker, Linux man pages) and, where relevant, CVE records.
- Content is curated and written fresh from Venu's personal cybersecurity knowledge base. It is original prose, not copied from any third-party text.
- Corrections found while verifying are logged in [`KB-CORRECTIONS.md`](KB-CORRECTIONS.md).

## Status

This repository is built and published in batches, one domain at a time. See each domain's `README.md` for what is currently covered.
