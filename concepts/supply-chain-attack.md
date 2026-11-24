---
type: Concept
title: Supply-chain attack
description: An attack that compromises something a target depends on - a third-party library, package registry, build system, update channel, vendor or developer tool - so the malicious code or access reaches the target through a trusted path instead of a direct attack.
tags:
- security
domain: security
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: MITRE ATT&CK. T1195 Supply Chain Compromise
  resource: 'https://attack.mitre.org/techniques/T1195/'
  kind: standard
- title: NIST (2022). SP 800-161 Rev. 1 - Cybersecurity Supply Chain Risk Management Practices
  resource: 'https://csrc.nist.gov/pubs/sp/800/161/r1/final'
  kind: standard
- title: SLSA. Supply-chain Levels for Software Artifacts v1.0 - Threats
  resource: 'https://slsa.dev/spec/v1.0/threats'
  kind: standard
- title: 'Ohm, M. et al. (2020). Backstabber''s Knife Collection - A Review of Open Source Software Supply Chain Attacks. DIMVA 2020, arXiv:2005.09535'
  resource: 'https://arxiv.org/abs/2005.09535'
  kind: paper
---

# Supply-chain attack

**Definition.** A software supply-chain attack targets the process that produces and delivers software rather than the running software itself. One compromise upstream can reach thousands of downstream organisations, because they install and run what their suppliers give them. MITRE ATT&CK classifies it as technique T1195, Supply Chain Compromise.

Domain: [security](/domains/security.md). Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Common vectors: malicious packages (typosquatting such as "reqeusts", dependency confusion where a public package shadows an internal name), takeover of a maintainer's account or a long social-engineering campaign (xz-utils backdoor, 2024), compromised build systems that inject code into signed releases (SolarWinds Orion, 2020), poisoned update servers, and compromised CI actions or developer tools (including AI coding tools and MCP servers). Ohm et al. catalogued hundreds of real malicious packages in npm, PyPI and RubyGems. Defences: pin and verify dependencies (lockfiles, hashes), SBOMs, provenance and signing (SLSA levels, Sigstore), isolated reproducible builds, least-privilege CI tokens, and vendor risk management (NIST SP 800-161).

## Why it matters and trade-offs

Modern applications are mostly third-party code, so the supply chain is a large, shared attack surface with huge leverage for attackers. Defences add friction to updating dependencies and building software, and verifying every upstream is impossible, so organisations prioritise by criticality.

## Example

In 2020 attackers inserted a backdoor into the SolarWinds Orion build process; about 18,000 customers installed the signed update, including US government agencies.

## Review notes for Kenji

The draft limited this to "a dependency release"; it now covers build systems, update channels and vendors too, matching NIST and MITRE.

## Sources

1. MITRE ATT&CK. *T1195 Supply Chain Compromise*. <https://attack.mitre.org/techniques/T1195/> — Technique definition and sub-techniques (dependencies, software, hardware).
2. NIST (2022). *SP 800-161 Rev. 1 - Cybersecurity Supply Chain Risk Management Practices*. <https://csrc.nist.gov/pubs/sp/800/161/r1/final> — Supply-chain risk management for organisations.
3. SLSA. *Supply-chain Levels for Software Artifacts v1.0 - Threats*. <https://slsa.dev/spec/v1.0/threats> — Threat model from source to build to distribution; provenance levels.
4. Ohm, M. et al. (2020). *Backstabber's Knife Collection - A Review of Open Source Software Supply Chain Attacks*. DIMVA 2020, arXiv:2005.09535. <https://arxiv.org/abs/2005.09535> — Dataset and taxonomy of real malicious packages.

## Relations
- (none yet)

## Notes
- [Preflight: Agentic Last-Mile Verification for AI Software Supply Chains](/notes/security/2026-11-24-preflight-agentic-last-mile-verification-for-ai-software-sup.md)
