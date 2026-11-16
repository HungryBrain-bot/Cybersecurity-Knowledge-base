---
title: "Encryption Security Notions and Cryptographic Security Levels"
domain: [General]
category: [Cryptography]
difficulty: Advanced
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r5.pdf
  - https://datatracker.ietf.org/doc/html/rfc8439
  - https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197-upd1.pdf
open_issues: []
---

# Encryption Security Notions and Cryptographic Security Levels

## Summary

Cryptography is judged against **formal security notions** that say precisely what an attacker is allowed to do and what they must be unable to achieve. For encryption, the notions are built around **indistinguishability**: an attacker who picks two plaintexts and sees the encryption of one cannot tell which it was, even with specified powers. **IND-CPA** (chosen-plaintext) assumes the attacker can get arbitrary plaintexts encrypted; achieving it requires **randomized or nonce-based** encryption, which is why deterministic schemes like ECB fail. **IND-CCA** (chosen-ciphertext) additionally lets the attacker submit ciphertexts to a decryption oracle, modeling real systems that decrypt attacker-supplied data; achieving it requires **integrity**, which is why authenticated encryption (AEAD) is the practical target. The companion idea is the **security level**, measured in bits: a scheme has "128-bit security" if the best attack costs about 2^128 operations. Key sizes are chosen to hit a target level — symmetric keys roughly equal their bit length (AES-128 ≈ 128-bit), hashes give half their output for collisions (SHA-256 ≈ 128-bit), and asymmetric sizes are much larger for the same level (3072-bit RSA or 256-bit ECC ≈ 128-bit). Two forces move these targets: **Moore's-law growth** in attacker compute (pushing minimums up over time) and the **quantum** threat (Grover halves symmetric security; Shor breaks RSA/ECC), which is reshaping long-term choices. The practical takeaway: pick primitives that meet IND-CCA/AEAD and key sizes that give at least 128-bit security with margin for the data's lifetime.

## Notions and levels

| Notion / level | Meaning | Implication |
|---|---|---|
| **IND-CPA** | Can't distinguish given chosen plaintexts | Needs randomized/nonce encryption (no ECB) |
| **IND-CCA** | Also safe against a decryption oracle | Needs integrity — use AEAD |
| **Security level (bits)** | Best attack ≈ 2^k work | Target ≥ 128-bit |
| **Symmetric sizing** | Key bits ≈ security bits | AES-128/256 |
| **Asymmetric sizing** | Much larger keys per level | 3072-bit RSA ≈ 256-bit ECC ≈ 128-bit |

Choosing a primitive means choosing a notion (aim for IND-CCA/AEAD) and a level (aim for ≥ 128-bit, with margin for how long the data must stay secret).

## Security relevance

- **Deterministic encryption fails IND-CPA**: equal plaintexts reveal equality; always randomize/nonce.
- **No integrity fails IND-CCA**: padding oracles and malleability come from CPA-only schemes; AEAD is the fix.
- **Under-sized keys age badly**: data that must stay secret for 20 years needs margin against future compute and quantum.
- **Quantum rebalancing**: Grover suggests doubling symmetric key sizes for long-term secrets; Shor forces post-quantum asymmetric migration.

## Practices

- **Target AEAD / IND-CCA** primitives (AES-GCM, ChaCha20-Poly1305) for confidentiality with integrity.
- **Use ≥ 128-bit security**: AES-128+ symmetric, SHA-256+ hashing, 3072-bit RSA or 256-bit ECC.
- **Match level to data lifetime**: longer secrecy needs more margin.
- **Plan post-quantum** for data that must remain confidential beyond the near term.

## Explanation

How do cryptographers decide a scheme is "secure" rather than just "unbroken so far"? They write down a game. In the game, an attacker is given specific superpowers — say, the ability to get any message they like encrypted — and then challenged to learn even one bit they shouldn't, like which of two messages was encrypted. If no realistic attacker can win, the scheme meets that notion. The weaker bar, IND-CPA, assumes the attacker can request encryptions; passing it just requires that encrypting the same thing twice looks different, which is why the naive "scramble every block identically" approach flunks. The stronger, realistic bar, IND-CCA, also lets the attacker feed in tampered ciphertexts and watch what happens — exactly what a real server does — and passing it requires a tamper-check, which is why authenticated encryption is the modern default. Alongside the "what can they do" question sits "how hard would brute force be," measured in bits: 128-bit security means roughly 2^128 tries, a number beyond any foreseeable machine. That's why key sizes differ across tools, and why two pressures — ever-cheaper computing and the eventual arrival of quantum machines — keep nudging the recommended sizes upward, especially for secrets that must survive for decades.
