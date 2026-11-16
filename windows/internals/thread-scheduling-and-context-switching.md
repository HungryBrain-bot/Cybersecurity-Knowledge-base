---
title: "Thread Scheduling and Context Switching"
domain: [Windows]
category: [Internals]
difficulty: Advanced
verification: Needs review
attack_ids: [T1055.003, T1497.003]
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/procthread/scheduling-priorities
  - https://learn.microsoft.com/en-us/windows/win32/procthread/context-switches
  - https://learn.microsoft.com/en-us/windows/win32/procthread/priority-boosts
  - https://attack.mitre.org/techniques/T1055/003/
  - https://attack.mitre.org/techniques/T1497/003/
open_issues:
  - "Thread state names (standby, deferred ready) and per-processor dispatcher database details come from Windows Internals Part 1 and are not described on Microsoft Learn."
---

# Thread Scheduling and Context Switching

## Summary

Windows schedules **threads**, not processes. The scheduler is **preemptive and priority-based**: every thread has a priority from 0 to 31, the highest-priority ready thread always runs, and threads of equal priority share the processor round-robin in time slices called **quanta**. A **context switch** saves one thread's processor state and restores another's. Priorities move: Windows temporarily **boosts** a thread when its wait completes or its window gets input, then decays it back. This is mostly a performance topic, but it explains why thread hijacking works, how APCs get delivered, and why timing checks in sandboxes are unreliable.

## Key ideas

| Concept | Meaning |
|---|---|
| **Priority 0–31** | 0 is reserved for the zero-page thread; 1–15 are the dynamic range; 16–31 are the real-time range |
| **Base priority** | Process priority class combined with the thread's relative priority |
| **Dynamic priority** | The value the scheduler actually uses; can be boosted above base, never below it |
| **Quantum** | The time slice a thread gets before the scheduler considers another thread at the same priority |
| **Ready queues** | One queue per priority level; the scheduler takes the head of the highest non-empty queue |
| **Not ready** | Threads created suspended, suspended with `SuspendThread`, or waiting on an object or input |

## How a context switch works

1. Save the context (registers, stack pointer, instruction pointer) of the thread that was preempted or yielded.
2. If that thread is still ready, put it at the end of the queue for its priority.
3. Find the highest-priority queue that has a ready thread.
4. Take the thread at the head of that queue, restore its context and run it.

## Priority boosts

- A process in the normal priority class that comes to the foreground is boosted relative to background processes.
- A thread that owns a window receiving input (keyboard, mouse, timer messages) is boosted.
- A thread whose wait completes (for example disk or keyboard I/O) is boosted.
- After a boost, the dynamic priority drops one level per completed time slice until it reaches the base priority. Boosting can be turned off per process or thread (`SetProcessPriorityBoost`, `SetThreadPriorityBoost`). Threads in the real-time range are not boosted.

On multiprocessor systems the scheduler also weighs the thread's ideal processor, its affinity mask, NUMA locality and, on hybrid CPUs, core type.

## Security relevance

- **Thread execution hijacking (T1055.003).** An attacker suspends a thread in another process, rewrites its saved context so the instruction pointer points at injected code, and resumes it. This is the save/restore step of a context switch used against its owner.
- **APC delivery.** User-mode APCs queued to a thread only run when that thread enters an alertable wait, so injection that relies on `QueueUserAPC` depends on scheduling and wait behaviour (see APC Injection).
- **Time-based sandbox evasion (T1497.003).** Malware sleeps or measures elapsed time to detect accelerated sandbox clocks; the scheduler's quanta and boosts make naive timing noisy for both sides.
- **Starvation.** A thread raised into the real-time range by an administrator can starve other work; this is an availability concern more than a confidentiality one.

## Detection

- Remote `SuspendThread` / `SetThreadContext` / `ResumeThread` sequences against another process are a strong injection signal; EDRs and Sysmon process-access events (granted rights including thread context access) help surface them.
- Unexpected real-time priority on user processes is worth investigating.

## ATT&CK mapping

- **T1055.003 Thread Execution Hijacking**
- **T1497.003 Time Based Checks**

## Explanation

Imagine a single checkout lane with a strict rule: the most important customer in line is always served next, and customers of equal importance take turns for a fixed number of minutes each. When someone's turn ends, the cashier notes exactly where they were in their shopping list and calls the next person. Thread hijacking is someone slipping into the queue, swapping a sleeping customer's shopping list for their own, and letting the cashier carry on as if nothing happened.
