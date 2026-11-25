---
type: Concept
title: Least privilege and defense in depth
description: Two basic design principles - give every user, service and program only the access it needs for its task (least privilege), and protect assets with several independent layers of controls so one failure does not expose everything (defense in depth).
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
- title: Saltzer, J. H. and Schroeder, M. D. (1975). The Protection of Information in Computer Systems. Proceedings of the IEEE 63(9)
  resource: 'https://web.mit.edu/Saltzer/www/publications/protection/'
  kind: paper
- title: NIST (2020). SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations (AC-6 Least Privilege)
  resource: 'https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final'
  kind: standard
- title: NIST CSRC. Glossary - defense-in-depth
  resource: 'https://csrc.nist.gov/glossary/term/defense_in_depth'
  kind: standard
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§1.4 Fundamental Security Design Principles, pp. 27-31'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 1 Least Privilege and Defense in Depth, pp. 1-6'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 10 Defense-in-Depth Security Architectures'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Least privilege and defense in depth

**Definition.** Saltzer and Schroeder (1975) listed least privilege among their design principles for protection: every program and user should operate with the least set of privileges necessary to complete the job. NIST SP 800-53 turns it into control AC-6. Defense in depth, a term from military strategy, means layering people, technology and operations controls so an attacker must defeat several of them.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Both principles limit blast radius: when (not if) something is compromised, the damage stays small. They underlie zero trust architecture and are especially important for AI agents, which should get only the tools and permissions a task needs. The cost is friction: fine-grained permissions take work to define and maintain, and too many layers slow people down.

## Example

The daily-run task can push only to its own branch, not to main (least privilege); and even if it were tricked, the validator, the pull request review and Kenji's approval are further layers (defense in depth).

## Narrower concepts

- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)
- [Information flow control](/concepts/security/information-flow-control.md)
- [Threat modeling](/concepts/security/threat-modeling.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §1.4 Fundamental Security Design Principles, pp. 27-31: Least privilege, separation of privilege, fail-safe defaults, complete mediation, layering.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 1 Least Privilege and Defense in Depth, pp. 1-6: The two principles applied to cloud environments.
- **Chapple, M. and Seidl, D. (2017)**, *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*, ch. 10 Defense-in-Depth Security Architectures: Layered security architecture from an analyst's view.

## Sources

1. Saltzer, J. H. and Schroeder, M. D. (1975). *The Protection of Information in Computer Systems*. Proceedings of the IEEE 63(9). <https://web.mit.edu/Saltzer/www/publications/protection/> — Least privilege and the other classic security design principles.
2. NIST (2020). *SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations* (AC-6 Least Privilege). <https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final> — Least privilege as a formal control.
3. NIST CSRC. *Glossary - defense-in-depth*. <https://csrc.nist.gov/glossary/term/defense_in_depth> — Definition of defense in depth.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §1.4 Fundamental Security Design Principles, pp. 27-31. Least privilege, separation of privilege, fail-safe defaults, complete mediation, layering.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 1 Least Privilege and Defense in Depth, pp. 1-6. The two principles applied to cloud environments.
6. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 10 Defense-in-Depth Security Architectures. Layered security architecture from an analyst's view.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
