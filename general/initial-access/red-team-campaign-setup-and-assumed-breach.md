---
title: "Red Team Campaign Setup and Assumed Breach Exercises"
domain: [General]
category: [Initial Access, Command and Control, Tooling & Scripting]
difficulty: Advanced
verification: Verified
attack_ids: []
checked_against:
  - https://attack.mitre.org/
  - https://csrc.nist.gov/pubs/sp/800/115/final
open_issues: []
---

# Red Team Campaign Setup and Assumed Breach Exercises

## Summary

A **red-team campaign** is a planned, objective-driven emulation of a real adversary against an organization (with authorization), and its **setup** and the **assumed-breach** model are key to making it realistic and valuable. Campaign setup includes: **defining objectives** (specific goals that matter to the business — e.g. reach the crown-jewel data, demonstrate domain compromise), **rules of engagement and authorization** (scope, allowed techniques, deconfliction, and legal sign-off), **threat emulation** (modeling a relevant adversary's TTPs, often via ATT&CK), **infrastructure** (C2, redirectors, phishing setup), and **planning the operation** across the attack lifecycle. The **assumed-breach** model is an increasingly standard approach: rather than spending weeks (and budget) attempting initial access — which a determined attacker will eventually achieve anyway — the exercise **starts from a presumed foothold** (the red team is given or granted an initial position, such as a compromised workstation or a set of credentials). This focuses the engagement on what matters most: **detection and response to post-compromise activity** (lateral movement, privilege escalation, persistence, exfiltration), which is where most organizations are weakest and where the greatest learning lies. Assumed breach is more efficient and more representative of real risk, since preventing *all* initial access is unrealistic, and it ensures the exercise tests the defenses that matter when (not if) an attacker gets in. The core insight is that realistic red teaming is **objective-driven and increasingly assumes compromise**, concentrating on post-breach detection and response — because the valuable question is rarely "can they get in?" (they can) but "how far can they go before we catch them, and how well do we respond?"

## Campaign elements

| Element | Purpose |
|---|---|
| **Objectives** | Business-relevant goals (crown jewels) |
| **Rules of engagement** | Scope, authorization, deconfliction |
| **Threat emulation** | Model a relevant adversary (ATT&CK) |
| **Infrastructure** | C2, redirectors, phishing |
| **Assumed breach** | Start from a foothold; focus on post-compromise |

The valuable question is rarely "can they get in?" but "how far before we catch them?" — so assumed breach focuses on post-compromise detection and response.

## Security relevance

- **Realistic and efficient**: assuming breach skips unavoidable initial access to test what matters.
- **Post-compromise focus**: lateral movement, escalation, and exfiltration are where most orgs are weakest.
- **Objective-driven**: business-relevant goals make findings meaningful to leadership.
- **Detection/response test**: measures how far an attacker gets and how well defenders react.

## Practices

- **Define business-relevant objectives** and clear rules of engagement with authorization.
- **Emulate a relevant adversary** (ATT&CK-based) rather than generic testing.
- **Consider assumed breach** to focus on post-compromise detection and response.
- **Deconflict with defenders** and handle infrastructure/data securely.
- **Measure detection and response**, feeding results into improvement (purple teaming).

## Explanation

A red-team campaign is a carefully planned, authorized dress rehearsal for a real attack, and how you set it up determines whether it teaches you anything. Good setup starts with a clear goal that actually matters to the business — not "poke around," but "can you reach the customer database?" — plus firm rules about what's allowed, legal sign-off, and the infrastructure the team will use to run the operation. Then there's a smart shortcut that's become standard practice: the assumed-breach model. Traditionally, a red team might burn weeks trying to get their initial foothold, but here's the thing — a determined real attacker will *eventually* get in anyway; preventing every single break-in is a losing battle. So instead of spending the whole budget proving the obvious, assumed-breach exercises simply *hand* the red team a starting foothold — say, one compromised laptop — and get straight to the interesting part: now that they're in, how far can they spread, how fast can they grab the crown jewels, and — most importantly — does anyone on the defense side notice and stop them? This matters because post-break-in activity, like creeping from machine to machine and escalating privileges, is exactly where most organizations are blind and weak, and it's where the real lessons hide. The whole philosophy reflects a mature view of security: the useful question usually isn't "can they get in?" (assume yes) but "how far can they get before we catch them, and how well do we respond when we do?" Answering that honestly is what makes a red-team campaign worth running.
