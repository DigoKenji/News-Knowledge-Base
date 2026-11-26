---
type: Concept
title: 'Economies of speed'
description: 'Competing on how fast an organisation can learn and deliver rather than on how cheaply it can produce at scale - cutting wait time, batch size and approval overhead instead of maximising resource utilisation.'
tags:
- software-engineering
- delivery
domain: software-engineering
subdomain: delivery
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 35 Economies of Speed; ch. 36 The Infinite Loop; ch. 39 Who Likes Standing in Line?'
- title: 'DORA (DevOps Research and Assessment). Research programme'
  resource: 'https://dora.dev/research/'
  kind: docs
- title: 'Reinertsen, D. G. (2009). The Principles of Product Development Flow. Celeritas'
  resource: 'https://books.google.com/books?vid=ISBN9781935401001'
  kind: book
---

# Economies of speed

**Definition.** Traditional enterprises pursue economies of scale: large batches, high utilisation, volume discounts. Hohpe contrasts this with economies of speed, where the goal is to shorten the time from idea to running software. His example: a company spent about seven months to standardise on a version control product without adopting it, while a start-up had a repository and first commit in about ten minutes. Speed comes less from working harder than from waiting less; queueing theory shows that pushing utilisation towards 100% makes wait times explode, so the delays are found between activities, not in them. Fast feedback loops (Build-Measure-Learn) then turn speed into learning.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `delivery`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

The idea is the organisational argument behind DevOps, CI/CD and cloud self-service, and it matches the DORA research, where deployment frequency and lead time for changes are key measures of delivery performance, and Reinertsen's product-development flow principles on queues and batch size.

## Example

A team waits three weeks for a test environment that takes one hour to build. Replacing the ticket with self-service infrastructure as code removes the wait without anyone working faster.

## Related concepts

- [DevOps](/concepts/software-engineering/devops.md)
- [Continuous integration and continuous delivery (CI/CD)](/concepts/software-engineering/ci-cd.md)
- [Infrastructure as code](/concepts/cloud-infra/infrastructure-as-code.md)

## Sources

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 35 Economies of Speed; ch. 36 The Infinite Loop; ch. 39 Who Likes Standing in Line?. Scale versus speed, the version-control example, Build-Measure-Learn, and queueing theory on why high utilisation slows organisations down.
2. DORA (DevOps Research and Assessment). Research programme. <https://dora.dev/research/> — DORA research programme: delivery performance measured by deployment frequency, lead time, change failure rate and recovery time.
3. Reinertsen, D. G. (2009). *The Principles of Product Development Flow*. Celeritas. <https://books.google.com/books?vid=ISBN9781935401001> — Product-development flow: the cost of queues, small batches and why utilisation is the wrong target.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
