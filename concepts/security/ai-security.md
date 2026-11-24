---
type: Concept
title: AI security and adversarial machine learning
description: Protecting AI systems against attacks that target their data, models and behaviour - poisoning training data, evading or extracting models, prompt injection, data leakage - and the new risks created when AI systems are given access to tools and data.
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
- title: NIST (2025). AI 100-2 E2025 - Adversarial Machine Learning - A Taxonomy and Terminology of Attacks and Mitigations
  resource: 'https://csrc.nist.gov/pubs/ai/100/2/e2025/final'
  kind: standard
- title: OWASP. OWASP Top 10 for LLM Applications 2025
  resource: 'https://genai.owasp.org/llm-top-10/'
  kind: standard
- title: MITRE. ATLAS - Adversarial Threat Landscape for Artificial-Intelligence Systems
  resource: 'https://atlas.mitre.org/'
  kind: standard
foundation_candidate: true
---

# AI security and adversarial machine learning

**Definition.** NIST's adversarial machine learning taxonomy (AI 100-2) classifies attacks by the system's lifecycle stage and the attacker's goal: evasion (inputs crafted to cause wrong outputs), poisoning (corrupting training data or models), privacy attacks (extracting training data or the model itself), and, for generative AI, abuse such as prompt injection and jailbreaks. AI security also covers ordinary security of the surrounding system, since models run inside applications with data, credentials and tools.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

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

## Sources

1. NIST (2025). *AI 100-2 E2025 - Adversarial Machine Learning - A Taxonomy and Terminology of Attacks and Mitigations*. <https://csrc.nist.gov/pubs/ai/100/2/e2025/final> — Taxonomy of attacks on predictive and generative AI.
2. OWASP. *OWASP Top 10 for LLM Applications 2025*. <https://genai.owasp.org/llm-top-10/> — Ranked risks for LLM applications.
3. MITRE. *ATLAS - Adversarial Threat Landscape for Artificial-Intelligence Systems*. <https://atlas.mitre.org/> — Knowledge base of real-world tactics and techniques against AI systems.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
