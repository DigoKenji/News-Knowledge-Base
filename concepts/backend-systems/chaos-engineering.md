---
type: Concept
title: Chaos engineering
description: The practice of running controlled experiments that inject real faults into a system (often in production) to test a hypothesis about how it keeps working, so weaknesses are found before they cause outages.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Principles of Chaos Engineering (2019)
  resource: 'https://principlesofchaos.org/'
  kind: practitioner
- title: 'Basiri, A. et al. (2016). Chaos Engineering. IEEE Software 33(3), arXiv:1702.05843'
  resource: 'https://arxiv.org/abs/1702.05843'
  kind: paper
- title: AWS. What is AWS Fault Injection Service?
  resource: 'https://docs.aws.amazon.com/fis/latest/userguide/what-is.html'
  kind: docs
---

# Chaos engineering

**Definition.** Chaos engineering is "the discipline of experimenting on a system in order to build confidence in the system's capability to withstand turbulent conditions in production" (Principles of Chaos). It is an experiment, not random breakage: you define normal behaviour, predict it will hold under a fault, inject the fault with a limited blast radius and check whether you were right.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `system-design`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Steps: (1) define a steady state with business metrics (orders per minute, playback starts), not just CPU; (2) hypothesise it holds in both control and experimental groups; (3) inject realistic events (instance or zone loss, latency, dependency errors, clock skew); (4) look for a difference. Advanced principles: run in production, automate continuously, minimise blast radius. Netflix started with Chaos Monkey (randomly terminating instances, 2011) and later built ChAP for targeted experiments. Tools: AWS Fault Injection Service, Gremlin, LitmusChaos, Chaos Mesh. GameDays are the manual, scheduled form.

## Why it matters and trade-offs

It finds failure modes that tests and reviews miss, especially in distributed systems with emergent behaviour, and trains teams to respond. It needs good observability, a culture that accepts controlled risk, and the ability to abort experiments; without those, it is just causing incidents.

## Example

Hypothesis: "if the recommendations service adds 500 ms of latency for 1% of users, checkout success stays above 99.5%". Inject it, watch the metric, stop automatically if it drops.

## Sources

1. Principles of Chaos Engineering (2019). <https://principlesofchaos.org/> — Definition and the steady-state, hypothesis, real-world events, production and blast-radius principles.
2. Basiri, A. et al. (2016). *Chaos Engineering*. IEEE Software 33(3), arXiv:1702.05843. <https://arxiv.org/abs/1702.05843> — Peer-reviewed description of the discipline and Netflix's experiments.
3. AWS. *What is AWS Fault Injection Service?*. <https://docs.aws.amazon.com/fis/latest/userguide/what-is.html> — Independent managed tooling with stop conditions and blast-radius controls.

## Relations
- (none yet)

## Notes
- [Resilience testing your agents on Amazon Bedrock AgentCore](/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md)
