---
type: Concept
title: FinOps and cloud cost
description: An operational framework and cultural practice that brings engineering, finance and business together to manage variable cloud spending - making costs visible, allocating them to teams and optimising them for business value.
tags:
- cloud-infra
- core
domain: cloud-infra
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: FinOps Foundation. What is FinOps? and FinOps Framework
  resource: 'https://www.finops.org/introduction/what-is-finops/'
  kind: standard
- title: Storment, J. R. and Fuller, M. (2023). Cloud FinOps, 2nd edition. O'Reilly
  resource: 'https://www.oreilly.com/library/view/cloud-finops-2nd/9781492098348/'
  kind: book
- title: Amazon Web Services. Cost Optimization Pillar - AWS Well-Architected Framework
  resource: 'https://docs.aws.amazon.com/wellarchitected/latest/framework/cost-optimization.html'
  kind: docs
---

# FinOps and cloud cost

**Definition.** The FinOps Foundation (part of the Linux Foundation) defines FinOps as an operational framework and cultural practice which maximises the business value of cloud, enables timely data-driven decision making and creates financial accountability through collaboration between engineering, finance and business teams. Its life cycle has three phases - inform (visibility and allocation), optimise (rightsizing, commitments, waste removal) and operate (governance and continuous improvement).

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

In the cloud every engineer can spend money with an API call, so cost becomes an engineering metric alongside latency and reliability. Tagging, showback/chargeback, unit costs (cost per customer or per 1,000 LLM calls) and budgets are the basic tools; AI workloads and GPU pricing made FinOps more urgent. This project's US$20/month budget is a FinOps constraint.

## Example

A monthly cost report, split by team tags, shows the ML team's GPU spend doubled. Investigation finds idle notebook instances running all weekend; an auto-shutdown policy and a one-year commitment for the steady baseline cut the bill by 35%.

## Narrower concepts

- [AI gateway](/concepts/ai-ml/ai-gateway.md)
- [Well-Architected Framework](/concepts/cloud-infra/well-architected.md)

## Sources

1. FinOps Foundation. *What is FinOps?* and *FinOps Framework*. <https://www.finops.org/introduction/what-is-finops/> — Definition of FinOps, principles and the inform-optimise-operate life cycle.
2. Storment, J. R. and Fuller, M. (2023). *Cloud FinOps*, 2nd edition. O'Reilly. <https://www.oreilly.com/library/view/cloud-finops-2nd/9781492098348/> — Practitioner reference on cost allocation, optimisation and culture.
3. Amazon Web Services. *Cost Optimization Pillar - AWS Well-Architected Framework*. <https://docs.aws.amazon.com/wellarchitected/latest/framework/cost-optimization.html> — Provider guidance on cloud financial management practices.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
