---
type: Concept
title: Silent failure
description: A failure that produces no error - the system returns output that looks normal but is wrong, incomplete or stale - so monitoring based on errors and uptime does not see it and damage accumulates until someone notices the results.
tags:
- backend-systems
- reliability
domain: backend-systems
subdomain: reliability
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
- title: Huang, P. et al. (2017). Gray Failure - The Achilles' Heel of Cloud-Scale Systems. HotOS 2017
  resource: 'https://www.microsoft.com/en-us/research/publication/gray-failure-achilles-heel-cloud-scale-systems/'
  kind: paper
- title: 'Dixit, H. D. et al. (2021). Silent Data Corruptions at Scale. arXiv:2102.11245'
  resource: 'https://arxiv.org/abs/2102.11245'
  kind: paper
- title: Hochschild, P. et al. (2021). Cores that don't count. HotOS 2021
  resource: 'https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s01-hochschild.pdf'
  kind: paper
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 6 Monitoring Distributed Systems. O'Reilly / Google
  resource: 'https://sre.google/sre-book/monitoring-distributed-systems/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Silent failure

**Definition.** A silent failure is a fault whose symptoms do not trip any alarm: no exception, no 5xx, no crash. Examples range from hardware silent data corruption to a pipeline that quietly processes zero records, a cache serving stale data, or an ML model whose accuracy degrades while still answering confidently.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `reliability`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Related research ideas: gray failure (Huang et al., Microsoft), where a component is degraded in ways that its health checks do not detect but its users do; silent data corruption from faulty CPUs, documented at Meta and Google ("cores that don't count"), where arithmetic is occasionally wrong without any error signal. Detection depends on checking outputs, not just liveness: end-to-end probes with known answers, data-quality checks (row counts, freshness, distributions), checksums, reconciliation between systems, and SLOs on correctness. Google's SRE book recommends black-box monitoring of what users experience in addition to internal signals.

## Why it matters and trade-offs

Silent failures last longer and spread further than loud ones, because nothing triggers a response; corrupted data may be copied into backups and downstream systems. Correctness checks add cost and false alarms, so teams must pick the outputs that matter most.

## Example

After a source changes its feed format, the daily run keeps succeeding but selects zero articles from it for two weeks; only a "no items from source X in N days" check catches it.

## Sources

1. Huang, P. et al. (2017). *Gray Failure - The Achilles' Heel of Cloud-Scale Systems*. HotOS 2017. <https://www.microsoft.com/en-us/research/publication/gray-failure-achilles-heel-cloud-scale-systems/> — Failures that health checks miss but users experience (differential observability).
2. Dixit, H. D. et al. (2021). *Silent Data Corruptions at Scale*. arXiv:2102.11245. <https://arxiv.org/abs/2102.11245> — Hardware silent data corruption measured across Meta's fleet.
3. Hochschild, P. et al. (2021). *Cores that don't count*. HotOS 2021. <https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s01-hochschild.pdf> — Independent Google evidence of mercurial cores producing silent wrong results.
4. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 6 Monitoring Distributed Systems. O'Reilly / Google. <https://sre.google/sre-book/monitoring-distributed-systems/> — Black-box vs white-box monitoring; monitoring symptoms users see.

## Relations
- (none yet)

## Notes
- [Resilience testing your agents on Amazon Bedrock AgentCore](/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md)
