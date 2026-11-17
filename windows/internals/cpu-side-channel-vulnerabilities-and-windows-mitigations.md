---
title: "CPU Side-Channel Vulnerabilities and Windows Mitigations (KVA Shadow, Retpoline)"
domain: [Windows]
category: [Internals, Exploitation]
difficulty: Advanced
verification: Verified
attack_ids: []
checked_against:
  - https://github.com/microsoft/SpeculationControl
  - https://techcommunity.microsoft.com/blog/windowsosplatform/mitigating-spectre-variant-2-with-retpoline-on-windows/295618
  - https://www.cve.org/CVERecord?id=CVE-2017-5753
  - https://www.cve.org/CVERecord?id=CVE-2017-5715
  - https://www.cve.org/CVERecord?id=CVE-2017-5754
  - https://www.cve.org/CVERecord?id=CVE-2018-3639
  - https://www.cve.org/CVERecord?id=CVE-2018-3620
open_issues: []
---

# CPU Side-Channel Vulnerabilities and Windows Mitigations (KVA Shadow, Retpoline)

## Summary

Modern processors guess ahead. They predict branches, execute instructions **speculatively** before they know the instructions should run, and keep recently used data in caches. When a guess is wrong the architectural results are thrown away, but traces remain in the cache, and an attacker who times memory accesses carefully can read them back. The 2018 **Spectre** and **Meltdown** disclosures showed that this lets unprivileged code infer kernel memory or another process's secrets. Windows answered with operating-system mitigations such as **KVA Shadow** (separate kernel address space for user mode) and **retpoline** (a compiler technique against branch target injection), combined with microcode-provided controls. These mitigations cost performance and depend on the exact CPU, so security engineers need to know how to check which are active.

## The main vulnerability classes

| CVE | Common name | What leaks | Windows mitigation |
|---|---|---|---|
| CVE-2017-5753 | Spectre variant 1, bounds check bypass | Data past a bounds check in the same address space | Code-level fixes (speculation barriers) in affected code |
| CVE-2017-5715 | Spectre variant 2, branch target injection | Data via a poisoned indirect-branch predictor | Retpoline, plus hardware controls (IBRS, STIBP) |
| CVE-2017-5754 | Meltdown, rogue data cache load | Kernel memory from user mode | **KVA Shadow** |
| CVE-2018-3639 | Speculative store bypass | Stale data from before a store | SSBD (speculative store bypass disable) |
| CVE-2018-3620 | L1 terminal fault | Data in the L1 cache, including across VMs | L1TF mitigation (kernel and hypervisor) |
| — | Microarchitectural data sampling (MDS) | Data in CPU internal buffers | Buffer clearing on transitions |

## How the main mitigations work

- **KVA Shadow.** Meltdown works because the kernel's memory is mapped into every process's address space and protected only by a permission bit that speculative execution ignored on affected CPUs. KVA Shadow keeps a second, mostly empty set of page tables for user mode, so kernel memory is simply not mapped while user code runs. The cost is a page-table switch on every kernel entry and exit; processors with PCID/INVPCID support make that cheaper, and Microsoft's checker reports whether PCID optimisation is enabled.
- **Retpoline.** Spectre variant 2 abuses the indirect-branch predictor. A retpoline replaces an indirect jump or call with a construct that sends speculation into a harmless loop, so the predictor cannot be steered. Microsoft adapted retpoline for the Windows kernel, and its checker reports retpoline and the related **import optimisation** feature as separate settings.
- **Hardware controls.** Updated microcode exposes controls such as **IBRS** (restrict indirect-branch speculation), **STIBP** (stop sibling hyperthreads sharing predictions) and **SSBD**. Windows uses them where the CPU supports them and where software techniques are not enough.

## Checking a system

Microsoft publishes the **SpeculationControl** PowerShell module. Its `Get-SpeculationControlSettings` cmdlet reports, per CVE, whether the hardware is vulnerable, whether Windows support is present, and whether it is enabled (for example "kernel VA shadow is enabled", "retpoline enabled", "import optimisation enabled", "speculative store bypass disable is enabled system-wide").

```powershell
Install-Module SpeculationControl
Get-SpeculationControlSettings
```

## Security relevance

- **Information disclosure, not code execution.** These bugs leak data (kernel pointers, keys, other tenants' memory). They are most serious on shared hosts: browsers running untrusted JavaScript, multi-tenant virtualisation and cloud.
- **KASLR defeat.** Even partial kernel memory disclosure helps a local exploit find kernel addresses.
- **Mitigation drift.** Mitigations can be disabled by policy for performance; a host that reports a mitigation as "present but not enabled" is still exposed.
- **Hypervisor boundary.** L1TF and MDS can cross VM boundaries, so hypervisor hosts need both OS and hypervisor mitigations.

## Defence

- Apply both Windows updates and firmware/microcode updates; neither alone is complete.
- Verify with `Get-SpeculationControlSettings` after patching, especially on hypervisor hosts and servers running untrusted code.
- Treat performance-driven mitigation opt-outs as risk acceptances and record them.

## ATT&CK mapping

No ATT&CK technique covers hardware side channels directly; they are an exploitation enabler for information disclosure and privilege escalation.

## Explanation

Imagine a librarian who, to save time, starts fetching the book they think you will ask for before you finish your sentence. If you were not allowed that book, they put it back, but the empty gap on the shelf and the warm cover remain. By watching how quickly the librarian returns with different books, a clever patron works out what was fetched. KVA Shadow moves the restricted books to a locked room the librarian cannot reach while serving the public; retpoline stops the librarian from being tricked into guessing a particular shelf.
