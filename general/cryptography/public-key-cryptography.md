---
title: "Public-Key Cryptography (Hard Problems, RSA, Diffie-Hellman, Elliptic Curves)"
domain: [General]
category: [Cryptography]
difficulty: Advanced
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-56Ar3.pdf
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-56Br2.pdf
  - https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-5.pdf
open_issues: []
---

# Public-Key Cryptography (Hard Problems, RSA, Diffie-Hellman, Elliptic Curves)

## Summary

**Public-key (asymmetric) cryptography** gives each party a key pair — a **public key** anyone may know and a **private key** kept secret — so that two people who have never shared a secret can still communicate securely. Its security rests on mathematical problems that are easy one way and infeasible to reverse: **RSA** relies on the difficulty of **factoring** large integers (and the RSA problem); **Diffie–Hellman (DH)** and **DSA** rely on the **discrete logarithm problem** in a finite field; **elliptic-curve** schemes (ECDH, ECDSA, EdDSA) rely on the discrete log problem over elliptic-curve groups, which is harder per bit, so they achieve equivalent security with much smaller keys (a 256-bit curve ≈ 3072-bit RSA). Public-key crypto delivers three capabilities: **key exchange/establishment** (DH/ECDH let two parties derive a shared secret over a public channel), **public-key encryption** (encrypt to a public key; only the private key decrypts — RSA-OAEP, or hybrid schemes), and **digital signatures** (sign with the private key; anyone verifies with the public key — RSA-PSS, ECDSA, Ed25519). In practice, asymmetric operations are slow, so they are used to set up or sign, and fast symmetric ciphers carry the bulk data (**hybrid encryption**). Two caveats dominate security: **padding/scheme choice** (textbook RSA is insecure; use OAEP/PSS), and the looming threat of **quantum computers**, which would break RSA/DH/ECC via Shor's algorithm, motivating post-quantum standards.

## Primitives and hard problems

| Scheme | Hard problem | Primary use |
|---|---|---|
| **RSA** | Integer factorization | Encryption (OAEP), signatures (PSS) |
| **Diffie–Hellman** | Discrete log (finite field) | Key exchange |
| **ECDH / ECDSA / Ed25519** | Elliptic-curve discrete log | Key exchange, signatures (small keys) |
| **Hybrid encryption** | — | Asymmetric to wrap a symmetric key |

Elliptic curves give the same security as RSA/DH at far smaller key sizes, which is why ECDH and Ed25519 dominate modern protocols.

## Security relevance

- **Textbook RSA is insecure**: deterministic, malleable; always use OAEP for encryption and PSS for signatures.
- **Ephemeral keys for forward secrecy**: ephemeral (EC)DH means compromising a long-term key does not decrypt past sessions.
- **Authenticate the key exchange**: raw DH is vulnerable to man-in-the-middle; bind it to signatures or certificates.
- **Quantum threat**: Shor's algorithm breaks factoring and discrete log; plan migration to post-quantum (ML-KEM/ML-DSA) for long-lived secrets.
- **Parameter validation**: invalid-curve and small-subgroup attacks exploit unvalidated public keys; validate received points.

## Practices

- **Use standard schemes**: RSA-OAEP/PSS, ECDH with named curves (P-256, X25519), Ed25519 for signatures.
- **Use ephemeral (EC)DH** (forward secrecy) and authenticate the exchange.
- **Choose adequate sizes**: RSA ≥ 3072-bit or ECC ≥ 256-bit for ~128-bit security.
- **Validate peer public keys** and use constant-time implementations.
- **Track post-quantum migration** for data that must stay secret for years.

## Explanation

Symmetric crypto has a chicken-and-egg problem: to talk secretly you first need a shared secret, but how do you share it secretly? Public-key cryptography breaks the loop by giving everyone two matched keys — one you publish to the world, one you guard. Anything scrambled with your public key only your private key can unscramble, and anything you sign with your private key anyone can check with your public key. The magic comes from math that is a one-way street: multiplying two huge primes is easy, but splitting the product back apart (factoring) is effectively impossible at the right size — that's RSA. Other schemes lean on a similar trapdoor called the discrete logarithm, and the elliptic-curve versions get the same strength from much smaller keys, which is why your phone's secure connections use them. These operations are slow, so the usual trick is "hybrid": use the fancy public-key math just to agree on a throwaway symmetric key, then let the fast symmetric cipher do the heavy lifting. Two warnings travel with all of this. First, never use the raw, "textbook" versions — the safe variants add essential padding. Second, a large enough quantum computer would crack factoring and discrete logs, so the world is already standardizing quantum-resistant replacements for anything that must stay secret for decades.
