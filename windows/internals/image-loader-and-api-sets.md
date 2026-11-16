---
title: "Windows Image Loader and API Sets"
domain: [Windows]
category: [Internals]
difficulty: Advanced
verification: Verified
attack_ids: [T1574.001, T1129, T1622]
checked_against:
  - https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order
  - https://learn.microsoft.com/en-us/windows/win32/apiindex/windows-apisets
  - https://learn.microsoft.com/en-us/windows/win32/apiindex/api-set-loader-operation
  - https://learn.microsoft.com/en-us/windows/win32/api/winternl/ns-winternl-peb_ldr_data
  - https://learn.microsoft.com/en-us/windows/win32/debug/pe-format
  - https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
  - https://attack.mitre.org/techniques/T1574/001/
  - https://attack.mitre.org/techniques/T1129/
  - https://attack.mitre.org/techniques/T1622/
open_issues: []
---

# Windows Image Loader and API Sets

## Summary

When a process starts, the **image loader** in `ntdll.dll` does a lot of work before the program's own code runs: it maps the executable, finds and maps every DLL the program imports (and their imports), fills in the import address table, runs TLS callbacks and DLL initialisers, and only then jumps to the entry point. How the loader turns a DLL *name* into a *file* is the root cause of DLL search-order hijacking, and the loader's bookkeeping in the PEB is what malware walks to find APIs without importing them. **API sets** add a layer of indirection: names like `api-ms-win-core-*.dll` are virtual and are redirected by the loader to whichever real DLL implements them on that edition of Windows.

## Loader steps

1. **Early initialisation**: the loader sets up the process heap, loader lock and compatibility shims.
2. **Name resolution** for each imported DLL (see below).
3. **Mapping** the DLL image and recording it in the loaded-module lists.
4. **Import processing**: walk the import directory, resolve each function by name or ordinal, write addresses into the IAT. Delay-loaded imports are resolved on first call instead.
5. **Initialisation**: run TLS callbacks listed in the PE TLS directory, then each DLL's `DllMain` with `DLL_PROCESS_ATTACH`.
6. **Entry point** of the executable.

## How a DLL name is resolved (unpackaged desktop apps)

Microsoft documents this order for a load that does not give a full path:

1. DLL redirection (e.g. `.local` redirection)
2. API sets
3. Side-by-side (SxS) manifest redirection
4. Loaded-module list (already loaded)
5. Known DLLs (`HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs`)
6. Package dependency graph (Windows 11 21H2 and later)
7. The folder the application was loaded from
8. The system folder (`System32`)
9. The 16-bit system folder
10. The Windows folder
11. The current folder
12. Directories in `PATH`

With safe DLL search mode disabled (`SafeDllSearchMode` = 0), the current folder moves up to just after the application folder. Calling `SetDllDirectory` effectively changes the order too.

## API sets

An API set name ending in `.dll` does not refer to a file on disk. When the loader meets one, it consults the **API set schema**, a mapping built into the OS, and redirects the import to the host binary for that device family. This lets the same program run on desktop, server and IoT editions where the implementing DLL differs. Analysts see these names in import tables and need to resolve them to the real host before reading the code.

## Loaded-module bookkeeping

The loader records each module in the process environment block: `PEB->Ldr` points to `PEB_LDR_DATA`, whose doubly linked lists (for example `InMemoryOrderModuleList`) hold an `LDR_DATA_TABLE_ENTRY` per module. Shellcode walks these lists to find `kernel32.dll` and its exports without any imports; rootkit-style malware unlinks its own entry to hide an injected DLL from tools that rely on the lists.

## Security relevance

- **DLL search-order hijacking (T1574.001)**: placing a DLL earlier in the search order than the real one, typically the application folder or a writable `PATH` directory. ATT&CK now folds DLL side-loading into this same sub-technique.
- **Shared modules (T1129)**: malware loading its own modules through the loader to execute payloads.
- **TLS callbacks run before the entry point**, so malware uses them for anti-debugging (T1622) and to run code before an analyst's breakpoint on `main`.
- **IAT** is a hook target (see API Hooking).

## Detection

- Sysmon image-load events (Event ID 7) for DLLs loaded from user-writable paths or with unexpected signers.
- Cross-check the loader lists against memory regions (e.g. Volatility `ldrmodules`) to find unlinked modules.
- Watch for modifications to the `KnownDLLs` key or `SafeDllSearchMode`.

## ATT&CK mapping

- **T1574.001 DLL** (search order hijacking and side-loading)
- **T1129 Shared Modules**
- **T1622 Debugger Evasion** (TLS callback tricks)

## Explanation

The loader is a stage manager getting a show ready. Before the curtain rises (the entry point), it fetches every actor the script names. If the script just says "the butler", the stage manager checks a fixed list of places in order: the reserved list of regulars, the company's own green room, the main theatre roster, and finally anyone wandering by. An attacker who sits in an early room wearing a "butler" badge gets cast. API sets are role names like "the butler" that the theatre maps to whichever actor plays that part at this venue.
