---
title: "Forensic Data Analysis Techniques"
domain: [General]
category: [Forensics]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf
  - https://www.sleuthkit.org/sleuthkit/docs.php
open_issues: []
---

# Forensic Data Analysis Techniques

## Summary

Once a forensic image is acquired and verified, the **analysis** phase turns raw bytes into a reconstructed account of what happened. The core techniques are: **timeline analysis** (correlating file-system MAC timestamps, log entries, and artifact times into a chronological narrative — the backbone of most investigations); **file carving** (recovering files from unallocated space or fragments by recognizing file **signatures/headers and footers**, independent of file-system metadata, which recovers deleted data even when the index is gone); **keyword and pattern searching** (indexed searches across the image for names, terms, or indicators); **hash analysis** (using **known-file hash sets** to filter out known-good OS files and flag known-bad files, dramatically reducing what a human must review); **artifact analysis** (parsing OS and application artifacts — browser history, registry/plist entries, link files, prefetch, email stores — for user and attacker activity); **metadata and EXIF** examination; and **data/file-type recovery** for deleted or hidden content. A central practical principle is **data reduction**: forensic images are huge, so techniques that cut the haystack — hash filtering of known files, deduplication, and targeted searching — are what make analysis feasible. The techniques combine to answer the investigative questions: what files existed, who did what and when, what was deleted or hidden, and what the content means. Throughout, the examiner works on the verified copy and documents each step, so the reconstruction stays reproducible and defensible.

## Analysis techniques

| Technique | Yields |
|---|---|
| **Timeline analysis** | Chronological narrative of events |
| **File carving** | Deleted/fragmented files via signatures |
| **Keyword/pattern search** | Relevant terms, names, indicators |
| **Hash set analysis** | Filter known-good, flag known-bad |
| **Artifact parsing** | Browser/registry/email/prefetch activity |
| **Metadata/EXIF** | Authorship, device, timestamps |

Data reduction is the enabler: hash filtering and targeted search shrink enormous images into a reviewable set of relevant evidence.

## Detection / value

- **Timelines** place activity in order, revealing the sequence of an intrusion or user action.
- **Carving** recovers evidence that deletion and file-system damage would otherwise hide.
- **Known-file hashing** removes thousands of irrelevant OS files, focusing effort on the unusual.
- **Artifacts** expose concrete behavior — sites visited, programs run, files opened, messages sent.

## Practices

- **Build a super-timeline** from file system, logs, and artifacts for a unified chronology.
- **Carve unallocated space** by signature to recover deleted and fragmented files.
- **Apply hash sets** to filter known-good and highlight known-bad quickly.
- **Parse OS/app artifacts** for user and attacker activity; corroborate across sources.
- **Document every tool and step** so the analysis is reproducible and defensible.

## Explanation

Acquiring the disk image is just collecting the haystack; analysis is finding the needles and, more importantly, piecing together the story. The single most powerful technique is building a timeline. Every file carries hidden timestamps, logs are stamped with times, and dozens of little system records note when things happened; stitch them together and a dry pile of data becomes a minute-by-minute account — the attacker logged in here, ran this, copied that, deleted this at 2:14 a.m. To recover things someone tried to erase, examiners use carving: instead of trusting the disk's index (which deletion scrambles), they scan the raw bytes for the telltale opening and closing signatures that mark the start and end of, say, a JPEG or a PDF, and reassemble the file directly. Because disks are enormous, a huge part of the craft is *shrinking* the problem: investigators keep giant lists of fingerprints for ordinary, known files — the millions of boring operating-system files — and automatically set those aside, so a human only ever looks at the unusual remainder. The rest is reading the digital exhaust everyone leaves behind: browser history, records of which programs ran, opened-file traces, email archives. Woven together, these answer the investigator's real questions — who did what, when, and what it all means.
