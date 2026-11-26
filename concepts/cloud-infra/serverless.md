---
type: Concept
title: Serverless
description: A cloud model where developers deploy code or use services without provisioning or managing servers; the provider runs, scales (including to zero) and bills them per request or execution time. Functions as a service (FaaS) is the best-known form.
tags:
- cloud-infra
- cloud-platforms
- core
domain: cloud-infra
subdomain: cloud-platforms
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Jonas, E. et al. (2019). Cloud Programming Simplified - A Berkeley View on Serverless Computing. arXiv 1902.03383
  resource: 'https://arxiv.org/abs/1902.03383'
  kind: paper
- title: CNCF Serverless Working Group (2018). CNCF Serverless Whitepaper v1.0
  resource: 'https://github.com/cncf/wg-serverless/blob/master/whitepapers/serverless-overview/README.md'
  kind: standard
- title: Amazon Web Services. What is AWS Lambda? (Lambda Developer Guide)
  resource: 'https://docs.aws.amazon.com/lambda/latest/dg/welcome.html'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Serverless

**Definition.** Jonas et al. (Berkeley) define serverless computing by three traits: decoupled computation and storage, executing code without managing resource allocation, and paying in proportion to resources used instead of resources allocated. The CNCF Serverless Whitepaper distinguishes FaaS (event-triggered functions such as AWS Lambda) from backend as a service (managed databases, queues, authentication).

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `cloud-platforms`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Serverless is ideal for spiky or event-driven workloads and small teams: no idle cost, no patching. Trade-offs are cold-start latency, execution time limits, harder local testing, and cost that can exceed always-on servers at steady high load. This project's scheduled cloud tasks follow the same "pay per run" idea.

## Example

When a photo is uploaded to object storage, an event triggers a function that creates three thumbnails and exits after 400 ms. With no uploads overnight, nothing runs and nothing is billed; on a busy day, the provider runs thousands of copies in parallel.

## Narrower concepts

- [FinOps and cloud cost](/concepts/cloud-infra/finops.md)

## Sources

1. Jonas, E. et al. (2019). *Cloud Programming Simplified - A Berkeley View on Serverless Computing*. arXiv 1902.03383. <https://arxiv.org/abs/1902.03383> — Definition of serverless, its traits and open challenges.
2. CNCF Serverless Working Group (2018). *CNCF Serverless Whitepaper v1.0*. <https://github.com/cncf/wg-serverless/blob/master/whitepapers/serverless-overview/README.md> — FaaS vs BaaS and use cases, from the cloud-native foundation.
3. Amazon Web Services. *What is AWS Lambda?* (Lambda Developer Guide). <https://docs.aws.amazon.com/lambda/latest/dg/welcome.html> — Concrete FaaS behaviour - event triggers, scaling, billing.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
