---
title: "Randomness, PRNGs and Entropy"
domain: [General]
category: [Cryptography]
difficulty: Intermediate
verification: Verified
attack_ids: []
checked_against:
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf
  - https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90B.pdf
  - https://man7.org/linux/man-pages/man2/getrandom.2.html
open_issues: []
---

# Randomness, PRNGs and Entropy

## Summary

Nearly every cryptographic operation needs unpredictable values: keys, IVs/nonces, salts, DH secrets, and signature nonces. **Entropy** is the measure of true unpredictability, and it comes from physical noise sources (hardware RNGs, timing jitter, interrupts). Because raw entropy is scarce and slow, systems use a **Cryptographically Secure Pseudo-Random Number Generator (CSPRNG)**: a deterministic algorithm **seeded** with enough entropy that then stretches it into an effectively unlimited stream of unpredictable bits (NIST SP 800-90A defines approved DRBGs). The crucial distinction is between a **statistical PRNG** (like C's `rand()` or a Mersenne Twister) — fast, uniform, but **predictable** from its output, and therefore fatal for security — and a **CSPRNG**, which is designed so that observing output never lets you predict past or future output. The right way to get randomness is to ask the operating system (`getrandom()`/`/dev/urandom` on Linux, `BCryptGenRandom` on Windows, `SecRandomCopyBytes` on macOS), which maintain a well-seeded CSPRNG. Classic failures are **predictable or insufficient seeding** (e.g. seeding from the time, or generating keys at boot before entropy is collected), **reusing or leaking signature nonces** (a repeated ECDSA/DSA nonce reveals the private key), and **using a non-cryptographic PRNG** for tokens or keys. The rule of thumb: never roll your own randomness — use the OS CSPRNG, and ensure it is seeded before generating secrets.

## Randomness concepts

| Concept | Meaning |
|---|---|
| **Entropy** | True unpredictability from physical noise |
| **CSPRNG / DRBG** | Seeded deterministic generator, unpredictable output |
| **Statistical PRNG** | Uniform but predictable — **never for crypto** |
| **Seeding** | Mixing in enough entropy before use |
| **OS source** | `getrandom`, `/dev/urandom`, `BCryptGenRandom` |

Predictable randomness silently breaks otherwise-strong crypto: the algorithm is fine, but a guessable key or nonce hands the attacker everything.

## Security relevance

- **Predictable seeds = predictable keys**: seeding from time or PID lets attackers regenerate "secret" keys.
- **Nonce reuse reveals keys**: a repeated DSA/ECDSA signature nonce leaks the private key (the PlayStation 3 / Bitcoin wallet class of bugs).
- **Statistical PRNGs are reversible**: Mersenne Twister state can be recovered from output; never use it for tokens, salts, or keys.
- **Boot-time entropy starvation**: embedded devices generating keys at first boot often share low-entropy keys across units.

## Practices

- **Use the OS CSPRNG** (`getrandom`, `BCryptGenRandom`, `SecRandomCopyBytes`); do not implement your own.
- **Block or wait until seeded** before generating long-term keys, especially on embedded/VM first boot.
- **Generate a fresh, unique value** for every IV, nonce, and signature nonce (or use deterministic nonces per RFC 6979 for ECDSA).
- **Prefer hardware entropy sources** where available and feed them into the OS pool, not directly into keys.

## Explanation

Cryptography leans on secrets no one can guess, and "can't be guessed" is exactly what randomness provides — so bad randomness quietly wrecks good crypto. The raw material is entropy: genuine unpredictability harvested from messy physical events like the exact timing of keystrokes, disk interrupts, or dedicated noise circuits. But true entropy trickles in slowly, so computers use a clever amplifier: seed a special generator with a solid dose of entropy, and it spins out an endless stream of bits that look random and, crucially, can't be predicted even by someone watching the output. That last property is the whole game, and it's where a common disaster hides. The everyday "random" function in most programming languages is built for speed and statistics, not secrecy — watch a little of its output and you can predict the rest. Use it to make a password-reset token or an encryption key and you've handed attackers a skeleton key. The safe move is boring and correct: ask the operating system for randomness, because it carefully gathers entropy and runs a proper cryptographic generator. And a few things must be fresh every single time — the little one-shot values behind signatures especially, because reusing one has repeatedly spilled private keys into the open.
