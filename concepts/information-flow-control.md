---
type: Concept
title: Information flow control
description: A security approach that labels data by its confidentiality or integrity (for example "secret" or "untrusted") and tracks those labels as data moves through a program, blocking flows that would violate a policy - such as untrusted input deciding a privileged action.
tags:
- security
- appsec
domain: security
subdomain: appsec
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Denning, D. E. (1976). A Lattice Model of Secure Information Flow. Communications of the ACM 19(5)
  resource: 'https://dl.acm.org/doi/10.1145/360051.360056'
  kind: paper
- title: Sabelfeld, A. and Myers, A. C. (2003). Language-Based Information-Flow Security. IEEE JSAC 21(1)
  resource: 'https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf'
  kind: paper
- title: 'Debenedetti, E. et al. (2025). Defeating Prompt Injections by Design (CaMeL). arXiv:2503.18813'
  resource: 'https://arxiv.org/abs/2503.18813'
  kind: paper
- title: 'Costa, M. et al. (2025). Securing AI Agents with Information-Flow Control (FIDES). arXiv:2505.23643'
  resource: 'https://arxiv.org/abs/2505.23643'
  kind: paper
relations:
- type: mitigates
  target: indirect-prompt-injection
---

# Information flow control

**Definition.** Access control asks "may this subject read this object?". Information flow control (IFC) asks the follow-up question "where can the information go after it is read?". Each value carries a label, labels combine when values are combined, and the system enforces rules such as "secret data may not reach a public output" (confidentiality) or "untrusted data may not influence a trusted decision" (integrity).

Domain: [security](/domains/security.md) · subdomain `appsec`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Denning (1976) modelled labels as a lattice; Sabelfeld and Myers surveyed language-based IFC, where type systems prove at compile time that a program has no illegal flows (noninterference), handling both explicit flows (assignments) and implicit ones (branching on a secret). Dynamic IFC, or taint tracking, tags values at runtime. For LLM agents, IFC is a principled defence against prompt injection: data from emails or web pages is labelled untrusted, and the harness ensures it can be passed as an argument but never chooses which tool runs or where data is sent. Google DeepMind's CaMeL and Microsoft's FIDES apply this to agents.

## Why it matters and trade-offs

IFC gives guarantees that filters and prompts cannot: a policy holds regardless of what the injected text says. It is restrictive (legitimate tasks may need declassification), labels spread and can over-taint everything ("label creep"), and it requires redesigning how the agent plans and executes.

## Example

An assistant reads an untrusted email asking it to forward the user's contacts. Under IFC, the email body is labelled untrusted, so it cannot set the recipient of a send-email call; the action is blocked or sent to the user for approval.

## Sources

1. Denning, D. E. (1976). *A Lattice Model of Secure Information Flow*. Communications of the ACM 19(5). <https://dl.acm.org/doi/10.1145/360051.360056> — Original lattice model of information flow.
2. Sabelfeld, A. and Myers, A. C. (2003). *Language-Based Information-Flow Security*. IEEE JSAC 21(1). <https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf> — Noninterference, explicit and implicit flows, static enforcement.
3. Debenedetti, E. et al. (2025). *Defeating Prompt Injections by Design (CaMeL)*. arXiv:2503.18813. <https://arxiv.org/abs/2503.18813> — IFC-style capabilities applied to LLM agents.
4. Costa, M. et al. (2025). *Securing AI Agents with Information-Flow Control (FIDES)*. arXiv:2505.23643. <https://arxiv.org/abs/2505.23643> — Independent (Microsoft) IFC planner for agents with confidentiality and integrity labels.

## Relations
- **mitigates** [indirect-prompt-injection](/concepts/indirect-prompt-injection.md)

## Notes
- [Kaer: Security-Aware Replanning for Multi-Agent Systems](/notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md)
