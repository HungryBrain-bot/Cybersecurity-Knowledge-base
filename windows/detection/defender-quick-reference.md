---
title: "Defender Quick Reference: Identify, Protect, Detect, Respond, Recover"
domain: [Windows, Linux, Network]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netstat
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/taskkill
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks-query
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wmic
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/auditpol
  - https://raw.githubusercontent.com/nmap/nmap/master/docs/nmap.usage.txt
  - https://raw.githubusercontent.com/the-tcpdump-group/tcpdump/master/tcpdump.1.in
  - https://raw.githubusercontent.com/wireshark/wireshark/master/doc/man_pages/mergecap.adoc
  - https://raw.githubusercontent.com/wireshark/wireshark/master/doc/man_pages/editcap.adoc
open_issues: []
---

# Defender Quick Reference: Identify, Protect, Detect, Respond, Recover

## Summary

Field manuals for blue teams collect the commands a defender needs under pressure and arrange them by the phases of the NIST Cybersecurity Framework: **Identify** what you have, **Protect** it, **Detect** attacks, **Respond** to them and **Recover** afterwards. The value is less in any single command than in having a tested, phase-by-phase checklist ready before an incident. This page summarises the categories such a reference covers and gives a small set of verified, built-in commands for each, so it can serve as a starting point for your own runbook.

## Identify: know the estate

| Goal | Example |
|---|---|
| Find live hosts and services on your network | `nmap -sS -sV -O -oA scan 10.0.0.0/24` (SYN scan, service and OS detection, output in all formats) |
| List listening ports and owning processes (Windows) | `netstat -ano` (`-b` adds the executable, needs elevation) |
| List running processes and their services | `tasklist /svc` |
| List scheduled tasks in detail | `schtasks /query /fo LIST /v` |
| Check effective audit policy | `auditpol /get /category:*` |

Combine with vulnerability scanners (for example Nessus or OpenVAS) and your asset inventory.

## Protect: reduce attack surface

- Disable unneeded services and remove unused software.
- Turn on host firewalls and restrict inbound management protocols to admin networks.
- Use application allow-listing (WDAC or AppLocker on Windows).
- Enforce configuration with Group Policy or your endpoint management tool, and use IPsec or SMB signing where lateral traffic needs protection.

## Detect: see what is happening

| Goal | Example |
|---|---|
| Capture traffic on an interface | `tcpdump -i eth0 -w capture.pcap` |
| Read a capture back | `tcpdump -r capture.pcap` or `tshark -r capture.pcap` |
| Cut a capture to a time window | `editcap -A "<start>" -B "<stop>" in.pcap out.pcap` |
| Merge captures | `mergecap -w merged.pcap a.pcap b.pcap` |
| Query Windows event logs | `wevtutil qe Security /c:20 /rd:true /f:text` |

Add deception (honey accounts, honey files, honey tokens), passive DNS logging and central log collection.

## Respond: contain and investigate

- **Live triage** on Windows: processes (`tasklist /v`), network connections (`netstat -ano`), services, scheduled tasks, autoruns, recent logons from the Security log. On Linux: `ps`, `ss`/`netstat`, `crontab -l`, systemd units, `last`.
- **Stop a malicious process**: `taskkill /pid <PID> /f /t` ends it and its children (Windows); `kill -9 <PID>` on Linux. Capture memory first if you need it as evidence.
- **Contain**: isolate the host through EDR or network controls rather than powering it off.
- Write network signatures (Snort/Suricata) for confirmed indicators, and follow an incident checklist so evidence is preserved.

Note: the `wmic` command line, common in older references, is deprecated from Windows 10 21H1 and Windows Server 21H1; use PowerShell CIM cmdlets instead.

## Recover: restore and learn

- Restore from known-good backups and rebuild rather than clean where persistence is uncertain.
- Reset credentials the attacker could have touched, including service and privileged accounts.
- Record lessons learned and update detections and the runbook itself.

## ATT&CK mapping

None directly; this is a defensive operations reference. The Detect and Respond commands support investigations of many techniques covered elsewhere in this knowledge base.

## Explanation

Think of this as the laminated checklist kept in a fire station. When the alarm rings nobody wants to remember command syntax from scratch, so the card says: first know the building (Identify), keep fire doors shut (Protect), watch the smoke detectors (Detect), put the fire out without destroying the evidence of how it started (Respond), and then reopen the building and fix whatever let it start (Recover).
