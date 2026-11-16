---
title: "PowerShell for Windows Security Research (NtObjectManager)"
domain: [Windows]
category: [Reverse Engineering, Internals]
difficulty: Intermediate
verification: Verified
attack_ids: [T1106, T1059.001]
checked_against:
  - https://github.com/googleprojectzero/sandbox-attacksurface-analysis-tools
  - https://github.com/googleprojectzero/sandbox-attacksurface-analysis-tools/blob/main/NtObjectManager/NtObjectManager.psd1
  - https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging_windows
  - https://attack.mitre.org/techniques/T1106/
  - https://attack.mitre.org/techniques/T1059/001/
open_issues: []
---

# PowerShell for Windows Security Research (NtObjectManager)

## Summary

**NtObjectManager** is a PowerShell module by James Forshaw, published in Google Project Zero's *sandbox-attacksurface-analysis-tools* repository. It wraps a managed library (NtCoreLib) that calls NT system calls directly, so a researcher can inspect kernel objects, tokens, security descriptors and access checks interactively instead of writing and compiling C for every question. The repository describes the toolset as a way to test the security properties of sandboxes on Windows, and recommends running it as administrator or SYSTEM so it can enumerate the whole system.

## Getting started

Per the repository README, you copy the module into your PowerShell modules folder, then:

```powershell
Import-Module NtObjectManager
Get-ChildItem NtObject:\     # browse the NT object manager namespace
```

The README notes that you may need to relax PowerShell script-signing requirements to load it. Use it on a lab or research machine.

## What it is good for

| Question | Example cmdlets exported by the module |
|---|---|
| What objects exist in the object namespace? | `Get-ChildItem NtObject:\`, `Get-NtDirectoryEntry`, `Get-NtSymbolicLinkTarget` |
| Who has a handle open to what? | `Get-NtHandle`, `Get-NtObjectFromHandle` |
| What is in a process or thread token? | `Get-NtToken`, `Test-NtTokenPrivilege` |
| What does an object's security descriptor say? | `Get-NtSecurityDescriptor` |
| What access would a given caller actually get? | `Get-NtGrantedAccess`, `Get-AccessibleObject`, `Get-AccessibleDevice`, `Get-AccessibleNamedPipe` |
| What memory regions does a process have? | `Get-NtVirtualMemory` |
| Which modules import or export a given function? | `Get-Win32ModuleImport`, `Get-Win32ModuleExport` |
| What RPC servers listen on ALPC? | `Get-RpcAlpcServer` |

Many of the tools accept a process ID: they impersonate that process's token and report what it could open. That is exactly the question you ask when looking for a sandbox escape or a privilege-escalation path through a weakly secured object.

```mermaid
flowchart LR
    A[Pick a low-privilege process]:::detect --> B[Use its token]:::detect
    B --> C[Enumerate objects, devices, pipes]:::detect
    C --> D[Compute granted access]:::detect
    D --> E[Unexpected write access = finding]:::defend
    classDef detect fill:#1f6feb,color:#fff
    classDef defend fill:#2da44e,color:#fff
```

## Security relevance

- **Attack-surface review**: list every object a sandboxed or low-integrity process can write to.
- **Access-control debugging**: see the exact security descriptor and the access the kernel grants, instead of guessing.
- **Learning internals**: it is the companion tooling for *Windows Security Internals* (Forshaw, 2024), which uses it throughout.

Attackers can use the same native-API access (T1106) from PowerShell (T1059.001), so defenders should treat an unexpected import of this module on a production endpoint as worth a look.

## Detection notes

- When Script Block Logging is enabled, PowerShell records the content of the script blocks it processes (event ID 4104), which includes `Import-Module NtObjectManager` and the cmdlets that follow.
- The module is a research tool, not malware; context (who, where, why) decides whether its use is expected.

## ATT&CK mapping

- **T1106 Native API**: the module calls NT system calls rather than documented Win32 wrappers.
- **T1059.001 PowerShell**: it is driven from PowerShell.

## Explanation

The Windows object manager is a huge building with thousands of locked doors. Normally you only find out whether a door is open by walking up and trying it in C code. NtObjectManager hands you the building plan and a copy of any tenant's key ring, and tells you which doors that key ring opens.
