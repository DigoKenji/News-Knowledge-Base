---
type: Concept
title: Correlated failure
description: Several components that were meant to fail independently fail together because they share a hidden cause - a dependency, a configuration push, a software bug, a power or network domain - which defeats redundancy designed on the assumption of independence.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Ford, D. et al. (2010). Availability in Globally Distributed Storage Systems. OSDI 2010
  resource: 'https://www.usenix.org/conference/osdi10/availability-globally-distributed-storage-systems'
  kind: paper
- title: AWS (2022). AWS Fault Isolation Boundaries. Whitepaper
  resource: 'https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/abstract-and-introduction.html'
  kind: practitioner
- title: AWS (2021). Summary of the AWS Service Event in the Northern Virginia (US-EAST-1) Region
  resource: 'https://aws.amazon.com/message/12721/'
  kind: practitioner
---

# Correlated failure

**Definition.** Failures are correlated when the failure of one component makes the failure of another more likely. Redundancy math (two 99.9% replicas give 99.9999%) assumes independence; correlated failures break that assumption, so real availability is much lower than the math says.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `system-design`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Typical shared causes: the same software version or bad config deployed everywhere at once; a shared dependency (DNS, identity, a control plane, a single region's service that "global" features rely on); shared physical domains (rack power, switch, availability zone); and shared load, where failover traffic overloads the survivors. Ford et al. found at Google that failure bursts across many nodes, often within one rack, dominate data unavailability, and that models assuming independence underestimate unavailability by orders of magnitude. Defences: spread replicas across failure domains (racks, zones, regions, even providers), stagger deployments (cells, waves, canaries), remove hidden cross-region dependencies, and test failover for real.

## Why it matters and trade-offs

Correlated failure is why multi-region architectures still go down together. Reducing it costs money (more domains, more replicas), slows deployment (staggered waves), and requires dependency mapping that is easy to let rot.

## Example

Two regions are both "active", but both read feature flags from a service hosted only in us-east-1; when that region has an incident, both regions fail at the same time.

## Sources

1. Ford, D. et al. (2010). *Availability in Globally Distributed Storage Systems*. OSDI 2010. <https://www.usenix.org/conference/osdi10/availability-globally-distributed-storage-systems> — Measured correlated failure bursts at Google and their impact on availability models.
2. AWS (2022). *AWS Fault Isolation Boundaries*. Whitepaper. <https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/abstract-and-introduction.html> — Zones, regions and cells as independent failure domains; hidden global dependencies.
3. AWS (2021). *Summary of the AWS Service Event in the Northern Virginia (US-EAST-1) Region*. <https://aws.amazon.com/message/12721/> — A real incident where an internal network issue in one region affected many dependent services together.

## Relations
- (none yet)

## Notes
- [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md)
