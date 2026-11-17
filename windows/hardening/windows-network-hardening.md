---
title: "Windows Network Hardening (Firewall, Exploit Guard, Name Resolution Poisoning)"
domain: [Windows, Network]
category: [Network Security]
difficulty: Intermediate
verification: Needs review
attack_ids: [T1557.001, T1021.002, T1071]
checked_against:
  - https://attack.mitre.org/techniques/T1557/001/
  - https://learn.microsoft.com/en-us/defender-endpoint/network-protection
  - https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netsh-advfirewall
  - https://learn.microsoft.com/en-us/windows-server/storage/file-server/smb-signing-overview
  - https://learn.microsoft.com/en-us/windows-server/storage/file-server/smb-security-hardening
  - https://attack.mitre.org/techniques/T1021/002/
  - https://attack.mitre.org/techniques/T1071/
open_issues:
  - "WPAD hardening guidance (disabling automatic proxy discovery) could not be confirmed against a current Microsoft page; kept as general advice."
---

# Windows Network Hardening (Firewall, Exploit Guard, Name Resolution Poisoning)

## Summary

Most Windows intrusions move across the network: an attacker on one workstation tries to reach others over SMB, RDP or WinRM, listens for broadcast name lookups to steal credentials, and calls out to command-and-control servers. Host-level network controls cut each of those paths. The main ones on Windows are the **host firewall** managed centrally, **Microsoft Defender network protection** (originally part of Windows Defender Exploit Guard) for outbound connections to malicious destinations, removing **legacy name resolution** that enables poisoning, and **SMB signing** to stop relay. In Azure, network security groups (NSGs) play the equivalent role around virtual machines.

## Controls

| Control | Stops | Notes |
|---|---|---|
| **Windows Firewall with Advanced Security**, set by Group Policy | Inbound lateral movement to workstations (SMB, RDP, WinRM, RPC) | Default-deny inbound on clients; allow management only from admin subnets. `netsh advfirewall` and PowerShell manage rules locally |
| **Network protection** (Defender for Endpoint) | Outbound connections to malicious or suspicious sites and IPs | Needs Defender Antivirus real-time protection; run in audit mode first, then block |
| **Disable LLMNR, NBT-NS and mDNS** where not needed | Name resolution poisoning and credential capture (T1557.001) | Recommended by ATT&CK's mitigation for T1557.001; via Group Policy or local settings |
| **SMB signing required** | NTLM relay over SMB | Domain controllers require signing by default; extend the requirement to servers and clients |
| **Limit outbound protocols** | C2 over unusual ports (T1071) | Egress filtering at the host and perimeter |
| **NSGs in Azure** | Exposure of VM management ports | Pair with just-in-time access for RDP/SSH |

## Name resolution poisoning in one paragraph

When DNS cannot resolve a name, Windows falls back to broadcast or multicast protocols: LLMNR (UDP 5355), NetBIOS Name Service (UDP 137) and mDNS (UDP 5353). Any host on the segment can answer. An attacker running a poisoning tool replies "that's me", the victim connects and offers NTLM authentication, and the attacker captures the hash or relays it. Turning these fallbacks off, requiring SMB signing, and blocking the traffic on host firewalls removes the attack. Web Proxy Auto-Discovery (WPAD) lookups can be abused the same way, so disable automatic proxy discovery where it is not used.

## Rolling it out

1. Inventory which inbound services each class of machine genuinely needs.
2. Build firewall GPOs per tier (workstations, servers, domain controllers) and deploy in logging mode first.
3. Enable network protection in audit mode, review blocks, then switch to block.
4. Disable LLMNR and NetBIOS over TCP/IP and confirm nothing legitimate depended on them.
5. Require SMB signing, then verify with packet captures or SMB client/server configuration queries.

## Detection

- Firewall logs and Windows Filtering Platform events for blocked inbound attempts between workstations.
- LLMNR/NBT-NS responses from hosts that are not name servers.
- Network protection block events from Defender.

## ATT&CK mapping

- **T1557.001 Name Resolution Poisoning and SMB Relay**
- **T1021.002 SMB/Windows Admin Shares**
- **T1071 Application Layer Protocol**

## Explanation

Hardening the network on Windows is like securing an office floor. Interior doors between desks stay locked unless IT needs them (the firewall); the post room refuses parcels addressed to known scam companies (network protection); staff stop shouting "does anyone know where Finance is?" across the floor, because anyone can shout back (LLMNR/NetBIOS); and every internal memo is sealed so a forger cannot pass one on with changes (SMB signing).
