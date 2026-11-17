---
title: "Windows Kernel Deferred Execution (APCs, DPCs, Work Items, Timers, Callbacks)"
domain: [Windows]
category: [Internals, Execution]
difficulty: Advanced
verification: Verified
attack_ids: [T1055.004, T1014]
checked_against:
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/types-of-apcs
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/managing-hardware-priorities
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-dpcs
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/system-worker-threads
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/timer-objects-and-dpcs
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/completing-irps
  - https://attack.mitre.org/techniques/T1055/004/
  - https://attack.mitre.org/techniques/T1014/
open_issues: []
---

# Windows Kernel Deferred Execution (APCs, DPCs, Work Items, Timers, Callbacks)

## Summary

Kernel code often cannot finish its work where it starts. An interrupt handler must return quickly; a routine running at a high priority level cannot block or touch pageable memory; a driver may need to run code in a particular thread's context. Windows gives drivers several ways to **defer work**: **DPCs** for short work after an interrupt, **work items** run by system worker threads, **APCs** that run inside a chosen thread, **timers** that fire a DPC later, **completion routines** that run when an I/O request finishes, and **notification callbacks** for system events. Reverse engineers need to recognise all of them, because rootkits use exactly these hooks to run code at the moment and privilege level they want.

## The IRQL ladder

Every mechanism is tied to an **interrupt request level (IRQL)**, which decides what the code may do.

| IRQL | What runs there | Constraints |
|---|---|---|
| **PASSIVE_LEVEL** | `DriverEntry`, most dispatch routines, system threads, work items | May block and touch pageable memory |
| **APC_LEVEL** | Special kernel APCs, some dispatch routines | APCs are masked; still has thread context |
| **DISPATCH_LEVEL** | DPCs, `StartIo`, timer DPCs | No blocking, no page faults; scheduler is masked |
| **DIRQL** | Interrupt service routines | Only device interrupts with a higher level, clock and power-fail can preempt |

## Mechanisms

| Mechanism | Runs at / in | Typical use |
|---|---|---|
| **DPC** (`DpcForIsr`, `CustomDpc`) | DISPATCH_LEVEL, arbitrary thread context | Finish interrupt-driven I/O, start the next request |
| **Timer + DPC** | DISPATCH_LEVEL when the timer expires | Periodic or delayed work |
| **Work item** (`IoQueueWorkItem`) | PASSIVE_LEVEL in a system worker thread | Short work that must block or needs a system thread context |
| **System thread** | PASSIVE_LEVEL | Long-running driver work (worker pool is shared and limited) |
| **Special user-mode APC** | User mode, runs even if the thread is not alertable | Newer API (`QueueUserAPC2`) |
| **Regular user-mode APC** | User mode, only during an alertable wait | Overlapped I/O completion, `QueueUserAPC` |
| **Normal kernel APC** | Kernel, PASSIVE_LEVEL in the target thread | File systems and filters |
| **Special kernel APC** | Kernel, APC_LEVEL in the target thread | I/O completion |
| **Completion routine** | When a lower driver completes an IRP | Post-processing an I/O request |
| **Notification callbacks** | Kernel, on system events | Process/thread creation, image load, registry operations (`CmRegisterCallbackEx`) |

## Security relevance

- **APC injection.** The user-mode APC is the documented mechanism behind APC injection (T1055.004): queue an APC pointing at attacker code to a thread in another process and wait for it to become alertable. Kernel-mode implants use kernel APCs to push code into a user process from a driver.
- **Stealthy timing.** Rootkits use timers and DPCs to run periodically without owning a visible thread, and work items to borrow a system worker thread (T1014).
- **Callbacks cut both ways.** Process, thread, image-load and registry callbacks are how EDRs see activity; kernel attackers enumerate and remove those callbacks to blind them.
- **IRQL bugs.** Touching pageable memory at DISPATCH_LEVEL crashes the machine; analysts use this to reason about what a suspicious routine can safely be doing.

## Detection and defence

- In memory forensics, enumerate registered callbacks, timers and DPCs and attribute each routine address to a loaded, signed module; addresses outside any module are suspicious.
- Watch for drivers that remove or overwrite EDR callbacks; HVCI and the vulnerable driver blocklist reduce the ability to load such drivers.
- In user mode, cross-process APC queuing is a high-signal injection behaviour.

## ATT&CK mapping

- **T1055.004 Asynchronous Procedure Call**
- **T1014 Rootkit**

## Explanation

Think of the kernel as an emergency room. The triage nurse (interrupt handler) spends seconds per patient and writes a note for later (a DPC). Paperwork that needs a quiet desk goes to the back office pool (work items). Some tasks must be done by a particular patient's own doctor in that patient's room (APCs). Alarm clocks remind staff to check in later (timers), and a pager list says who to call when someone is admitted or discharged (callbacks). An intruder who learns this routine can leave notes, set alarms, or put themselves on the pager list, and defenders find them by checking every note and pager entry against the staff directory.
