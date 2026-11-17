---
title: "Windows Job Objects and Containers (Server Silos)"
domain: [Windows]
category: [Internals, Hardening]
difficulty: Advanced
verification: Needs review
attack_ids: [T1611]
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/procthread/job-objects
  - https://learn.microsoft.com/en-us/windows/win32/procthread/nested-jobs
  - https://learn.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/hyperv-container
  - https://learn.microsoft.com/en-us/virtualization/windowscontainers/about/containers-vs-vm
  - https://attack.mitre.org/techniques/T1611/
open_issues:
  - "The term 'server silo' and its kernel implementation (a job object extended with its own namespace) come from Windows Internals Part 1; Microsoft Learn describes the isolation behaviour but not the silo internals."
---

# Windows Job Objects and Containers (Server Silos)

## Summary

A **job object** lets Windows manage a group of processes as one unit: apply limits (memory, CPU, number of active processes, UI access), get notifications, and terminate the whole group at once. Browsers and sandboxes put their renderer processes in jobs so a compromised renderer cannot, for example, spawn new processes. Windows containers are built on the same idea: in **process isolation** mode, a container is a set of processes that share the host kernel but get their own view of the file system, registry, object namespace and network. Internally that container boundary is a **server silo**, a job extended with its own namespaces. **Hyper-V isolation** instead runs each container inside a lightweight utility VM with its own kernel.

## Key ideas

| Concept | Meaning |
|---|---|
| **Job object** | Named, securable kernel object; every process assigned to it is subject to its limits |
| **Assignment is permanent** | Once a process is in a job it cannot leave; children inherit the job by default |
| **Nested jobs** | Since Windows 8 / Server 2012 a process can belong to a hierarchy of jobs |
| **Limits and notifications** | Working set, process priority, CPU rate, active process count, UI restrictions; completion port notifications on events |
| **Process isolation (container)** | Shared host kernel; namespace isolation for file system, registry, network, object namespace |
| **Hyper-V isolation** | Each container runs in a highly optimised VM with its own kernel |

## How it works

1. A process creates a job (`CreateJobObject`) and sets limits with `SetInformationJobObject`.
2. Processes are added with `AssignProcessToJobObject`; children created afterwards are in the job too.
3. The kernel enforces the limits on every member, for example failing a new process creation when the active-process limit is reached, and posts notifications to a completion port.
4. For a container, the host's container service creates the silo, gives it a private object namespace, a virtualised registry and file system view and its own network compartment, and starts the container's processes inside it.

## Security relevance

- **Sandbox hardening.** A job limited to one active process stops a compromised renderer or document viewer from launching `cmd.exe` or a second-stage payload. Job UI restrictions limit access to the clipboard and other desktops.
- **Container boundary.** Process-isolated containers share the kernel, so a kernel vulnerability or a mis-shared resource can lead to **escape to host** (T1611). Microsoft positions Hyper-V isolation for hostile multi-tenant workloads.
- **Forensics.** Tools that list processes may not show which job or silo a process belongs to; checking job membership explains why a process could not spawn children or why it saw a different registry.

## Defence

- Use Hyper-V isolation for untrusted container workloads; keep process isolation for trusted, single-tenant workloads.
- In your own services, put untrusted helpers in a job with a process-count limit and kill-on-close.
- Patch the host kernel promptly; process-isolated containers inherit its vulnerabilities.

## ATT&CK mapping

- **T1611 Escape to Host**: breaking out of a container boundary to the host.

## Explanation

A job is a supervised field trip: the teacher sets the rules once (no more than ten children, home by three, nobody leaves the group) and every child who joins the trip, including anyone who joins later, follows them. A process-isolated container is a group of pupils given their own classroom, own lockers and own notice board inside the same school building; a Hyper-V container is sending them to a separate building altogether. The separate building is safer if you do not trust the pupils, because a broken wall in a shared building affects everyone.
