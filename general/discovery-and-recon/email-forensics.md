---
title: "Email Forensics"
domain: [General]
category: [Discovery & Recon]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://datatracker.ietf.org/doc/html/rfc5322
  - https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf
open_issues: []
---

# Email Forensics

## Summary

**Email forensics** is the analysis of email to investigate its origin, authenticity, and content — central to cases involving phishing, business email compromise (BEC), fraud, harassment, data exfiltration, and policy violations. The richest evidence is the **email header** (RFC 5322), which records the message's journey: the **`Received:` headers** (added by each mail server in transit, read bottom-up, revealing the path and often the true originating IP), **`From`/`Reply-To`/`Return-Path`** (which can be spoofed and compared for inconsistencies), **`Message-ID`**, timestamps, and **authentication results** — **SPF, DKIM, and DMARC** — which indicate whether the sending domain was authorized and the message unaltered (failures or absence are strong signals of spoofing/phishing). Analysts also examine **attachments and links** (for malware or credential-phishing), **metadata**, and the **mail store** (PST/OST/mbox/maildir files, or server/cloud mailboxes and their logs). Investigations reconstruct what was sent, when, by whom, and whether headers were forged, and correlate email evidence with endpoint and network data. Key challenges: headers can be **partially forged** (though `Received:` lines added by trusted servers are harder to fake), and much email now lives in **cloud services** (requiring provider logs and legal process). The value is that email is both a leading **attack vector** (phishing/BEC) and a dense **evidence source**: careful header and authentication analysis exposes spoofing and traces origin, while the mailbox and logs reveal the sequence of a compromise or fraud. As always, evidence must be collected with forensic discipline to remain credible.

## Email evidence sources

| Source | Yields |
|---|---|
| **Received: headers** | Transit path, originating IP (read bottom-up) |
| **From/Reply-To/Return-Path** | Spoofing inconsistencies |
| **SPF/DKIM/DMARC results** | Authorized sender? Unaltered? |
| **Attachments/links** | Malware, credential phishing |
| **Mail store / cloud logs** | Message history, access records |

Headers and authentication results are the core: they expose spoofing and trace origin, while mailbox and provider logs reconstruct the sequence.

## Detection / value

- **Header analysis traces origin**: `Received:` lines reveal the path and often the true source.
- **Authentication failures flag spoofing**: SPF/DKIM/DMARC results expose forged senders (phishing/BEC).
- **Inconsistencies reveal forgery**: mismatched From/Reply-To/Return-Path indicate manipulation.
- **Mailbox/logs reconstruct events**: what was sent/accessed, when, and by whom.

## Practices

- **Analyze full headers** (read `Received:` bottom-up) and check SPF/DKIM/DMARC results.
- **Compare From/Reply-To/Return-Path** for spoofing inconsistencies.
- **Safely examine attachments/links** (sandbox) for malware or phishing.
- **Collect from mail stores and cloud provider logs** with proper authority and forensic discipline.
- **Correlate email with endpoint/network evidence** to reconstruct the incident.

## Explanation

Email is both the front door most attacks walk through and a surprisingly detailed logbook of what happened — which makes email forensics a core investigative skill. The real treasure isn't the message you see; it's the hidden header attached to every email, which records its entire journey. As a message hops from server to server, each one stamps a "Received" line onto the header, so by reading those lines from the bottom up, an investigator can often trace the message back toward where it truly came from — even when the visible "From" address is a lie. Spoofing the sender is trivial, which is why analysts cross-check several address fields for inconsistencies and, crucially, look at the results of three modern authentication checks (SPF, DKIM, and DMARC) that a receiving server performs to ask "was this message really authorized by the domain it claims, and was it tampered with?" When those checks fail or are missing, it's a loud signal of forgery — the hallmark of phishing and business-email-compromise scams. Beyond the headers, investigators examine attachments and links for malware, and dig into the mailbox itself (and, increasingly, the cloud provider's access logs) to reconstruct exactly what was sent, received, and read, and when. Two realities shape the work: parts of a header can be faked, though the stamps added by trusted servers are much harder to forge, and because so much email now lives in the cloud, getting the full picture often requires the provider's cooperation and proper legal process. Handled carefully, email evidence both unmasks the spoofer and lays out the timeline of a fraud or breach.
