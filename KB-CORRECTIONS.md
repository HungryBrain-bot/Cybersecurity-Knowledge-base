# Knowledge Base Corrections

Issues found while verifying content against primary sources, to be fixed in the source Notion knowledge base. The published Markdown already uses the corrected values; this file is the to-do list for the Notion side.

## MITRE ATT&CK IDs that have been revoked or renumbered

Checked against the current MITRE ATT&CK Enterprise dataset (spec 2.0, matrix version 2.0, modified 2026-07-31). Several IDs in the Notion `MITRE ATT&CK ID` column point to techniques that ATT&CK has since revoked and replaced. The Notion rows should be updated to the new IDs.

| Notion ID (old) | Status | Correct current ID | Current name | Affected KB concept(s) |
|---|---|---|---|---|
| T1562 | Revoked | T1685 | Disable or Modify Tools | (row using bare T1562) |
| T1562.001 | Revoked | T1685 | Disable or Modify Tools | rows tagged T1562.001 |
| T1562.006 | Revoked | T1685 | Disable or Modify Tools | rows tagged T1562.006 |
| T1562.008 | Revoked | T1685.002 | Disable or Modify Cloud Log | Google Workspace Takeover and CloudTrail Evasion |
| T1070.001 | Revoked | T1685.005 | Clear Windows Event Logs | row tagged T1070.001 |

Note: the ATT&CK "Impair Defenses" (T1562) sub-techniques were restructured under a new technique, T1685, in this dataset version. Any KB concept about disabling or tampering with security tools, logging or cloud logs should move to the matching T1685 ID.

## Notes

- All other ATT&CK IDs present in the Cloud batch were confirmed current.
- Terminology refreshed in the published files where vendors renamed products (for example Azure AD → Microsoft Entra ID, 2023; Azure Security Center → Microsoft Defender for Cloud). These are wording updates, not KB errors, and are listed here only so the Notion source can be refreshed if desired.
- OWASP Top 10 references updated to the 2025 edition where the KB cited an older list.

## Network batch (terminology current as of ATT&CK Enterprise v2)

- **T1557.001** is now named **"Name Resolution Poisoning and SMB Relay"** (previously "LLMNR/NBT-NS Poisoning and SMB Relay"). Files use the current name. Affected KB concepts: "Network Compromise: Responder, CrackMapExec and Credential Capture", "NTLM Relay Attack".
- **T1599 (Network Boundary Bridging)** sits under the **Defense Impairment** tactic (TA0112) in this dataset version, the same restructure that produced T1685. Noted in "Bypassing Network Access Control".
- All other ATT&CK IDs present in the Network batch were confirmed current against the Enterprise dataset.

## Windows gap-fill batch (PRs #36 to #44)

Found while writing the 58 concepts that were in Notion but missing from the repo. ATT&CK IDs checked against the same Enterprise dataset as above.

- Image loader: Notion lists KnownDLLs before API sets/SxS/.local; Microsoft Learn order is DLL redirection, API sets, SxS, loaded-module list, KnownDLLs. File uses the Microsoft order.
- Protected Processes (PPL), Pico Processes and Trustlets: Notion MITRE ID T1562 is revoked; file uses T1685 (Disable or Modify Tools) plus T1003.001 and T1068.
- Monitoring Operating System Events: Notion MITRE ID T1070.001 is revoked; file uses T1685.005 (Clear Windows Event Logs). Notion also lists System event 104 (log cleared); not confirmed in the Microsoft auditing docs used, so it was omitted.
- Command History Recovery: Notion says cmd.exe history lived in csrss.exe before Windows 7; not confirmed from a primary source, so the file names only conhost.exe (Volatility 3 cmdscan/consoles). Notion's "ConsoleHost_history.txt" is stated in Microsoft docs only as the pattern $($Host.Name)_history.txt.
- Encryption Key Recovery from Memory: Notion implies BitLocker FVEK plug-ins in Volatility; none exist in Volatility core (2 or 3). VeraCrypt support was not confirmed and is omitted.
- Network Artifacts in Memory: Volatility 2 connections/sockets are XP/2003 only; current names are Volatility 3 windows.netscan / windows.netstat / linux.sockstat / linux.ip.
- Kernel Exploitation: no Microsoft primary source found for the "Windows blocks NULL-page mapping" mitigation; omitted.
