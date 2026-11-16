---
title: "Windows Cache Manager and File Systems (NTFS, ReFS, Filter Drivers)"
domain: [Windows]
category: [Internals]
difficulty: Advanced
verification: Verified
attack_ids: [T1564.004, T1006, T1685]
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/fileio/file-caching
  - https://learn.microsoft.com/en-us/windows/win32/fileio/master-file-table
  - https://learn.microsoft.com/en-us/windows/win32/fileio/change-journals
  - https://learn.microsoft.com/en-us/windows/win32/fileio/file-streams
  - https://learn.microsoft.com/en-us/windows/win32/fileio/hard-links-and-junctions
  - https://learn.microsoft.com/en-us/windows/win32/fileio/reparse-points
  - https://learn.microsoft.com/en-us/windows-server/storage/refs/refs-overview
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/ifs/filter-manager-concepts
  - https://learn.microsoft.com/en-us/windows-hardware/drivers/ifs/load-order-groups-and-altitudes-for-minifilter-drivers
  - https://attack.mitre.org/techniques/T1564/004/
  - https://attack.mitre.org/techniques/T1006/
  - https://attack.mitre.org/techniques/T1685/
open_issues: []
---

# Windows Cache Manager and File Systems (NTFS, ReFS, Filter Drivers)

## Summary

Between a program calling `ReadFile` and bytes moving on disk sit three layers that matter to security work. The **cache manager** keeps recently used file data in memory and decides when changes are written back. **File system drivers** (NTFS, ReFS, FAT/exFAT, UDF and network redirectors) turn file operations into on-disk structures. **Filter drivers**, almost always **minifilters** managed by the Filter Manager, sit on top of the file system and see every operation first; antivirus, EDR, encryption and backup products all live here. NTFS's own metadata, especially the **Master File Table** and the **change journal**, is also the richest source of file system forensic evidence on Windows.

## Cache manager

- One centralised cache for all file systems, organised by file stream rather than by disk block.
- Data is mapped into system address space in fixed-size views ("slots") and copied to the caller.
- **Read-ahead** predicts sequential reads and fetches data early.
- **Lazy writer**: dirty pages are not written immediately; a lazy writer runs every second and queues a portion of unflushed pages for writing. Applications that need durability request **write-through** for specific operations.
- Implication: a file's latest contents may exist only in memory for a short time, which matters for crash-consistency, ransomware timing and memory forensics.

## File systems

| File system | Notes |
|---|---|
| **NTFS** | Default for Windows volumes; MFT, ACLs, alternate data streams, hard links, junctions and reparse points, change journal, compression, EFS encryption |
| **ReFS** | Resilient File System for Windows Server storage; integrity streams, block cloning, designed for large-scale and virtualised storage |
| **FAT32 / exFAT** | Removable media; no ACLs, limited metadata |
| **UDF / CDFS** | Optical media |
| **Network redirectors** | SMB client presents remote shares as file systems |

### NTFS structures that matter

- **Master File Table (MFT)**: at least one entry per file, including the MFT itself. Size, timestamps, permissions and small file contents are stored in the entry or described by it. Deleted files' entries are marked free and reused later, which is why deleted-file recovery is possible.
- **Alternate data streams**: a file can carry extra named streams beyond its main data (see NTFS Alternate Data Streams).
- **Hard links, junctions and reparse points**: let one path point at other content; abused for redirection and for confusing privileged file operations.
- **Change journal (USN journal)**: a per-volume log of changes, created so backup and indexing tools need not rescan the disk; forensically it records file creation, renames and deletions.

## Filter drivers and the Filter Manager

`FltMgr.sys` attaches to each volume's file system stack. Minifilters register with it for the operations they care about and are ordered by **altitude**: a number assigned per product that, together with **load order groups** (anti-virus, encryption, backup and others), fixes who sees an operation first.

## Security relevance

- **Security products are minifilters.** Unloading or detaching an EDR's minifilter, or preventing it from loading, blinds file monitoring (T1685 Disable or Modify Tools). `fltmc` lists loaded filters and their altitudes, which is useful to both sides.
- **Direct volume access (T1006).** Reading the raw volume bypasses the file system and its filters, letting an attacker copy locked files such as registry hives or `NTDS.dit`.
- **Hidden data (T1564.004)** in alternate data streams.
- **Link and reparse-point tricks** turn a privileged service's file write into an arbitrary file overwrite.
- **Forensics.** MFT and USN journal parsing reconstructs activity even after files are deleted or timestamps altered.

## Detection

- Baseline `fltmc filters` output; a missing EDR filter or an unknown filter at an unusual altitude is a strong signal.
- Sysmon Event ID 9 (RawAccessRead) flags raw volume reads.
- Collect `$MFT` and `$UsnJrnl:$J` in triage packages for timeline analysis.

## ATT&CK mapping

- **T1685 Disable or Modify Tools** (minifilter tampering)
- **T1006 Direct Volume Access**
- **T1564.004 NTFS File Attributes**

## Explanation

Picture a busy warehouse. Near the door is a fast staging area (the cache) where popular boxes are kept and where new deliveries wait before a clerk shelves them every second (the lazy writer). The shelving system itself (NTFS) has a master ledger listing every box and where it lives, plus a diary of every change. Security guards (minifilters) stand in a fixed order at the door and inspect each box; whoever stands closest to the door sees it first. An intruder can try to send a guard home, climb in through a window that skips the door (raw volume access), or hide items in a box's secret compartment.
