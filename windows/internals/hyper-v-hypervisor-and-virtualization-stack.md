---
title: "Windows Hypervisor and Virtualization Stack (Hyper-V)"
domain: [Windows]
category: [Internals]
difficulty: Advanced
verification: Verified
attack_ids: [T1611, T1564.006]
checked_against:
  - https://learn.microsoft.com/en-us/windows-server/administration/performance-tuning/role/hyper-v-server/architecture
  - https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/tlfs
  - https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/hypercall-interface
  - https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/vsm
  - https://attack.mitre.org/techniques/T1611/
  - https://attack.mitre.org/techniques/T1564/006/
open_issues: []
---

# Windows Hypervisor and Virtualization Stack (Hyper-V)

## Summary

Hyper-V is a **type 1 hypervisor**: a thin layer that runs directly on the hardware, below every operating system on the machine. It virtualises processors and memory and divides the machine into **partitions**. The **root partition** runs the Windows you log on to, owns the physical devices and hosts the **virtualization stack** that creates and manages **child partitions** (virtual machines). The same hypervisor is what makes **virtualization-based security** possible: it splits a single Windows installation into Virtual Trust Levels so that a Secure Kernel and isolated processes such as `LsaIso` stay protected even from a compromised normal kernel.

## Architecture

| Component | Where | Role |
|---|---|---|
| **Hypervisor** | Below all partitions | Virtualises CPU and memory, enforces isolation, handles hypercalls and intercepts |
| **Root partition** | Host Windows | Owns physical I/O devices; runs the virtualization stack and management APIs |
| **Child partitions** | Guest VMs | Run guest operating systems with virtual processors and memory |
| **Virtualization stack** | Root partition | VM memory manager, management APIs, emulated devices (e.g. IDE, PS/2) and synthetic devices |
| **VSP / VSC** | Root / child | Virtualization service providers in the root serve virtualization service clients in the guest |
| **VMBus** | Between partitions | Channel-based bus using shared memory for fast synthetic device I/O |
| **Hypercalls** | Guest to hypervisor | Documented calling interface (Hypervisor Top-Level Functional Specification) |
| **Virtual Trust Levels (VTLs)** | Within a partition | VTL0 normal world; VTL1 secure world used by VBS |

## How it works

1. At boot the hypervisor loads first and starts Windows inside the root partition.
2. Management tools ask the virtualization stack in the root to create a child partition, allocate memory and virtual processors, and attach devices.
3. Guests with Hyper-V-aware drivers talk to synthetic devices through VSCs over VMBus; guests without them use slower emulated devices.
4. Privileged operations in a guest (certain instructions, memory access to protected regions) trap to the hypervisor as **intercepts**; enlightened guests call the hypervisor directly with hypercalls.
5. With **Virtual Secure Mode**, the hypervisor enforces memory protections between VTLs in the same partition, so VTL0 code, including the normal kernel, cannot read or modify VTL1 memory.

## Security relevance

- **Isolation boundary.** The hypervisor is the boundary between tenants and between a guest and its host. Escapes are among the most valuable bugs; the VMBus channels and emulated devices in the root partition are the main attack surface.
- **Foundation of VBS.** Credential Guard, HVCI and Secure Kernel trustlets depend on the hypervisor; if it is not running, those protections are not either (see Virtualization-Based Security).
- **Hidden VMs (T1564.006).** Attackers have run their own lightweight VMs on compromised hosts to hide tooling from host-based security products.
- **Escape to host (T1611).** Breaking out of a VM or container isolation into the host.

## Detection and defence

- Confirm the hypervisor and VBS are actually running (`msinfo32` shows Virtualization-based security status) rather than assuming it from policy.
- Restrict who can create VMs; unexpected Hyper-V role installation or new VMs on workstations is suspicious.
- Keep hosts patched: Hyper-V escape and VMBus bugs are fixed through regular Windows updates.
- Prefer synthetic (VMBus) devices over legacy emulated ones in guests to reduce emulator attack surface where practical.

## ATT&CK mapping

- **T1611 Escape to Host**
- **T1564.006 Run Virtual Instance**

## Explanation

Think of the hypervisor as the building's structure itself: walls, floors and locked doors that every tenant relies on. The root partition is the building manager's office, which holds the keys to the utilities and runs the services tenants use; the child partitions are rented flats. Tenants request services through a pneumatic tube system (VMBus) rather than walking into the manager's office. Virtualization-based security uses the same walls to build a vault inside one flat that even that flat's own owner cannot open.
