---
title: "Virtual Machine Rootkits (Hypervisor and VM-Based)"
domain: [General]
category: [Defense Evasion]
difficulty: Advanced
verification: Verified
attack_ids: [T1014, T1564.006]
checked_against:
  - https://attack.mitre.org/techniques/T1014/
  - https://attack.mitre.org/techniques/T1564/006/
open_issues: []
---

# Virtual Machine Rootkits (Hypervisor and VM-Based)

## Summary

A **virtual-machine-based rootkit (VMBR)** is a stealth technique in which malware installs a thin hypervisor *beneath* the running operating system, so that the original OS is transparently relocated into a guest VM while the malicious hypervisor runs at a higher privilege level than the OS itself. The appeal to an attacker is profound stealth: code that sits below the OS can intercept and manipulate everything the OS sees, while the OS — and the security tools running inside it — have no straightforward way to observe the layer controlling them (**Rootkit**, T1014). A related but distinct technique is running malicious activity inside an attacker-created **virtual instance** to hide it from host-based tools (**Run Virtual Instance**, T1564.006). VMBRs were demonstrated in research (e.g. the "SubVirt" and "Blue Pill" proofs of concept) as a worst-case stealth scenario. In practice, modern hardware and platform protections make them harder and rarer than the theory suggests: **hardware virtualization features, hypervisor-based security (e.g. virtualization-based security), measured/secure boot, and timing/behavioral discrepancies** all work against a hidden hypervisor, because installing one leaves measurable artifacts (boot-chain changes, timing anomalies, virtualization state) that detection and attestation can catch. For defenders the concept is important as the archetype of "**attack from below**": it illustrates why trust must be anchored in hardware and the boot chain, why in-OS security tools cannot fully vouch for the layer beneath them, and why secure boot and attestation matter.

## Concept and detection

| Aspect | Detail |
|---|---|
| **Mechanism** | Malicious hypervisor relocates the OS into a guest (T1014) |
| **Goal** | Control and observe the OS from a higher privilege level |
| **Variant** | Hide activity in an attacker VM (Run Virtual Instance, T1564.006) |
| **Reality check** | Hard on modern HW; leaves boot/timing/virtualization artifacts |
| **Trust anchor** | Hardware root of trust, secure/measured boot, attestation |

The core lesson: security tools inside an OS cannot fully attest to a layer running beneath the OS, so trust must be rooted in hardware and the verified boot chain.

## Detection / value

- **Boot-chain integrity**: measured/secure boot and attestation detect an inserted hypervisor layer.
- **Timing and behavioral discrepancies**: virtualization introduces measurable overhead and artifacts an OS can probe for.
- **Virtualization state**: unexpected hypervisor presence or CPU virtualization flags can reveal subversion.
- **Defense from below**: hardware-backed security (VBS/hypervisor protections) raises the floor attackers must get beneath.

## Defense / practices

- **Anchor trust in hardware**: enable secure boot, measured boot, and remote attestation; don't rely solely on in-OS tools.
- **Use virtualization-based security** so the legitimate hypervisor protects the OS rather than the reverse.
- **Monitor boot integrity** and firmware/bootloader changes for signs of a planted layer.
- **Keep firmware and platform protections current**; the bar against VMBRs is largely a hardware/firmware story.

## ATT&CK mapping

| ID | Name | Tactic |
|---|---|---|
| T1014 | Rootkit | Stealth |
| T1564.006 | Hide Artifacts: Run Virtual Instance | Stealth |

## Explanation

Normally we picture malware living inside your operating system and security software hunting it there. A virtual-machine rootkit turns that picture upside down by sliding *underneath* the operating system. It installs a tiny control layer — a hypervisor — and quietly demotes your whole OS to being a "guest" running on top of it, without the OS realizing anything changed. From that lower perch, the malware can watch and tamper with everything the OS does, while the OS and its antivirus, now sitting above it, have no clean way to look down and see their new landlord. Researchers built proof-of-concept versions (with memorable names like Blue Pill) to show how devastating this stealth could be. The reassuring part is that it's much harder to pull off in reality than in theory: wedging a secret layer beneath a modern system leaves fingerprints — the boot process changes, timings drift, virtualization leaves telltale signs — and today's computers use hardware-backed secure boot and attestation specifically to notice such tampering. The lasting idea for defenders is simple and important: a guard standing inside a building can't vouch for whoever controls the foundation, so real trust has to be anchored in the hardware and the verified startup chain, not in software asking itself whether it's been fooled.
