---
title: "Data Quality and Hunting Program Metrics"
domain: [General]
category: [Threat Hunting, Logging & Monitoring]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/datasources/
  - https://www.sans.org/white-papers/
open_issues: []
---

# Data Quality and Hunting Program Metrics

## Summary

Threat hunting is fundamentally **constrained by the data available** — you cannot find what you never recorded — so **data quality** and **program metrics** are what separate an effective hunting capability from a busy but blind one. **Data quality** has several dimensions: **coverage** (do you collect the telemetry needed to hunt the techniques in your threat model? — mapping data sources to ATT&CK reveals gaps), **completeness** (are logs captured from all relevant hosts, not just some?), **fidelity/granularity** (does the data contain the fields needed — e.g. command-line arguments, parent process — not just coarse events?), **timeliness** (is data available quickly enough to act?), **retention** (is it kept long enough to hunt historically, given long dwell times?), and **consistency/normalization** (is it structured so it can be queried reliably?). Poor data quality silently undermines hunting: a hunt that "finds nothing" may mean the threat isn't there — or that the data to see it was never collected. **Program metrics** measure whether the hunting program delivers value: number and outcomes of hunts, new detections created from hunts, dwell-time reduction, coverage improvements (techniques now huntable), and gaps identified and closed. The key insight is that metrics should emphasize **outcomes and improvement** (detections built, visibility gained, risks reduced) rather than vanity counts (hunts run), and that investing in data quality often yields more detection improvement than any single hunt. Together, data-quality management and meaningful metrics turn hunting from ad hoc activity into a measurable, improving program grounded in the visibility it actually has.

## Data quality dimensions

| Dimension | Question |
|---|---|
| **Coverage** | Do we collect data for the techniques we hunt? |
| **Completeness** | Are all relevant hosts logging? |
| **Fidelity** | Do logs have needed fields (cmdline, parent)? |
| **Timeliness** | Is data available fast enough to act? |
| **Retention** | Kept long enough for historical hunts? |
| **Consistency** | Normalized and reliably queryable? |

You can only hunt in the data you have; a hunt that finds nothing may mean the threat is absent — or that the telemetry to see it was never captured.

## Detection / value

- **Visibility defines hunt-ability**: ATT&CK data-source mapping shows which techniques you can and cannot hunt.
- **Silent failure risk**: missing or low-fidelity data makes absence of findings meaningless.
- **Retention enables historical hunts**: long dwell times require long retention to catch past activity.
- **Outcome metrics**: detections created and visibility gained measure real program value.

## Practices

- **Map data sources to ATT&CK**; identify and close coverage gaps before hunting.
- **Ensure fidelity**: capture command lines, parent processes, and key fields, not just coarse events.
- **Set adequate retention** for dwell times; verify completeness across all hosts.
- **Measure outcomes**: new detections, visibility gained, gaps closed — not just hunt counts.
- **Invest in data quality** as a force multiplier for every future hunt.

## Explanation

A threat hunter is only as good as the data they can look at — hunting in logs you never collected is like searching a dark room with no flashlight. That's why serious hunting programs obsess over data quality, which is really a set of plain questions. Do we even record the kind of activity we're trying to catch? Are we collecting it from *all* our computers, or just the convenient ones? Does the data include the useful details — like the exact command an attacker typed and which program launched it — or just a vague "something ran"? Do we keep it long enough? This last point matters because attackers often lurk for months, so if you only keep a week of logs, you're blind to how they got in. The sneaky danger is silent failure: you run a hunt, find nothing, and feel reassured — when in fact the threat was there all along, invisible because the right data was never captured. On top of data quality sits the question of whether the whole hunting effort is actually worth it, which is where metrics come in. The wrong metrics count activity ("we ran 50 hunts!"); the right ones count results — how many new automatic detections did those hunts produce, how much visibility did we gain, how much faster can we now spot intruders? And frequently, the highest-value move isn't running another hunt at all, but fixing the data so that every future hunt can actually see.
