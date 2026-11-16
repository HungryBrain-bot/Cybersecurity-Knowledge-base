---
title: "Network Artifacts in Memory"
domain: [Windows, Linux, Network]
category: [Forensics, Detection]
difficulty: Intermediate
verification: Verified
attack_ids: [T1071, T1040, T1014]
checked_against:
  - https://github.com/volatilityfoundation/volatility3/blob/develop/volatility3/framework/plugins/windows/netscan.py
  - https://github.com/volatilityfoundation/volatility3/blob/develop/volatility3/framework/plugins/windows/netstat.py
  - https://github.com/volatilityfoundation/volatility3/blob/develop/volatility3/framework/plugins/linux/sockstat.py
  - https://github.com/volatilityfoundation/volatility3/blob/develop/volatility3/framework/plugins/linux/ip.py
  - https://github.com/volatilityfoundation/volatility/blob/master/volatility/plugins/connections.py
  - https://attack.mitre.org/techniques/T1071/
  - https://attack.mitre.org/techniques/T1040/
  - https://attack.mitre.org/techniques/T1014/
open_issues: []
---

# Network Artifacts in Memory

## Summary

When a host is compromised, `netstat` and similar tools on that host may be lying: a rootkit can filter their output (T1014). A memory image does not depend on the host's tools. The kernel's networking structures (sockets, TCP and UDP endpoints, listening ports, interfaces) are all in RAM, and each one can be tied back to the **process that owns it**. That turns "something on this network talks to a bad IP" into "this process, started at this time, from this path, talks to that IP", which is what an investigation needs.

## Windows

| Volatility 3 plugin | What it does | Notes |
|---|---|---|
| `windows.netscan` | **Scans** memory for network objects (TCP endpoints, TCP listeners, UDP endpoints) | Pool scanning can find objects that are closed or unlinked, so it may show recently closed connections and ones hidden from list-walking tools |
| `windows.netstat` | **Walks** the kernel's network tracking structures | Shows what the OS currently considers live |

Comparing the two is useful: an endpoint found by scanning but not by walking deserves a closer look. The older Volatility 2 `connections` and `sockets` plugins only work on Windows XP and Server 2003.

## Linux

| Volatility 3 plugin | What it does |
|---|---|
| `linux.sockstat` | Lists network connections for all processes |
| `linux.ip.Addr` / `linux.ip.Link` | Lists network interfaces and their addresses; the link view reports whether an interface is in **promiscuous mode** |

Volatility 2 offered equivalents such as `linux_netstat`, `linux_ifconfig` and `linux_arp`.

## What to look for

- **C2 connections**: processes with no business talking to the internet (e.g. `notepad.exe`, `lsass.exe`) holding external connections (T1071 Application Layer Protocol).
- **Unexpected listeners**: backdoor ports bound by unusual processes.
- **Sniffers**: an interface in promiscuous mode, or raw sockets, suggests packet capture for credential theft (T1040 Network Sniffing).
- **Hidden activity**: endpoints visible in memory but absent from the live host's `netstat` output collected during triage.

```mermaid
flowchart LR
    A[Memory image]:::detect --> B[netscan and netstat or linux.sockstat]:::detect
    B --> C[Map endpoints to owning process]:::detect
    C --> D[Compare with live netstat and firewall logs]:::detect
    D --> E[Suspicious process plus remote IP]:::defend
    classDef detect fill:#1f6feb,color:#fff
    classDef defend fill:#2da44e,color:#fff
```

## Practical rules

- Capture memory **before** disconnecting the host from the network if you can do so safely; some connection state disappears quickly.
- Join memory findings with firewall and proxy logs by IP and time.
- Remember that scanned results can include stale objects; confirm with other evidence before concluding a connection was active.

## ATT&CK mapping

- **T1071 Application Layer Protocol**: C2 channels attributed to processes.
- **T1040 Network Sniffing**: promiscuous interfaces and raw sockets.
- **T1014 Rootkit**: hidden connections that host tools don't show.

See also [Memory Forensics and Volatility](memory-forensics-and-volatility.md).

## Explanation

Asking a compromised host for its connections is like asking a suspect for their phone records. Reading memory is getting the records from the phone company: the same calls, but from a source the suspect can't edit, and with the name of who made each call.
