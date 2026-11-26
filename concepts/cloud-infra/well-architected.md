---
type: Concept
title: Well-Architected Framework
description: 'Cloud providers'' frameworks (AWS, Azure, Google Cloud) of pillars and review questions - reliability, security, cost, performance, operations, sustainability - for evaluating workloads.'
tags:
- cloud-infra
- cloud-platforms
domain: cloud-infra
subdomain: cloud-platforms
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'AWS. AWS Well-Architected Framework'
  resource: 'https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html'
  kind: docs
- title: 'Microsoft. Azure Well-Architected Framework'
  resource: 'https://learn.microsoft.com/en-us/azure/well-architected/'
  kind: docs
- title: 'Google Cloud. Google Cloud Well-Architected Framework'
  resource: 'https://cloud.google.com/architecture/framework'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Well-Architected Framework

**Definition.** A well-architected framework is a cloud provider's set of design principles, best practices and review questions for judging a workload. AWS introduced the idea and organises it in six pillars: operational excellence, security, reliability, performance efficiency, cost optimisation and sustainability. Microsoft Azure and Google Cloud publish equivalent frameworks with similar pillars, plus review tools that score a workload and list improvements.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `cloud-platforms`. Expanded on 2026-11-26 with independent sources.

## How it fits

The pillars are quality attributes made concrete for cloud, so the frameworks work as a checklist-style architecture review between deeper methods such as ATAM. They are provider-specific in their service advice but largely portable in their questions.

## Example

A review of a single-AZ database flags a reliability risk (no failover) and a cost finding (over-provisioned instances), each with the provider's recommended fix.

## Sources

1. AWS. AWS Well-Architected Framework. <https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html> — The six pillars and review process.
2. Microsoft. Azure Well-Architected Framework. <https://learn.microsoft.com/en-us/azure/well-architected/> — Azure's pillars and assessment.
3. Google Cloud. Google Cloud Well-Architected Framework. <https://cloud.google.com/architecture/framework> — Google Cloud's pillars and design principles.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
