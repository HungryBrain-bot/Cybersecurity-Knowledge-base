---
title: "Pre-Incident Preparation"
domain: [General]
category: [Incident Response, Hardening]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf
  - https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
open_issues: []
---

# Pre-Incident Preparation

## Summary

**Preparation** is the phase of incident response that most determines how well every later phase goes, because you cannot acquire the people, tooling, visibility, and relationships you need in the middle of a crisis. It has two sides: **preparing the organization** (reducing the chance and impact of incidents) and **preparing the response capability** (being ready to handle them). On the organizational side: hardening and patching, network segmentation, least privilege, and — critically — **logging and telemetry**, because you cannot investigate what you never recorded (endpoint/EDR logs, authentication logs, network flows, and centralized retention are the raw material of every investigation). On the response-capability side: a designated **incident response team** with defined roles, an **IR plan and playbooks**, **tooling** (forensic and analysis tools staged and licensed), **access** (break-glass credentials, out-of-band communications in case primary systems are compromised), **training and tabletop exercises**, and **relationships** (legal, PR, law enforcement, external IR retainer, and executive sponsorship). Preparation also includes **backups and recovery readiness** (tested, immutable backups) so recovery is possible. The unifying insight is that incidents are won or lost before they start: an organization with rich logs, rehearsed playbooks, staged tools, and clear roles investigates and recovers in hours; one without them spends days just trying to figure out what happened and who is in charge, while the attacker operates freely.

## Preparation areas

| Area | Examples |
|---|---|
| **Visibility** | EDR, auth logs, network flows, central retention |
| **Hardening** | Patching, segmentation, least privilege |
| **IR capability** | Team, roles, plan, playbooks |
| **Tooling & access** | Staged forensic tools, break-glass creds, OOB comms |
| **Readiness** | Training, tabletops, tested immutable backups |
| **Relationships** | Legal, PR, law enforcement, IR retainer |

You cannot build visibility or relationships mid-incident; preparation is where response outcomes are actually decided.

## Detection / value

- **Logging determines investigability**: absent telemetry, scoping and root-cause are guesswork.
- **Rehearsed plans cut response time**: tabletop-tested playbooks turn chaos into execution.
- **Out-of-band comms matter**: if email/chat are compromised, responders need a trusted channel.
- **Tested backups enable recovery**: preparation here is the difference between restore and ransom.

## Practices

- **Centralize and retain logs** (endpoint, auth, network) sufficient to investigate; verify coverage.
- **Harden proactively**: patch, segment, enforce least privilege and MFA.
- **Build the IR capability**: team, roles, plan, playbooks, and staged tooling.
- **Prepare access and comms**: break-glass credentials and out-of-band channels.
- **Rehearse** with tabletop exercises; maintain tested, immutable backups and external relationships.

## Explanation

Here's an uncomfortable truth about incident response: most of whether you'll handle a breach well is decided before the breach ever happens. You can't go collect six months of logs after the attacker has come and gone, and you can't build a coordinated team, sign a legal retainer, and learn your forensic tools while the house is on fire. Preparation is doing all of that in the calm beforehand. The most underrated piece is simply recording what your systems do — logs from laptops, servers, logins, and network traffic — because an investigation is only as good as the evidence that was captured, and you cannot examine something you never wrote down. Alongside that sits the human readiness: a named team who knows their roles, written playbooks for the likely disasters, forensic tools already installed and licensed, emergency "break-glass" credentials, and a backup way to communicate in case the attacker is reading your email. You rehearse it all with tabletop drills, the way a fire department practices, so that when it's real, muscle memory takes over. And you keep tested, tamper-proof backups so that if the worst happens, you can rebuild rather than pay a ransom. The organizations that sail through incidents aren't luckier; they did this unglamorous homework while nothing was wrong.
