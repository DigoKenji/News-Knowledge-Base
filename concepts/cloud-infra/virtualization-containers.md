---
type: Concept
title: Virtualisation and containers
description: Virtualisation runs several isolated virtual machines, each with its own operating system, on one physical host through a hypervisor; containers isolate applications that share the host's operating system kernel, making them lighter and faster to start.
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
- title: Popek, G. J. and Goldberg, R. P. (1974). Formal Requirements for Virtualizable Third Generation Architectures. Communications of the ACM 17(7)
  resource: 'https://doi.org/10.1145/361011.361073'
  kind: paper
- title: Open Container Initiative. OCI Runtime and Image Specifications
  resource: 'https://opencontainers.org/'
  kind: standard
- title: Docker. What is a container?
  resource: 'https://www.docker.com/resources/what-container/'
  kind: docs
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§12.8 Virtualization Security, pp. 432-436'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 3 Cloud Asset Management and Protection, pp. 29-48'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Virtualisation and containers

**Definition.** Popek and Goldberg formalised the requirements for a virtual machine monitor (hypervisor): equivalence, resource control and efficiency. A container packages an application with its dependencies and runs it as an isolated process using kernel features (namespaces, cgroups on Linux); the Open Container Initiative (OCI) standardises image and runtime formats so the same image runs anywhere. Containers isolate less strongly than VMs, so multi-tenant platforms sometimes run containers inside lightweight VMs.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Virtualisation is what makes IaaS possible (resource pooling); containers are the unit of deployment for microservices, CI/CD and Kubernetes. Containers fix "works on my machine" by shipping the environment with the code. Images must still be scanned and signed, because they are a supply-chain risk.

## Example

One physical server runs 20 virtual machines for different customers. Inside one VM, a team runs 15 containers built from OCI images - API, worker, cache - each starting in under a second and identical in development and production.

## Narrower concepts

- [Container orchestration (Kubernetes)](/concepts/cloud-infra/container-orchestration.md)
- [Serverless](/concepts/cloud-infra/serverless.md)
- [Remote attestation](/concepts/security/remote-attestation.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §12.8 Virtualization Security, pp. 432-436: Hypervisor types and virtualisation security issues.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 3 Cloud Asset Management and Protection, pp. 29-48: Inventorying and protecting VMs, containers and other cloud assets.

## Sources

1. Popek, G. J. and Goldberg, R. P. (1974). *Formal Requirements for Virtualizable Third Generation Architectures*. Communications of the ACM 17(7). <https://doi.org/10.1145/361011.361073> — Formal requirements for virtual machine monitors.
2. Open Container Initiative. *OCI Runtime and Image Specifications*. <https://opencontainers.org/> — Open standards for container images and runtimes.
3. Docker. *What is a container?*. <https://www.docker.com/resources/what-container/> — Containers vs virtual machines in practice.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §12.8 Virtualization Security, pp. 432-436. Hypervisor types and virtualisation security issues.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 3 Cloud Asset Management and Protection, pp. 29-48. Inventorying and protecting VMs, containers and other cloud assets.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
