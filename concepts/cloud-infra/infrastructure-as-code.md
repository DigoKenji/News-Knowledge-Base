---
type: Concept
title: Infrastructure as code
description: Defining servers, networks, databases and other infrastructure in version-controlled files that tools apply automatically, instead of clicking in consoles - so environments are reproducible, reviewable and repeatable.
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
- title: Morris, K. (2020). Infrastructure as Code - Dynamic Systems for the Cloud Age, 2nd edition. O'Reilly
  resource: 'https://infrastructure-as-code.com/book/'
  kind: book
- title: HashiCorp. What is Terraform? (Terraform documentation)
  resource: 'https://developer.hashicorp.com/terraform/intro'
  kind: docs
- title: Amazon Web Services. Introduction to DevOps on AWS - Infrastructure as code (whitepaper)
  resource: 'https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html'
  kind: docs
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 13 Never Send a Human to Do a Machine''s Job; ch. 14 If Software Eats the World, Better Use Version Control!'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Infrastructure as code

**Definition.** Morris defines infrastructure as code as an approach to automating infrastructure based on practices from software development: definitions in code, version control, testing and continuous delivery. Tools are declarative (Terraform, CloudFormation, Kubernetes manifests: describe the end state and the tool computes the changes) or imperative (scripts that list steps). Terraform's plan-then-apply cycle shows the exact changes before they are made.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

IaC brings code review, CI/CD and audit history to infrastructure and removes configuration drift between environments. The risks move too: a bad change in code can delete production at scale, and secrets must never be committed, so plans are reviewed and state files protected.

## Example

A pull request adds a new database replica in a Terraform file. CI runs terraform plan and posts "1 to add, 0 to change, 0 to destroy"; after review and merge, the pipeline applies it, and the staging environment gets the identical change from the same code.

## Narrower concepts

- [Control plane](/concepts/backend-systems/control-plane.md)
- [Well-Architected Framework](/concepts/cloud-infra/well-architected.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 13 Never Send a Human to Do a Machine's Job; ch. 14 If Software Eats the World, Better Use Version Control!: Automate everything and make the rest self-service; once infrastructure is software-defined, it needs version control, testing and a software delivery life cycle.

## Sources

1. Morris, K. (2020). *Infrastructure as Code - Dynamic Systems for the Cloud Age*, 2nd edition. O'Reilly. <https://infrastructure-as-code.com/book/> — Definition and core practices of infrastructure as code.
2. HashiCorp. *What is Terraform?* (Terraform documentation). <https://developer.hashicorp.com/terraform/intro> — Declarative configuration and the plan/apply workflow.
3. Amazon Web Services. *Introduction to DevOps on AWS - Infrastructure as code* (whitepaper). <https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html> — IaC as a DevOps practice from a provider's perspective.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 13 Never Send a Human to Do a Machine's Job; ch. 14 If Software Eats the World, Better Use Version Control!. Automate everything and make the rest self-service; once infrastructure is software-defined, it needs version control, testing and a software delivery life cycle.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
