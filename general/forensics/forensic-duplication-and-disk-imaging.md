---
title: "Forensic Duplication and Disk Imaging"
domain: [General]
category: [Forensics]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf
  - https://www.cftt.nist.gov/disk_imaging.htm
open_issues: []
---

# Forensic Duplication and Disk Imaging

## Summary

**Forensic duplication (disk imaging)** is the process of creating an exact, bit-for-bit copy of a storage device so that analysis is performed on the copy while the original is preserved untouched as evidence. "Bit-for-bit" is the crucial distinction from an ordinary file copy: a forensic image captures **every sector** — allocated files, deleted-but-not-overwritten data, slack space, and unallocated areas — because deleted and hidden data is often the most valuable evidence. The process requires a **write blocker** (hardware or software) on the source so that the act of imaging cannot alter the original, and it requires **hash verification**: the imaging tool computes a cryptographic hash of the source and the resulting image and confirms they match, proving the copy is faithful (and later re-verification proves nothing changed). Images are stored in standard formats — **raw/dd** (a plain sector copy) or **E01/EWF** (Expert Witness Format, which adds compression, metadata, and embedded hashes). Related concepts include **live imaging** (capturing a running system's disk when it cannot be powered off, accepting some inconsistency), and the growing challenge of **full-disk encryption** (an image of an encrypted drive is useless without the key, pushing collection toward live acquisition or key recovery). Tools include dd/dcfldd, FTK Imager, and Guymager. The underlying principle ties back to evidence integrity: you analyze a verified, write-protected duplicate so the original stays pristine and the findings remain defensible.

## Imaging essentials

| Element | Why it matters |
|---|---|
| **Bit-for-bit copy** | Captures deleted data, slack, unallocated space |
| **Write blocker** | Imaging must not alter the source |
| **Hash verification** | Proves the image faithfully matches the source |
| **Format (raw/E01)** | E01 adds metadata, compression, embedded hash |
| **Live imaging** | For systems that can't be powered off |
| **Encryption challenge** | Image of encrypted disk needs the key |

The whole point is a verified, write-protected duplicate: analyze that, keep the original pristine, and the evidence stays defensible.

## Detection / value

- **Deleted and hidden data**: a sector-level image recovers files and fragments an ordinary copy would miss.
- **Integrity proof**: matching source and image hashes demonstrate the copy is exact and unaltered.
- **Source preservation**: write blockers keep the original admissible by preventing any modification.
- **Encryption shifts strategy**: FDE forces live acquisition or key recovery, since a cold image is unreadable.

## Practices

- **Always use a write blocker** on the source during imaging.
- **Hash source and image and verify they match**; record values and algorithm.
- **Prefer E01** for metadata/embedded hashes, or raw/dd where tooling requires.
- **Plan for live imaging** when power-off is impossible, and document the inconsistency trade-off.
- **Account for encryption**: capture keys or image live before shutdown loses access.

## Explanation

Before an investigator analyzes a suspect computer, they make a perfect clone of its drive and then work only on the clone, leaving the original sealed away as evidence. But "perfect clone" means something stricter than dragging files to a backup. A forensic image copies the disk sector by sector, including the spaces the operating system considers empty — because when you "delete" a file, the data usually lingers there until something else overwrites it, and that lingering data is often exactly what the investigator is after. Two safeguards make the clone trustworthy. A write blocker sits between the examiner and the original drive, allowing reading but physically preventing any writing, so the simple act of copying can't accidentally change the evidence. And a hash — a unique digital fingerprint — is taken of both the original and the clone; if they match, you've proven the copy is a true, complete duplicate, and if they match again later, you've proven nothing's been altered. Modern wrinkles complicate this: if the drive is fully encrypted, a clone of the scrambled bits is useless without the key, so examiners increasingly have to capture the data while the machine is still running and unlocked. The constant, though, is the principle: never touch the original, work on a verified copy, and always be able to prove the copy is faithful.
