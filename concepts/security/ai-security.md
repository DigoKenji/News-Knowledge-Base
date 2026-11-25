---
type: Concept
title: AI security and adversarial machine learning
description: Protecting AI systems against attacks that target their data, models and behaviour - poisoning training data, evading or extracting models, prompt injection, data leakage - and the new risks created when AI systems are given access to tools and data.
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
- title: NIST (2025). AI 100-2 E2025 - Adversarial Machine Learning - A Taxonomy and Terminology of Attacks and Mitigations
  resource: 'https://csrc.nist.gov/pubs/ai/100/2/e2025/final'
  kind: standard
- title: OWASP. OWASP Top 10 for LLM Applications 2025
  resource: 'https://genai.owasp.org/llm-top-10/'
  kind: standard
- title: MITRE. ATLAS - Adversarial Threat Landscape for Artificial-Intelligence Systems
  resource: 'https://atlas.mitre.org/'
  kind: standard
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 5 Defensive Prompt Engineering, pp. 235-251 (jailbreaking and prompt injection p. 238; information extraction p. 243; defenses p. 248)'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# AI security and adversarial machine learning

**Definition.** NIST's adversarial machine learning taxonomy (AI 100-2) classifies attacks by the system's lifecycle stage and the attacker's goal: evasion (inputs crafted to cause wrong outputs), poisoning (corrupting training data or models), privacy attacks (extracting training data or the model itself), and, for generative AI, abuse such as prompt injection and jailbreaks. AI security also covers ordinary security of the surrounding system, since models run inside applications with data, credentials and tools.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

The OWASP Top 10 for LLM Applications and MITRE ATLAS (an ATT&CK-style knowledge base of real attacks on AI) are the main practitioner references. Agents raise the stakes: an LLM that can browse, read email and call tools turns a misleading answer into a harmful action, which is why least privilege, human approval and information flow control matter so much there.

## Example

Evasion - stickers on a stop sign make a vision model read it as a speed-limit sign. Prompt injection - hidden text in a web page makes a browsing agent leak the user's data.

## Narrower concepts

- [Indirect prompt injection](/concepts/security/indirect-prompt-injection.md)
- [Memory poisoning](/concepts/security/memory-poisoning.md)
- [Information flow control](/concepts/security/information-flow-control.md)
- [Model Context Protocol (MCP)](/concepts/ai-ml/model-context-protocol.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 5 Defensive Prompt Engineering, pp. 235-251 (jailbreaking and prompt injection p. 238; information extraction p. 243; defenses p. 248): Attacks on LLM applications and layered defences; ch. 10 guardrails (p. 451) add the system view.

## Sources

1. NIST (2025). *AI 100-2 E2025 - Adversarial Machine Learning - A Taxonomy and Terminology of Attacks and Mitigations*. <https://csrc.nist.gov/pubs/ai/100/2/e2025/final> — Taxonomy of attacks on predictive and generative AI.
2. OWASP. *OWASP Top 10 for LLM Applications 2025*. <https://genai.owasp.org/llm-top-10/> — Ranked risks for LLM applications.
3. MITRE. *ATLAS - Adversarial Threat Landscape for Artificial-Intelligence Systems*. <https://atlas.mitre.org/> — Knowledge base of real-world tactics and techniques against AI systems.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 5 Defensive Prompt Engineering, pp. 235-251 (jailbreaking and prompt injection p. 238; information extraction p. 243; defenses p. 248). Attacks on LLM applications and layered defences; ch. 10 guardrails (p. 451) add the system view.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
