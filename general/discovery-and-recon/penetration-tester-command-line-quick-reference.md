---
title: "Penetration Tester Command-Line Quick Reference"
domain: [General]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://www.kali.org/tools/
  - https://csrc.nist.gov/pubs/sp/800/115/final
open_issues: []
---

# Penetration Tester Command-Line Quick Reference

## Summary

A penetration tester's workflow is largely **command-line driven**, and a mental "quick reference" of the standard tools, organized by phase, reflects how authorized testing actually proceeds. The categories map to the engagement flow. **Reconnaissance/enumeration**: `nmap` (port and service scanning, the workhorse), `dig`/`host`/`whois` (DNS and registration), `gobuster`/`ffuf` (web directory and subdomain brute-forcing), `nikto`/`whatweb` (web server fingerprinting), `enum4linux`/`smbclient` (SMB enumeration), `snmpwalk`. **Vulnerability discovery**: `nmap` scripts (NSE), `searchsploit` (offline exploit database lookup), scanners. **Exploitation**: `metasploit` (exploitation framework), manual exploit scripts, `hydra` (credential brute-forcing where in scope). **Post-exploitation/privilege escalation**: enumeration scripts (`linpeas`/`winpeas`), `sudo -l`, credential tooling. **Pivoting/transfer**: `ssh` tunnels, `chisel`/`proxychains` (proxying through a foothold), and file-transfer one-liners (`python -m http.server`, `scp`, `certutil`). The value of a quick reference is **efficiency and completeness** — knowing the right tool for each task so enumeration is thorough and nothing is missed, and remembering syntax under time pressure. For defenders, this reference is effectively a **detection catalog**: these are the exact tools and commands attackers run, so monitoring for their signatures (aggressive `nmap` scans, directory brute-forcing patterns, `linpeas` execution, `proxychains`/`chisel` traffic, suspicious file-transfer one-liners) provides concrete detection opportunities. The underlying point is that offensive work is methodical and tool-driven, and the same tools that make testers efficient are observable artifacts defenders can learn to recognize.

## Tooling by phase

| Phase | Representative tools |
|---|---|
| **Recon/enumeration** | `nmap`, `dig`/`whois`, `gobuster`/`ffuf`, `enum4linux`, `smbclient` |
| **Vuln discovery** | `nmap` NSE, `searchsploit`, scanners |
| **Exploitation** | `metasploit`, manual scripts, `hydra` |
| **Priv-esc/post-ex** | `linpeas`/`winpeas`, `sudo -l` |
| **Pivot/transfer** | `ssh`, `chisel`, `proxychains`, `python -m http.server` |

The tester's efficiency tools are, from the defender's side, a catalog of observable artifacts to detect.

## Detection / value

- **Enumeration signatures**: aggressive port scans and web directory brute-forcing have recognizable patterns.
- **Priv-esc tooling**: `linpeas`/`winpeas` execution is a strong post-exploitation indicator.
- **Pivoting traffic**: `proxychains`/`chisel` and unexpected tunnels reveal lateral movement.
- **Transfer one-liners**: ad hoc `http.server`, `certutil` downloads flag staging/exfiltration.

## Defense / practices

- **Detect scanning/brute-forcing**: alert on `nmap`-style scans and directory enumeration volume.
- **Monitor for offensive tooling** execution and known artifacts on endpoints.
- **Watch pivoting/tunneling**: unexpected proxies, SSH tunnels, and outbound connections.
- **Flag ad hoc file transfers** and living-off-the-land download commands.

## Explanation

If you watched a penetration tester work, you'd mostly see them typing commands in a terminal, moving through a predictable sequence of tools — and that sequence is worth knowing, from both sides of the fence. They start by mapping the target: `nmap` to find open ports and running services is the universal first move, followed by tools that brute-force hidden web pages, enumerate file shares, and query DNS. Once they know what's there, they look for weaknesses — checking an offline database of known exploits, running scanners — and then exploit them, often with a framework like Metasploit or by hand. After getting in, they run "privilege escalation" helper scripts (with blunt names like linpeas) that automatically scan a compromised machine for ways to become administrator. And to reach networks hidden behind their foothold, they set up tunnels and proxies, plus quick one-liners to shuttle files around. For the tester, memorizing this toolbox is about speed and thoroughness — using the right tool for each step so nothing gets missed. But flip it around and this same list becomes a defender's cheat sheet, because these are the exact commands attackers run too. A sudden burst of port-scanning, a flood of guessed web addresses, one of those privilege-escalation scripts firing on a server, or traffic from a tunneling tool — each is a fingerprint you can learn to spot. The tools that make attackers efficient also make them visible, if you know what to watch for.
