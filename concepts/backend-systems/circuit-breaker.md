---
type: Concept
title: Circuit breaker
description: A resiliency pattern that stops calls to an operation likely to fail instead of repeating them, so the failing
  dependency can recover and the caller is protected.
tags:
- backend-systems
- reliability
domain: backend-systems
subdomain: reliability
status: draft
relations:
- type: mitigates
  target: /concepts/backend-systems/cascading-failure.md
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- title: 'EP234: Docker vs Kubernetes'
  resource: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
  kind: practitioner
---

# Circuit breaker

**Definition.** A resiliency pattern that stops calls to an operation likely to fail instead of repeating them, so the failing dependency can recover and the caller is protected.

Domain: [backend-systems](/domains/backend-systems.md). From the daily run on 2026-11-27; draft, waiting for Kenji.

## Relations
- **mitigates** [cascading-failure](/concepts/backend-systems/cascading-failure.md)

## Notes
- [EP234: Docker vs Kubernetes](/notes/cloud-infra/2026-11-27-ep234-docker-vs-kubernetes.md)
