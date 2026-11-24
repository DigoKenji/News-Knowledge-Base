---
type: Concept
title: Attack surface
description: The sum of all points where an attacker could try to enter, extract data from or affect a system - interfaces, inputs, services, dependencies, people - so reducing it is one of the most effective security measures.
tags:
- security
- core
domain: security
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: Manadhata, P. K. and Wing, J. M. (2011). An Attack Surface Metric. IEEE Transactions on Software Engineering 37(3)
  resource: 'https://doi.org/10.1109/TSE.2010.60'
  kind: paper
- title: NIST CSRC. Glossary - attack surface
  resource: 'https://csrc.nist.gov/glossary/term/attack_surface'
  kind: standard
- title: OWASP. Attack Surface Analysis Cheat Sheet
  resource: 'https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html'
  kind: practitioner
---

# Attack surface

**Definition.** Manadhata and Wing formalised a system's attack surface as the set of entry and exit points, channels and untrusted data items an attacker can use. NIST defines it as the set of points on the boundary of a system where an attacker can try to enter, cause an effect on, or extract data from it. Larger surfaces mean more places to defend.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Attack surface reduction means removing what is not needed: closing ports, disabling unused features, deleting old accounts and API keys, trimming dependencies. Attack surface analysis is part of threat modelling and secure design. New technology often grows the surface quietly: every MCP server, plugin or AI tool an agent can call, and every document it reads, is a new entry point.

## Example

A web app exposes its public site, an admin panel, a REST API, a file-upload form and 300 npm dependencies; each is part of its attack surface, and moving the admin panel behind single sign-on removes one large piece.

## Narrower concepts

- [Supply-chain attack](/concepts/supply-chain-attack.md)
- [Model Context Protocol (MCP)](/concepts/model-context-protocol.md)
- [Indirect prompt injection](/concepts/indirect-prompt-injection.md)

## Sources

1. Manadhata, P. K. and Wing, J. M. (2011). *An Attack Surface Metric*. IEEE Transactions on Software Engineering 37(3). <https://doi.org/10.1109/TSE.2010.60> — Formal definition and measurement of attack surface.
2. NIST CSRC. *Glossary - attack surface*. <https://csrc.nist.gov/glossary/term/attack_surface> — Standard definition.
3. OWASP. *Attack Surface Analysis Cheat Sheet*. <https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html> — How to map and reduce an application's attack surface.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
