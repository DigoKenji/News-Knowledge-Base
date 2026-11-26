---
type: Concept
title: Encryption
description: Transforming data with a key so that only holders of the right key can read it - symmetric encryption uses one shared key, asymmetric (public-key) encryption uses a key pair - used to protect data at rest and in transit.
tags:
- security
- security-foundations
- core
domain: security
subdomain: security-foundations
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
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 2 Cryptographic Tools, pp. 40-71; ch. 20 Symmetric Encryption, p. 637'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 2 Data Asset Management and Protection, pp. 13-28'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Encryption

**Definition.** Encryption converts plaintext into ciphertext with an algorithm and a key; decryption reverses it. Symmetric algorithms such as AES (FIPS 197) are fast and use the same secret key on both sides. Asymmetric algorithms (RSA, elliptic-curve), introduced by Diffie and Hellman in 1976, use a public key that anyone may know and a private key kept secret, which solves key exchange and enables digital signatures.

Domain: [security](/domains/security.md) · subdomain `security-foundations`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Real systems combine both: TLS uses public-key cryptography to authenticate the server and agree on a session key, then symmetric encryption for the data (in transit). Disk and database encryption protect data at rest. The hard part is usually key management (generation, storage, rotation, access), not the algorithm. Hashing is related but different: it is one-way and has no key to reverse it.

## Example

When you open https://github.com, TLS 1.3 verifies GitHub's certificate with public-key cryptography and then encrypts the session with AES or ChaCha20.

## Narrower concepts

- [Remote attestation](/concepts/security/remote-attestation.md)
- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, ch. 2 Cryptographic Tools, pp. 40-71; ch. 20 Symmetric Encryption, p. 637: Symmetric and public-key encryption, hashes, MACs, signatures and key management.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 2 Data Asset Management and Protection, pp. 13-28: Encryption at rest and in transit and key management in the cloud.

## Sources

1. NIST (2020). *SP 800-175B Rev. 1 - Guideline for Using Cryptographic Standards in the Federal Government*. <https://csrc.nist.gov/pubs/sp/800/175/b/r1/final> — Symmetric and asymmetric algorithms, key management, approved uses.
2. NIST (2001, updated 2023). *FIPS 197 - Advanced Encryption Standard (AES)*. <https://csrc.nist.gov/pubs/fips/197/final> — The standard symmetric cipher.
3. Rescorla, E. (2018). *RFC 8446 - The Transport Layer Security (TLS) Protocol Version 1.3*. IETF. <https://www.rfc-editor.org/rfc/rfc8446> — How public-key and symmetric encryption combine to protect data in transit.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 2 Cryptographic Tools, pp. 40-71; ch. 20 Symmetric Encryption, p. 637. Symmetric and public-key encryption, hashes, MACs, signatures and key management.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 2 Data Asset Management and Protection, pp. 13-28. Encryption at rest and in transit and key management in the cloud.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
