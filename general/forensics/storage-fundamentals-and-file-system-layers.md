---
title: "Storage Fundamentals and File System Layers for Forensics"
domain: [General]
category: [Forensics, Internals]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf
  - https://www.sleuthkit.org/sleuthkit/docs.php
open_issues: []
---

# Storage Fundamentals and File System Layers for Forensics

## Summary

Effective disk forensics requires understanding how storage is organized into **layers**, because evidence lives at every level and deleted or hidden data is recoverable precisely because of how these layers work. From bottom to top: the **physical/media layer** (sectors on an HDD/SSD, the smallest addressable units); the **volume/partition layer** (how the disk is divided into partitions and volumes — unpartitioned space can hide data); the **file system layer** (metadata structures like the **MFT** in NTFS or **inodes** in ext/UFS that track where each file's data lives, its timestamps, and its name); the **file/data layer** (the actual file contents in data blocks); and the **application/content layer** (the meaning of the data — documents, logs, databases). A forensic toolkit such as **The Sleuth Kit** maps directly onto these layers. Several forensically vital phenomena arise from this structure: **deleted files** typically leave their data blocks intact (only the metadata pointer is freed) until overwritten, so they are recoverable; **slack space** (the gap between a file's real size and its allocated blocks) can contain remnants of previously stored data; **unallocated space** holds deleted content and fragments; and **file system metadata** preserves rich timestamps (MAC times — modified, accessed, changed/created) essential for timelines. Understanding these layers is what lets an examiner recover deleted evidence, build accurate timelines, detect hidden data, and interpret what happened — and it is why simply "deleting" a file rarely destroys it.

## Storage layers

| Layer | What it holds | Forensic value |
|---|---|---|
| **Physical/media** | Sectors on disk | Raw imaging, bad-sector data |
| **Volume/partition** | Partition table, volumes | Hidden/unpartitioned space |
| **File system** | MFT/inodes, metadata | Timestamps, file-to-block mapping |
| **File/data** | File contents in blocks | Recovery of deleted data, slack |
| **Application** | Document/log/DB meaning | Interpreting content |

Deleted and hidden data survive because deletion usually frees a pointer, not the blocks — the layers are where recovery and timelines come from.

## Detection / value

- **Deleted-file recovery**: freeing metadata leaves data blocks intact until overwritten, enabling carving and undeletion.
- **Slack and unallocated space**: remnants of prior files hide here and often contain key evidence.
- **MAC timestamps**: file-system metadata drives forensic timelines of when files were created, accessed, and modified.
- **Hidden data**: unpartitioned or unallocated regions can conceal data an application-level view never shows.

## Practices

- **Analyze at every layer**, not just the file system view the OS presents.
- **Carve unallocated space and slack** for deleted content and fragments.
- **Build timelines from MAC times** while understanding how each file system updates them.
- **Account for SSD specifics** (TRIM/garbage collection can erase deleted data faster than on HDDs).
- **Use layer-aware tools** (e.g. The Sleuth Kit) to move between physical, file system, and file views.

## Explanation

A hard drive isn't one thing; it's a stack of layers, like a building with a foundation, a floor plan, room labels, and the furniture inside. At the bottom are the raw sectors — the physical slots that store bits. Above that, the disk is divided into partitions. Above that sits the file system, which is essentially an index: it keeps a card catalog (NTFS calls it the MFT, Linux uses inodes) recording each file's name, timestamps, and which storage blocks hold its contents. Then come the actual file contents, and finally the human meaning — this block of bytes is a photo, that one is a spreadsheet. Understanding this stack is the secret to digital forensics, because it explains why "delete" almost never means "destroy." When you delete a file, the system usually just tears up the index card; the furniture — the actual data — stays in the room until something new is moved in on top of it. That's why examiners can resurrect deleted files, and why they comb through the "empty" spaces of a disk, where fragments of old files linger. The same index also timestamps everything, letting investigators reconstruct a minute-by-minute story of what was created, opened, and changed. Looking only at what the operating system politely shows you — the neatly labeled rooms — misses most of the evidence, which hides in the layers underneath.
