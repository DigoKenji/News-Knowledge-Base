---
type: Concept
title: Encryption
description: Transforming data with a key so that only holders of the right key can read it - symmetric encryption uses one shared key, asymmetric (public-key) encryption uses a key pair - used to protect data at rest and in transit.
tags:
- security
- core
domain: security
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2020). SP 800-175B Rev. 1 - Guideline for Using Cryptographic Standards in the Federal Government
  resource: 'https://csrc.nist.gov/pubs/sp/800/175/b/r1/final'
  kind: standard
- title: NIST (2001, updated 2023). FIPS 197 - Advanced Encryption Standard (AES)
  resource: 'https://csrc.nist.gov/pubs/fips/197/final'
  kind: standard
- title: Rescorla, E. (2018). RFC 8446 - The Transport Layer Security (TLS) Protocol Version 1.3. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc8446'
  kind: standard
foundation_candidate: true
---

# Encryption

**Definition.** Encryption converts plaintext into ciphertext with an algorithm and a key; decryption reverses it. Symmetric algorithms such as AES (FIPS 197) are fast and use the same secret key on both sides. Asymmetric algorithms (RSA, elliptic-curve), introduced by Diffie and Hellman in 1976, use a public key that anyone may know and a private key kept secret, which solves key exchange and enables digital signatures.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Real systems combine both: TLS uses public-key cryptography to authenticate the server and agree on a session key, then symmetric encryption for the data (in transit). Disk and database encryption protect data at rest. The hard part is usually key management (generation, storage, rotation, access), not the algorithm. Hashing is related but different: it is one-way and has no key to reverse it.

## Example

When you open https://github.com, TLS 1.3 verifies GitHub's certificate with public-key cryptography and then encrypts the session with AES or ChaCha20.

## Narrower concepts

- [Remote attestation](/concepts/security/remote-attestation.md)
- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. NIST (2020). *SP 800-175B Rev. 1 - Guideline for Using Cryptographic Standards in the Federal Government*. <https://csrc.nist.gov/pubs/sp/800/175/b/r1/final> — Symmetric and asymmetric algorithms, key management, approved uses.
2. NIST (2001, updated 2023). *FIPS 197 - Advanced Encryption Standard (AES)*. <https://csrc.nist.gov/pubs/fips/197/final> — The standard symmetric cipher.
3. Rescorla, E. (2018). *RFC 8446 - The Transport Layer Security (TLS) Protocol Version 1.3*. IETF. <https://www.rfc-editor.org/rfc/rfc8446> — How public-key and symmetric encryption combine to protect data in transit.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
