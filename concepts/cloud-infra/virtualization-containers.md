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

## Sources

1. Popek, G. J. and Goldberg, R. P. (1974). *Formal Requirements for Virtualizable Third Generation Architectures*. Communications of the ACM 17(7). <https://doi.org/10.1145/361011.361073> — Formal requirements for virtual machine monitors.
2. Open Container Initiative. *OCI Runtime and Image Specifications*. <https://opencontainers.org/> — Open standards for container images and runtimes.
3. Docker. *What is a container?*. <https://www.docker.com/resources/what-container/> — Containers vs virtual machines in practice.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
