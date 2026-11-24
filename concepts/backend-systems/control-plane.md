---
type: Concept
title: Control plane
description: The part of a system that decides and configures what should happen - placement, routing rules, desired state, scheduling - as opposed to the data plane, which does the actual work on each request or packet.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Yang, L. et al. (2004). RFC 3746 - Forwarding and Control Element Separation (ForCES) Framework. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc3746'
  kind: standard
- title: Kubernetes. Kubernetes Components
  resource: 'https://kubernetes.io/docs/concepts/overview/components/'
  kind: docs
- title: AWS (2022). AWS Fault Isolation Boundaries - Control planes and data planes. Whitepaper
  resource: 'https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/control-planes-and-data-planes.html'
  kind: practitioner
---

# Control plane

**Definition.** The control plane / data plane split comes from networking: the control plane computes routes, the data plane forwards packets. In cloud and distributed systems, the control plane is the management layer (APIs to create, change and delete resources, schedulers, controllers that reconcile desired and actual state), and the data plane is what serves traffic (VMs, containers, load balancers, storage reads and writes).

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `system-design`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Kubernetes is the textbook example: the API server, etcd, scheduler and controller manager form the control plane; kubelets and pods on worker nodes are the data plane. In a service mesh, Istio's istiod is the control plane and the Envoy sidecars the data plane. AWS designs for static stability: the data plane keeps running with its last known configuration if the control plane fails, because control planes are more complex, change more often and fail more often. Control planes typically handle far less traffic but need strong consistency; data planes need high throughput and availability.

## Why it matters and trade-offs

Separating them lets each scale and fail independently, and keeps a management outage from becoming a serving outage. The risk is coupling sneaking back in: a data plane that calls the control plane on every request, or recovery steps that need the control plane while it is down.

## Example

During an AWS EC2 control-plane incident you may be unable to launch new instances, while instances already running keep serving traffic normally.

## Review notes for Kenji

The draft narrowed this to job orchestration ("decides what work runs, when"); it is now the general control/data-plane split, of which job scheduling is one case.

## Sources

1. Yang, L. et al. (2004). *RFC 3746 - Forwarding and Control Element Separation (ForCES) Framework*. IETF. <https://www.rfc-editor.org/rfc/rfc3746> — The networking origin of separating control from forwarding.
2. Kubernetes. *Kubernetes Components*. <https://kubernetes.io/docs/concepts/overview/components/> — Control plane components vs node components.
3. AWS (2022). *AWS Fault Isolation Boundaries - Control planes and data planes*. Whitepaper. <https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/control-planes-and-data-planes.html> — Cloud definition, static stability, why control planes fail more often.

## Relations
- (none yet)

## Notes
- [OpenAI Just Open-Sourced Symphony. Every AI Startup Should Pay Attention](/notes/ai-ml/2026-11-20-openai-just-open-sourced-symphony-every-ai-startup-should-pa.md)
