---
type: Concept
title: CIA triad (confidentiality, integrity, availability)
description: The three core goals of information security - confidentiality (only authorised parties can read information), integrity (information is not altered improperly) and availability (information and systems are usable when needed).
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
- title: ISO/IEC (2018). ISO/IEC 27000 - Information security management systems - Overview and vocabulary
  resource: 'https://www.iso.org/standard/73906.html'
  kind: standard
- title: NIST (2004). FIPS 199 - Standards for Security Categorization of Federal Information and Information Systems
  resource: 'https://csrc.nist.gov/pubs/fips/199/final'
  kind: standard
- title: NIST (2017). SP 800-12 Rev. 1 - An Introduction to Information Security
  resource: 'https://csrc.nist.gov/pubs/sp/800/12/r1/final'
  kind: standard
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§1.1 Computer Security Concepts, pp. 12-19'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 1 Mastering Security Basics, p. 61'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# CIA triad (confidentiality, integrity, availability)

**Definition.** ISO/IEC 27000 defines information security as the preservation of confidentiality, integrity and availability of information. NIST FIPS 199 uses the same three objectives to rate how severe a loss would be for each information system (low, moderate, high impact). Some frameworks add properties such as authenticity, accountability and non-repudiation.

Domain: [security](/domains/security.md) · subdomain `security-foundations`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Every security control protects at least one of the three: encryption and access control protect confidentiality; hashes, signatures and change control protect integrity; redundancy, backups and DDoS protection protect availability. They can conflict: tighter confidentiality controls can hurt availability. Threat modelling asks, for each part of a system, which of the three an attacker could break.

## Example

A leaked customer database breaks confidentiality, a tampered invoice amount breaks integrity, and ransomware that locks the files breaks availability.

## Narrower concepts

- [Threat modeling](/concepts/security/threat-modeling.md)
- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)
- [Information flow control](/concepts/security/information-flow-control.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §1.1 Computer Security Concepts, pp. 12-19: CIA as the three key objectives, plus authenticity and accountability; levels of impact.
- **Gibson, D. (2017)**, *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*, ch. 1 Mastering Security Basics, p. 61: Exam-level framing of confidentiality, integrity and availability (SY0-501, 2017).

## Sources

1. ISO/IEC (2018). *ISO/IEC 27000 - Information security management systems - Overview and vocabulary*. <https://www.iso.org/standard/73906.html> — Information security as preserving confidentiality, integrity and availability.
2. NIST (2004). *FIPS 199 - Standards for Security Categorization of Federal Information and Information Systems*. <https://csrc.nist.gov/pubs/fips/199/final> — The three security objectives and impact levels.
3. NIST (2017). *SP 800-12 Rev. 1 - An Introduction to Information Security*. <https://csrc.nist.gov/pubs/sp/800/12/r1/final> — Introductory explanation of the security objectives and controls.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §1.1 Computer Security Concepts, pp. 12-19. CIA as the three key objectives, plus authenticity and accountability; levels of impact.
5. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 1 Mastering Security Basics, p. 61. Exam-level framing of confidentiality, integrity and availability (SY0-501, 2017).

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
