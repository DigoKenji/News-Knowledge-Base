---
type: Concept
title: Secure software development (SSDLC)
description: Building security into every phase of the software development life cycle - requirements, design, coding, testing, release and maintenance - instead of testing for it only at the end.
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
- title: NIST (2022). SP 800-218 - Secure Software Development Framework (SSDF) Version 1.1
  resource: 'https://csrc.nist.gov/pubs/sp/800/218/final'
  kind: standard
- title: OWASP. Software Assurance Maturity Model (SAMM)
  resource: 'https://owaspsamm.org/'
  kind: standard
- title: Microsoft. Security Development Lifecycle (SDL)
  resource: 'https://www.microsoft.com/en-us/securityengineering/sdl'
  kind: practitioner
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 11 Software Security, pp. 375-415 (handling program input p. 380; writing safe code p. 392)'
- title: 'Stuttard, D. and Pinto, M. (2008). The Web Application Hacker''s Handbook. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9780470170779'
  kind: book
  locator: 'ch. 2 Core Defense Mechanisms, pp. 15-34'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 12 Software Development Security'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Secure software development (SSDLC)

**Definition.** A secure software development life cycle adds security practices to each stage: security requirements, threat modelling during design, secure coding standards, code review, static and dynamic analysis (SAST, DAST), dependency scanning, secure build and release, and vulnerability response after release. NIST's Secure Software Development Framework (SSDF, SP 800-218) groups these into four practice areas: prepare the organisation, protect the software, produce well-secured software, and respond to vulnerabilities.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Fixing a flaw in design costs far less than fixing it in production, which is the case for "shifting left". Microsoft's SDL (2004) popularised the approach; OWASP SAMM is an open maturity model for it. With AI writing more code, SSDLC practices such as automated scanning and review gates matter more, because generated code can carry vulnerabilities at scale.

## Example

A team threat-models a new payment feature at design time, runs SAST and dependency scans on every pull request, and has a documented process to patch and disclose vulnerabilities reported after launch.

## Narrower concepts

- [Threat modeling](/concepts/security/threat-modeling.md)
- [Supply-chain attack](/concepts/security/supply-chain-attack.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, ch. 11 Software Security, pp. 375-415 (handling program input p. 380; writing safe code p. 392): Defensive programming: input handling, safe code, OS interaction and output handling.
- **Stuttard, D. and Pinto, M. (2008)**, *The Web Application Hacker's Handbook*, ch. 2 Core Defense Mechanisms, pp. 15-34: Handling user access and input, handling attackers, and managing the application.
- **Chapple, M. and Seidl, D. (2017)**, *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*, ch. 12 Software Development Security: SDLC security activities and testing.

## Sources

1. NIST (2022). *SP 800-218 - Secure Software Development Framework (SSDF) Version 1.1*. <https://csrc.nist.gov/pubs/sp/800/218/final> — The four practice groups and individual practices.
2. OWASP. *Software Assurance Maturity Model (SAMM)*. <https://owaspsamm.org/> — Open maturity model for secure development.
3. Microsoft. *Security Development Lifecycle (SDL)*. <https://www.microsoft.com/en-us/securityengineering/sdl> — The industry process that popularised SSDLC.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 11 Software Security, pp. 375-415 (handling program input p. 380; writing safe code p. 392). Defensive programming: input handling, safe code, OS interaction and output handling.
5. Stuttard, D. and Pinto, M. (2008). *The Web Application Hacker's Handbook*. Wiley. <https://books.google.com/books?vid=ISBN9780470170779> — ch. 2 Core Defense Mechanisms, pp. 15-34. Handling user access and input, handling attackers, and managing the application.
6. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 12 Software Development Security. SDLC security activities and testing.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
