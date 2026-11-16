---
title: "Encryption Key Recovery from Memory"
domain: [Windows, Linux, macOS]
category: [Forensics, Cryptography, Credential Access]
difficulty: Advanced
verification: Verified
attack_ids: [T1555.001, T1003]
checked_against:
  - https://github.com/volatilityfoundation/volatility3/blob/develop/volatility3/framework/plugins/windows/truecrypt.py
  - https://github.com/volatilityfoundation/volatility/blob/master/volatility/plugins/tcaudit.py
  - https://github.com/volatilityfoundation/volatility/blob/master/volatility/plugins/mac/keychaindump.py
  - https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/countermeasures
  - https://attack.mitre.org/techniques/T1555/001/
  - https://attack.mitre.org/techniques/T1003/
open_issues: []
---

# Encryption Key Recovery from Memory

## Summary

Full-disk encryption protects data at rest, but a running system has to decrypt data constantly, so the keys must be in RAM while a volume is mounted or a credential store is unlocked. A memory image of a running machine can therefore yield the keys or passphrases needed to open an encrypted volume, and secrets from credential stores. For investigators this is a legitimate way past encryption on a seized, running system. For defenders it explains why physical access to an unlocked or sleeping machine is so dangerous, and which countermeasures matter.

## What can be recovered

| Target | Approach | Tooling (verified plugin names) |
|---|---|---|
| **TrueCrypt volumes** | Find the cached passphrase or master key in driver memory | Volatility 3 `windows.truecrypt.Passphrase`; Volatility 2 `truecryptpassphrase`, `truecryptmaster`, `truecryptsummary` |
| **macOS Keychain** | Recover candidate keychain master keys from memory | Volatility 2 `mac_keychaindump` (T1555.001) |
| **BitLocker** | Keys are present in memory once pre-boot authentication completes | Specialised third-party tooling; not in Volatility core |
| **Credential material in general** | Hashes, tickets and keys in process memory | See [Credential Dumping](../credential-access/credential-dumping.md) (T1003) |

Microsoft's BitLocker documentation states that BitLocker stores the encryption keys in memory only after preboot authentication completes, and describes DMA and memory-remanence attacks as threats to address.

```mermaid
flowchart LR
    A[Running system with mounted volume]:::detect --> B[Capture memory first]:::detect
    B --> C[Search for cached keys or passphrases]:::detect
    C --> D[Decrypt the volume image offline]:::defend
    classDef detect fill:#1f6feb,color:#fff
    classDef defend fill:#2da44e,color:#fff
```

## Forensic practice

- **Never pull the plug first** on a running system with encrypted volumes. Capture memory, then image the mounted (decrypted) volume if possible, then shut down.
- Record which volumes were mounted and which users were logged on at capture time.

## Defensive implications

| Threat | Countermeasure (from Microsoft's BitLocker guidance) |
|---|---|
| Attacker boots the machine and reads keys that load automatically | **Pre-boot authentication**, e.g. TPM plus PIN, so keys are not released without the user |
| DMA attacks through external ports | Protect DMA ports; policy to disable new DMA devices while the computer is locked |
| Memory remanence (reading RAM after power-off) | Features that mitigate DMA and memory remanence attacks; avoid leaving machines in sleep with volumes unlocked |

## ATT&CK mapping

- **T1555.001 Keychain**: recovering macOS keychain secrets.
- **T1003 OS Credential Dumping**: the broader family of extracting secrets from memory.

## Explanation

A safe is only safe while it's shut. While someone is working with it open, the combination doesn't matter. Memory capture is walking in while the safe is open; pre-boot authentication and DMA protection are making sure nobody can open it without the owner standing there.
