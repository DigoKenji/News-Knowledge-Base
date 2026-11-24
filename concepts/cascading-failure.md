---
type: Concept
title: Cascading failure
description: A failure that spreads because the reaction to one component failing - extra load, retries, failover traffic - overloads other components, which then fail and pass their load on in turn.
tags:
- backend-systems
domain: backend-systems
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 22 Addressing Cascading Failures. O'Reilly / Google
  resource: 'https://sre.google/sre-book/addressing-cascading-failures/'
  kind: book
- title: Huang, L. et al. (2022). Metastable Failures in the Wild. OSDI 2022
  resource: 'https://www.usenix.org/conference/osdi22/presentation/huang-lexiang'
  kind: paper
- title: Amazon Builders' Library (AWS Builder Center). Using load shedding to avoid overload
  resource: 'https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload'
  kind: practitioner
---

# Cascading failure

**Definition.** A cascading failure grows through positive feedback: the failure of part of a system increases the probability that other parts fail. The classic trigger is overload, where one replica dies, its traffic moves to the others, they exceed capacity and die too, until the whole tier is down.

Domain: [backend-systems](/domains/backend-systems.md). Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Common mechanisms (Google SRE book): server overload and the resulting latency; resource exhaustion (CPU, memory, threads, file descriptors) that makes servers slower or crash; retries that multiply load (see retry storm); health checks that kill slow but working instances, removing more capacity; and slow startup or cold caches that make recovered instances fail again. Some systems enter a metastable state where the overload sustains itself even after the trigger is gone. Defences: load shedding and adaptive concurrency limits, retry budgets with backoff and jitter, circuit breakers, deadline propagation, capacity headroom (N+2), and degrading features gracefully.

## Why it matters and trade-offs

Cascades turn small, local problems into total outages, and they are behind many of the biggest public incidents. Defences cost capacity (headroom), complexity, and sometimes deliberately failing requests early (shedding) to save the rest.

## Example

One of three API servers crashes; the other two take 50% more traffic, slow down, fail health checks and are restarted; clients retry, and the cold restarted servers collapse under the backlog.

## Sources

1. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 22 Addressing Cascading Failures. O'Reilly / Google. <https://sre.google/sre-book/addressing-cascading-failures/> — Definition, causes and defences.
2. Huang, L. et al. (2022). *Metastable Failures in the Wild*. OSDI 2022. <https://www.usenix.org/conference/osdi22/presentation/huang-lexiang> — Self-sustaining overload after the trigger is removed, with real incident data.
3. Amazon Builders' Library (AWS Builder Center). *Using load shedding to avoid overload*. <https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload> — Load shedding as a defence; goodput collapse under overload.

## Relations
- (none yet)

## Notes
- [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md)
