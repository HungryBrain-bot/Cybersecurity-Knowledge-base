---
title: "Command and Control Frameworks (Merlin, Koadic, SILENTTRINITY)"
domain: [General]
category: [Command and Control, Tooling & Scripting]
difficulty: Advanced
verification: Verified
attack_ids: [T1071, T1105]
checked_against:
  - https://attack.mitre.org/techniques/T1071/
  - https://attack.mitre.org/tactics/TA0011/
open_issues: []
---

# Command and Control Frameworks (Merlin, Koadic, SILENTTRINITY)

## Summary

**Command-and-Control (C2) frameworks** are the platforms that manage compromised hosts during an operation — the "operator console" of an intrusion — providing implants/agents, a communication channel, and post-exploitation tooling. Commercial and open-source examples (Cobalt Strike, and open-source frameworks like **Merlin**, **Koadic**, **SILENTTRINITY**, Mythic, Sliver, Covenant) vary in design but share core functions: deploying an **agent/implant** on the victim, establishing a **C2 channel** over common protocols (HTTP/HTTPS, DNS — **Application Layer Protocol**, T1071) to blend with legitimate traffic, delivering commands and additional tooling (**Ingress Tool Transfer**, T1105), and supporting post-exploitation (credential access, lateral movement, persistence). They differ in implant language and techniques: **Merlin** uses HTTP/2 for C2; **Koadic** (a "COM Command & Control") leverages Windows Script Host (JScript/VBScript) and living-off-the-land binaries; **SILENTTRINITY** uses .NET/IronPython for flexible, in-memory, hard-to-signature payloads. The trend is toward **in-memory, scriptable, hard-to-detect agents** that abuse legitimate interpreters and protocols. For defenders, C2 frameworks are both a threat and a study aid: understanding how they communicate and behave drives detection. Detection focuses on **network C2 patterns** (beaconing, suspicious HTTP/DNS, TLS/JA3 fingerprints), **host behaviors** (abuse of script hosts, in-memory execution, suspicious parent-child processes), and known framework signatures/defaults. The core insight is that every intrusion needs a way to control its implants, and C2 frameworks provide it — so detecting the characteristic communication and host behavior of these frameworks is one of the highest-value detection opportunities, even as attackers continually evolve toward stealthier, protocol-blending agents.

## Framework functions and examples

| Aspect | Detail |
|---|---|
| **Agent/implant** | Code on the victim awaiting commands |
| **C2 channel (T1071)** | HTTP/HTTPS/DNS to blend with normal traffic |
| **Tooling delivery (T1105)** | Push commands and additional modules |
| **Merlin** | HTTP/2-based C2 |
| **Koadic** | Windows Script Host / LOLBins |
| **SILENTTRINITY** | .NET/IronPython, in-memory, hard to signature |

Every intrusion needs to control its implants; detecting the characteristic C2 traffic and host behavior of these frameworks is a top detection opportunity.

## Detection / value

- **C2 beaconing**: regular check-ins and anomalous HTTP/DNS reveal framework traffic (T1071).
- **Script-host abuse**: Koadic-style use of JScript/VBScript and LOLBins is detectable on the host.
- **In-memory execution**: SILENTTRINITY-style .NET/in-memory payloads evade file-based AV but leave behavioral traces.
- **Defaults and signatures**: known framework artifacts and profiles aid detection.

## Defense / practices

- **Detect C2 network patterns**: beaconing regularity, rare domains, TLS/JA3 fingerprints, DNS anomalies.
- **Monitor host behaviors**: script-host execution, in-memory .NET, suspicious process lineage.
- **Baseline and alert** on anomalous outbound traffic and interpreter abuse.
- **Use threat intel** on framework signatures and known infrastructure for hunting and blocking.

## ATT&CK mapping

| ID | Name | Tactic |
|---|---|---|
| T1071 | Application Layer Protocol | Command and Control |
| T1105 | Ingress Tool Transfer | Command and Control |

## Explanation

Once attackers have planted their malware on your machines, they need a way to actually command those machines — give them orders, pull back stolen data, push new tools. That's the job of a command-and-control framework, which is essentially the attacker's remote-control console for the whole operation. There's a whole ecosystem of them, from the notorious commercial Cobalt Strike to open-source projects with names like Merlin, Koadic, and SILENTTRINITY. They all do the same core job — plant a little agent on your computer, open a secret communication channel back to the attacker, and let the operator issue commands — but they differ in clever ways. Merlin talks over a modern web protocol; Koadic abuses Windows' own built-in scripting so it barely looks like a program at all; SILENTTRINITY runs its payloads entirely in memory, leaving almost nothing on disk for antivirus to catch. The overall trend is toward agents that are invisible to file scanners, that hijack the computer's legitimate tools, and that disguise their chatter as ordinary web or DNS traffic. The good news for defenders is that control has to flow *somehow*, and that flow is detectable. These implants tend to "phone home" at regular, machine-like intervals, produce odd network signatures, and — on the host — do suspicious things like launching scripting engines or running code purely in memory. So even as attackers keep building stealthier consoles, watching for the characteristic heartbeat of command-and-control traffic and the odd behaviors these frameworks cause remains one of the most reliable ways to catch an intruder in the act.
