---
title: "Intelligence Basics and the Intelligence Cycle"
domain: [General]
category: [Threat Intelligence]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://www.cisa.gov/topics/cyber-threats-and-advisories
  - https://attack.mitre.org/resources/
open_issues: []
---

# Intelligence Basics and the Intelligence Cycle

## Summary

**Cyber threat intelligence (CTI)** is evidence-based knowledge about threats that is **processed and analyzed to support decisions** — the key distinction being between raw **data** (a log line), **information** (an aggregated feed of indicators), and **intelligence** (analyzed, contextualized knowledge that answers a decision-maker's question). CTI is produced through the **intelligence cycle**, a repeatable process adapted from traditional intelligence: **Planning & Direction** (define requirements — what questions must be answered, for whom), **Collection** (gather data from sources: OSINT, internal telemetry, feeds, dark web, sharing partners), **Processing** (normalize, deduplicate, and structure the raw data), **Analysis & Production** (turn processed information into intelligence — assess relevance, reliability, and meaning, and produce a finished product), **Dissemination** (deliver intelligence to the right consumers in the right form), and **Feedback** (consumers evaluate usefulness, refining the next cycle). CTI is commonly divided into levels: **strategic** (big-picture trends and risk for executives), **operational** (campaigns and adversary intent for defenders and management), and **tactical** (technical IOCs and TTPs for SOC/detection). The defining principle is that intelligence must be **actionable and requirements-driven** — collecting indiscriminately produces noise, whereas starting from "what decision does this support?" produces intelligence people actually use. Done well, the cycle transforms the flood of threat data into prioritized, relevant knowledge that guides detection, hunting, defense investment, and executive risk decisions.

## The intelligence cycle

| Stage | Purpose |
|---|---|
| **Planning & Direction** | Define requirements and consumers |
| **Collection** | Gather from OSINT, telemetry, feeds, partners |
| **Processing** | Normalize, deduplicate, structure |
| **Analysis & Production** | Assess and produce finished intelligence |
| **Dissemination** | Deliver to the right consumers |
| **Feedback** | Refine the next cycle |

The difference between data and intelligence is analysis and relevance: requirements-driven work produces knowledge people act on, not noise.

## Intelligence levels

| Level | Audience | Content |
|---|---|---|
| **Strategic** | Executives | Trends, risk, big picture |
| **Operational** | Defenders/management | Campaigns, adversary intent |
| **Tactical** | SOC/detection | IOCs, TTPs, technical detail |

## Detection / value

- **Actionability**: requirements-driven intelligence supports real decisions, unlike undirected feeds.
- **Prioritization**: analysis separates relevant threats from background noise.
- **Right audience, right level**: strategic/operational/tactical framing delivers useful intelligence to each consumer.
- **Continuous improvement**: feedback refines collection and analysis over time.

## Practices

- **Start from requirements**: define what decisions the intelligence must support and for whom.
- **Collect purposefully** from diverse sources; avoid indiscriminate feed ingestion.
- **Analyze for relevance and reliability**; produce finished, contextual intelligence, not raw dumps.
- **Disseminate by level** (strategic/operational/tactical) and gather feedback to refine the cycle.

## Explanation

People throw around the word "intelligence" loosely, but in the threat world it means something specific: not raw data, and not even a big pile of information, but *analyzed knowledge that helps someone make a decision*. A single suspicious IP address is data. A feed of ten thousand suspicious IPs is information. Intelligence is when an analyst looks at that feed, filters it for what's actually relevant to your organization, explains what it means, and tells your security team or your executives something they can act on. Producing that takes a disciplined loop called the intelligence cycle. It starts not with collecting, but with asking the right question — what decision are we trying to support, and for whom? Only then do you gather data, clean it up, analyze it into genuine insight, deliver it to the people who need it in a form they can use, and finally ask them whether it was helpful so you can do better next time. That starting question is everything; teams that skip it and just hoover up every threat feed drown in noise and wonder why none of it helps. And because different audiences need different things — a CEO wants the big-picture risk, a SOC analyst wants the specific technical indicators — good intelligence comes in strategic, operational, and tactical flavors, each pitched to its reader.
