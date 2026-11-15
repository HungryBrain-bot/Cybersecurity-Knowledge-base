---
title: "Cryptographic Hash Functions"
domain: [General]
category: [Cryptography]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf
  - https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-107r1.pdf
open_issues: []
---

# Cryptographic Hash Functions

## Summary

A **cryptographic hash function** maps an input of any length to a fixed-length digest (e.g. 256 bits) and is designed so that the digest behaves like a unique, tamper-evident fingerprint of the input. Three security properties define it: **preimage resistance** (given a digest, you cannot find an input that produces it), **second-preimage resistance** (given an input, you cannot find a different input with the same digest), and **collision resistance** (you cannot find *any* two distinct inputs with the same digest). Because the output is fixed-size, collisions exist mathematically; the point is that they must be computationally infeasible to find. Collision resistance is the hardest to maintain and is weakened by the **birthday bound**: finding a collision in an n-bit hash takes roughly 2^(n/2) work, so a 256-bit hash gives about 128-bit collision strength. **MD5 and SHA-1 are broken** for collision resistance and must not be used where collisions matter (certificates, signatures); the current standards are **SHA-2** (FIPS 180-4) and **SHA-3** (FIPS 202). Hashes underpin integrity checks, digital signatures, commitment schemes, and password storage — though **password hashing** requires deliberately slow, salted functions (bcrypt, scrypt, Argon2, PBKDF2), not raw SHA-2, because fast hashes are easy to brute-force.

## Properties and uses

| Property | Meaning | If broken |
|---|---|---|
| **Preimage resistance** | Can't invert a digest to an input | Reverse hashed values |
| **Second-preimage** | Can't match a given input's digest with another | Substitute a file undetected |
| **Collision resistance** | Can't find any two inputs that collide | Forge signatures/certificates |
| **Birthday bound** | Collision work ≈ 2^(n/2) | Guides output-size choice |

Integrity, signatures, and commitments rely on these properties; **passwords need slow, salted KDFs** (Argon2/bcrypt/scrypt/PBKDF2), not plain hashes.

## Security relevance

- **MD5/SHA-1 collisions are practical**: real certificate and file forgeries exist; treat both as broken for security use.
- **Length-extension**: Merkle–Damgård hashes (SHA-256) allow length-extension, so do not build a MAC as `hash(secret || message)` — use HMAC or SHA-3/KMAC.
- **Fast hashes are wrong for passwords**: raw SHA-256 enables billions of guesses per second; use memory-hard KDFs with per-user salts.
- **Output size sets collision strength**: 256-bit output ≈ 128-bit collision resistance via the birthday bound.

## Practices

- **Use SHA-256/SHA-3** for integrity and signatures; retire MD5 and SHA-1.
- **Use HMAC** (not raw `hash(key||msg)`) for keyed authentication.
- **Use Argon2id/bcrypt/scrypt** with salts for passwords; tune cost to hardware.
- **Verify digests over a trusted channel**; a hash only proves integrity if the reference value is authentic.

## Explanation

A cryptographic hash is a machine that turns any file, however big, into a short, fixed-length "fingerprint." Good fingerprints have three qualities: you can't work backwards from the fingerprint to the file, you can't cook up a different file that matches a given file's fingerprint, and you can't find any two files that happen to share one. The last is the slipperiest, and there's a mathematical ambush called the birthday problem: matching fingerprints by chance is far easier than intuition suggests, so a fingerprint needs to be twice as long as the security you actually want. Two once-trusted functions, MD5 and SHA-1, have fallen to exactly this — attackers can now manufacture collisions, which is how fraudulent certificates have been forged — so the modern choices are SHA-2 and SHA-3. One more trap catches people constantly: a hash is built for speed, which is great for checking a download but terrible for storing passwords, because a fast fingerprint lets an attacker try billions of guesses a second. Passwords get a different, deliberately slow and salted kind of function so each guess is expensive.
