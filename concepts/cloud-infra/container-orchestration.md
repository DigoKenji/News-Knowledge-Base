---
type: Concept
title: Container orchestration (Kubernetes)
description: Automating the deployment, scaling, networking and healing of containers across a cluster of machines; Kubernetes is the dominant open-source orchestrator, where you declare the desired state and controllers keep reality matching it.
tags:
- cloud-infra
- containers
- core
domain: cloud-infra
subdomain: containers
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: The Kubernetes Authors. Kubernetes documentation - Overview and Concepts
  resource: 'https://kubernetes.io/docs/concepts/overview/'
  kind: docs
- title: Burns, B., Grant, B., Oppenheimer, D., Brewer, E. and Wilkes, J. (2016). Borg, Omega, and Kubernetes. ACM Queue 14(1)
  resource: 'https://queue.acm.org/detail.cfm?id=2898444'
  kind: paper
- title: Verma, A. et al. (2015). Large-scale cluster management at Google with Borg. EuroSys 2015
  resource: 'https://doi.org/10.1145/2741948.2741964'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Container orchestration (Kubernetes)

**Definition.** An orchestrator schedules containers onto machines, restarts them when they fail, scales replica counts, gives them stable network names and load balancing, and rolls out new versions. Kubernetes, descended from Google's Borg and Omega, is declarative: users submit objects (Deployments, Services) describing desired state to an API server backed by etcd, and control loops continuously reconcile the actual state towards it.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `containers`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Orchestration makes running hundreds of containers manageable and is the typical platform for microservices and for self-hosted model serving. It brings real complexity (networking, storage, security policies, upgrades), so small teams often prefer managed Kubernetes or serverless platforms instead.

## Example

A team declares "run 6 replicas of api:v2" in a Deployment. When a node dies, Kubernetes notices only 4 are running and starts 2 more elsewhere; when they push api:v3, it replaces pods gradually and stops if health checks fail.

## Narrower concepts

- [Control plane](/concepts/cloud-infra/control-plane.md)
- [Canary / blue-green deploy](/concepts/software-engineering/canary-deploy.md)
- [Infrastructure as code](/concepts/cloud-infra/infrastructure-as-code.md)

## Sources

1. The Kubernetes Authors. *Kubernetes documentation - Overview and Concepts*. <https://kubernetes.io/docs/concepts/overview/> — What Kubernetes does, objects, desired state and controllers.
2. Burns, B., Grant, B., Oppenheimer, D., Brewer, E. and Wilkes, J. (2016). *Borg, Omega, and Kubernetes*. ACM Queue 14(1). <https://queue.acm.org/detail.cfm?id=2898444> — Lineage and design lessons of cluster orchestration at Google.
3. Verma, A. et al. (2015). *Large-scale cluster management at Google with Borg*. EuroSys 2015. <https://doi.org/10.1145/2741948.2741964> — Original large-scale orchestrator design.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
