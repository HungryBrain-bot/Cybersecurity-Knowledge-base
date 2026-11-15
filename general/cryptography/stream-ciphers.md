---
title: "Stream Ciphers"
domain: [General]
category: [Cryptography]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://datatracker.ietf.org/doc/html/rfc8439
  - https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38a.pdf
open_issues: []
---

# Stream Ciphers

## Summary

A **stream cipher** encrypts data by generating a pseudorandom **keystream** from a key (and usually a nonce) and combining it with the plaintext, almost always by XOR. It approximates the one-time pad — which is information-theoretically secure but needs a truly random key as long as the message — by replacing the random pad with a keystream a pseudorandom generator produces from a short key. The defining security requirement follows directly: **the keystream must never be reused**. If two messages are XORed with the same keystream, XORing the two ciphertexts cancels the keystream and leaks the XOR of the plaintexts, which is often enough to recover both. That is why stream ciphers take a **nonce**: the same key is safe across many messages only if each message uses a fresh nonce so the keystream differs every time. Modern practice favors **ChaCha20** (RFC 8439), a fast, constant-time design, usually paired with the Poly1305 MAC as the AEAD construction ChaCha20-Poly1305; older designs like RC4 are broken and deprecated. Block ciphers in **CTR mode** are effectively stream ciphers too. Like all confidentiality-only primitives, a raw stream cipher provides no integrity — flipping a ciphertext bit flips the corresponding plaintext bit — so it must be combined with authentication.

## Properties

| Aspect | Detail |
|---|---|
| **Core operation** | Keystream ⊕ plaintext (XOR) |
| **Keystream source** | PRG from key + nonce (e.g. ChaCha20) |
| **Hard rule** | Never reuse keystream — unique nonce per key |
| **Reuse failure** | C1 ⊕ C2 = P1 ⊕ P2, leaks plaintext |
| **Integrity** | None alone — malleable; pair with a MAC (Poly1305) |
| **Status** | ChaCha20 preferred; RC4 broken/deprecated |

The single most important fact is nonce uniqueness: a stream cipher with a repeated keystream collapses to trivially broken, regardless of key strength.

## Security relevance

- **Keystream reuse is fatal**: identical key+nonce across messages exposes plaintext XORs and often full recovery.
- **No built-in integrity**: an attacker who knows plaintext positions can flip targeted bits; always authenticate (ChaCha20-Poly1305).
- **Deprecated designs**: RC4 has exploitable biases and must not be used.
- **Constant-time matters**: ChaCha20 avoids table lookups that leak timing, reducing side-channel risk.

## Practices

- **Use ChaCha20-Poly1305 or AES-GCM**, not a bare stream cipher.
- **Guarantee nonce uniqueness** per key (random 96-bit nonce, or a strict counter).
- **Never use RC4**; retire legacy protocols that require it.
- **Authenticate every message**; reject on tag-verification failure before processing.

## Explanation

A stream cipher is the practical cousin of the "perfect" cipher, the one-time pad. The one-time pad is unbreakable but useless in practice because it needs a truly random secret as long as the message, used only once. A stream cipher cheats cleverly: instead of a giant random pad, it uses a small key to spin out a long, random-looking stream of bits, and then just XORs that stream onto your data. Decrypting is the same operation in reverse. The whole scheme lives or dies on one rule: never spin out the same stream twice. If you encrypt two different messages with the same stream, an eavesdropper can cancel the stream out by combining the two scrambled messages, and the secrets start falling out. That is why these ciphers demand a fresh "nonce" — a number used once — for every message under a given key. And because XOR is so simple, an attacker who guesses where a word sits can flip it without knowing the key, so a stream cipher on its own is never enough; you bolt on a tamper-check (a MAC). The modern default, ChaCha20 with Poly1305, does exactly this: fast scrambling plus a built-in seal that screams if anything was altered.
