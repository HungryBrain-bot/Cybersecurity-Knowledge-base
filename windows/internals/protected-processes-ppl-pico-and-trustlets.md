---
title: "Protected Processes (PPL), Pico Processes and Trustlets"
domain: [Windows]
category: [Internals, Hardening, Credential Access]
difficulty: Advanced
verification: Needs review
attack_ids: [T1003.001, T1685, T1068]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection
  - https://learn.microsoft.com/en-us/windows/win32/services/protecting-anti-malware-services-
  - https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/vsm
  - https://attack.mitre.org/techniques/T1003/001/
  - https://attack.mitre.org/techniques/T1685/
  - https://attack.mitre.org/techniques/T1068/
open_issues:
  - "The PPL signer hierarchy (WinTcb, Windows, Antimalware, Lsa), the exact access rights withheld, Pico process internals and trustlet policy metadata come from Windows Internals Part 1 and are not documented on Microsoft Learn."
---

# Protected Processes (PPL), Pico Processes and Trustlets

## Summary

Normally an administrator can open any process, read its memory and kill it. Windows has a few special process types that break that rule on purpose. **Protected processes** and **Protected Process Light (PPL)** are processes the kernel shields from other processes, including elevated ones, unless the caller is itself protected at an equal or higher level. Windows uses PPL for anti-malware services and, when **LSA protection** is enabled, for `lsass.exe`, which is exactly what makes credential dumping harder. **Pico processes** are stripped-down processes whose system calls are handled by a provider driver (they were how WSL 1 ran Linux binaries). **Trustlets** are processes that run in the virtualization-based secure world (VTL1), out of reach of the normal kernel entirely.

## Process types

| Type | What is different | Example uses |
|---|---|---|
| **Protected Process (PP)** | Kernel withholds powerful access rights (memory read/write, injection, termination) from unprotected callers | Originally media/DRM; core system processes |
| **Protected Process Light (PPL)** | Same idea with a signer-based hierarchy; a process can only obtain full access to another at the same or lower level | Anti-malware services, `lsass.exe` with LSA protection |
| **Minimal / Pico process** | No normal user-mode Windows environment; a pico provider driver handles its system calls | WSL 1 |
| **Trustlet (secure process)** | Runs in VTL1 under the Secure Kernel; VTL0 kernel cannot read its memory | Credential Guard's isolated LSA |

## How it works

### PPL for anti-malware

Since Windows 8.1, an anti-malware vendor with an **Early Launch Anti-Malware (ELAM)** driver can run its user-mode service as a protected service. The ELAM driver carries a resource section with the vendor's certificate information; at boot Windows extracts it, registers the service, and from then on code integrity allows only correctly signed code to load into the service. Admin processes can no longer inject into it or tamper with it.

### LSA protection (RunAsPPL)

Setting the `RunAsPPL` value under the LSA registry key (or the matching policy) makes `lsass.exe` start as a PPL. Optionally the setting is stored in a UEFI variable ("UEFI lock") so that it cannot be turned off by editing the registry. On new installations of Windows 11 22H2 and later it is applied by default without UEFI lock, and audit mode is on by default. Once LSASS is protected, plug-ins and drivers it loads must meet Microsoft signing requirements; Code Integrity logs failures as events 3033 and 3063 (blocked) and 3065 and 3066 (audit) in **Applications and Services Logs > Microsoft > Windows > CodeIntegrity > Operational**.

### Trustlets

With Virtual Secure Mode, the hypervisor keeps VTL1 memory inaccessible to VTL0, so even a compromised normal kernel or a malicious driver cannot read a trustlet's secrets. Credential Guard uses this to keep derived credentials out of the normal LSASS memory.

## Offence and defence

- **Credential dumping (T1003.001).** Tools that open `lsass.exe` with memory-read access fail against a PPL-protected LSASS. Attackers respond by loading a **vulnerable signed driver** and clearing the protection field in the kernel's process structure, or by disabling the protection through the kernel (T1068 to gain kernel access, then T1685 to switch off the protection on security tools).
- **Attacking security products (T1685).** The same driver-based approach is used to kill or neuter PPL-protected EDR services.
- **Defensive stack:** enable LSA protection with UEFI lock where possible, enable Credential Guard so secrets live in a trustlet, and enable the vulnerable driver blocklist and HVCI so the driver-based bypass is blocked.

## Detection

- CodeIntegrity Operational events 3033/3063/3065/3066 around `lsass.exe`.
- New kernel driver services, especially known-vulnerable signed drivers, immediately before LSASS access or EDR failure.
- Process access to `lsass.exe` from unexpected processes (Sysmon Event ID 10).

## ATT&CK mapping

- **T1003.001 LSASS Memory**
- **T1685 Disable or Modify Tools**
- **T1068 Exploitation for Privilege Escalation** (vulnerable-driver route to kernel)

## Explanation

Most rooms in the building open to the master key the administrator carries. Protected processes are rooms with an extra lock that only opens for keys of equal or higher rank, so the ordinary master key no longer works. Trustlets are rooms in a separate wing whose door is enforced by the building's structure itself; even the head of maintenance cannot get in. Attackers who cannot pick the extra lock try to bribe a contractor with a legitimate badge (a signed but vulnerable driver) to remove it, which is why defenders ban known-bad contractors outright.
