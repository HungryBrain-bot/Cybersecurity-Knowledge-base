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
