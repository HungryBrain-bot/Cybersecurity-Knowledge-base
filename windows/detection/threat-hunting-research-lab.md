---
title: "Threat Hunting Research Lab (Active Directory, ELK, HELK, Sysmon)"
domain: [Windows]
category: [Threat Hunting, Tooling & Scripting, Logging & Monitoring]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
  - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations
  - https://github.com/elastic/beats/tree/main/winlogbeat
  - https://github.com/Cyb3rWard0g/HELK
  - https://github.com/OTRF/Security-Datasets
  - https://github.com/redcanaryco/atomic-red-team
open_issues: []
---

# Threat Hunting Research Lab (Active Directory, ELK, HELK, Sysmon)

## Summary

Hunting skills are built by running attacks you understand and then finding them in the data. A **research lab** is a small, isolated copy of a corporate Windows environment, with a domain controller, a few clients, good endpoint logging and a log platform, where you can execute techniques safely, see exactly which events they produce, and turn that knowledge into hunts and detections. The common open-source recipe is Active Directory plus **Sysmon** on every host, **Winlogbeat** shipping event logs to an **Elastic (ELK)** stack, optionally the **HELK** hunting platform for notebooks and analytics, and attack simulations from **Atomic Red Team** or pre-recorded datasets.

## Lab components

| Layer | Component | Purpose |
|---|---|---|
| **Virtualisation** | A hypervisor (ESXi, Hyper-V, VirtualBox) with isolated networks/VLANs and a firewall | Contain attacks; snapshots to reset quickly |
| **Identity** | Windows Server domain controller with users, groups, OUs and GPOs | Realistic AD for credential and lateral-movement techniques |
| **Endpoints** | Windows clients and a member server joined to the domain | Targets for execution, persistence and movement |
| **Native logging** | Advanced Audit Policy via GPO (process creation, logon, DS changes and others) | Security log coverage |
| **Endpoint telemetry** | **Sysmon** with a tuned configuration | Process creation (1), network connections (3), image loads (7), process access (10), file and registry events (11–13), pipes (17/18), DNS (22) and more |
| **Shipping** | **Winlogbeat** | Runs as a Windows service and ships event logs to Elasticsearch or Logstash |
| **Analytics** | **ELK** (Elasticsearch, Logstash, Kibana) or **HELK** | Search, dashboards; HELK adds Apache Spark, GraphFrames and Jupyter notebooks for data-science style hunting |
| **Attack data** | **Atomic Red Team** tests; **OTRF Security Datasets** | Generate or replay known-technique telemetry |

## Building it in order

1. Create isolated networks; never bridge the lab to production.
2. Build the domain controller, then clients; populate realistic users, groups and a few misconfigurations worth hunting.
3. Push audit policy by GPO and deploy Sysmon with a community or custom configuration.
4. Stand up the log platform, install Winlogbeat on every Windows host and confirm events arrive.
5. Snapshot everything as a clean baseline.
6. Run a technique, hunt for it, write down the query that finds it, then revert.

## Practical notes

- HELK describes itself as alpha-status software; it is for research labs, not production monitoring.
- Pre-recorded datasets let you practise queries without running malware at all.
- Keep a log of each test: technique ID, command, time, host. It turns the lab into a detection test suite.
- Size the log platform generously; Elasticsearch is memory-hungry even for a handful of hosts.

## ATT&CK mapping

None directly. The lab exists to exercise and detect techniques across the whole matrix; see Hunting Quasar RAT and Common Tactics with Atomic Tests.

## Explanation

A hunting lab is a flight simulator. Pilots do not learn to handle an engine fire by waiting for one on a passenger flight; they trigger it in the simulator, watch every gauge, and practise until the response is automatic. The lab lets defenders set off an attack on purpose, watch every log "gauge" react, and rehearse finding it before it happens for real.
