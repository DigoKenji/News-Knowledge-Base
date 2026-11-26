---
type: Concept
title: Threat modeling
description: 'Systematically working out what can go wrong in a system - modelling its data flows and trust boundaries, listing threats (for example with STRIDE) and deciding mitigations.'
tags:
- security
- appsec
domain: security
subdomain: appsec
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§1.5 Attack Trees, pp. 32-34'
- title: 'Course handout (n.d.). Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)'
  resource: 'FIAP-MBA/Materiais/Referencias (local copy, no public URL)'
  kind: handout
  locator: 'p. 3 (threat modelling and attack trees in gap analysis)'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; short definition added'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
---

# Threat modeling

**Definition.** Threat modelling asks systematically what can go wrong in a system: model it (data flows, trust boundaries), enumerate threats (for example with STRIDE or attack trees) and decide mitigations. An attack tree, as Stallings and Brown present it, breaks an attacker's goal into alternative sub-goals and steps, showing which paths are cheapest to block.

Domain: [security](/domains/security.md) · subdomain `appsec`.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §1.5 Attack Trees, pp. 32-34: Attack trees as a structured way to enumerate how a goal can be attacked.
- **Course handout (n.d.)**, *Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)*, p. 3 (threat modelling and attack trees in gap analysis): Uses threat models to decide whether each control gap is accepted or mitigated.

## Sources

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §1.5 Attack Trees, pp. 32-34. Attack trees as a structured way to enumerate how a goal can be attacked.
2. Course handout (n.d.). *Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)*. — p. 3 (threat modelling and attack trees in gap analysis). Uses threat models to decide whether each control gap is accepted or mitigated.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
