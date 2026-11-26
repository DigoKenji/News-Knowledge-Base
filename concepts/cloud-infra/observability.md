---
type: Concept
title: Observability (logs, metrics, traces)
description: The ability to understand what is happening inside a running system from the signals it emits - mainly logs (events), metrics (numbers over time) and distributed traces (the path of one request across services) - so unknown problems can be investigated, not just known ones alerted on.
tags:
- cloud-infra
- observability
- core
domain: cloud-infra
subdomain: observability
also_in:
- backend-systems/reliability
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: OpenTelemetry Authors. OpenTelemetry documentation - Observability primer and Signals
  resource: 'https://opentelemetry.io/docs/concepts/observability-primer/'
  kind: standard
- title: Sigelman, B. H. et al. (2010). Dapper, a Large-Scale Distributed Systems Tracing Infrastructure. Google Technical Report
  resource: 'https://research.google/pubs/dapper-a-large-scale-distributed-systems-tracing-infrastructure/'
  kind: paper
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 6 "Monitoring Distributed Systems"
  resource: 'https://sre.google/sre-book/monitoring-distributed-systems/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Observability (logs, metrics, traces)

**Definition.** The term comes from control theory: a system is observable if its internal state can be inferred from its outputs. In software, OpenTelemetry - a CNCF standard - defines the signals (traces, metrics, logs) and a vendor-neutral way to produce and export them. Distributed tracing, described in Google's Dapper paper, propagates a trace id through every call so the latency of one request can be broken down by service.

Domain: [cloud-infra](/domains/cloud-infra.md) · subdomain `observability`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Monitoring tells you that something is wrong; observability helps you ask why, especially in distributed systems where failures cross many services. It feeds SLOs, incident response and capacity planning. Costs grow quickly with data volume and high-cardinality labels, so sampling and retention need design.

## Example

An alert fires: checkout p99 latency rose to 3 s. The trace of a slow request shows 2.7 s spent in the inventory service, its metrics show connection-pool exhaustion, and its logs show a new query introduced in this morning's deploy.

## Narrower concepts

- [Silent failure](/concepts/backend-systems/silent-failure.md)
- [SLO error budget](/concepts/cloud-infra/slo-error-budget.md)
- [Chaos engineering](/concepts/backend-systems/chaos-engineering.md)

## Sources

1. OpenTelemetry Authors. *OpenTelemetry documentation - Observability primer and Signals*. <https://opentelemetry.io/docs/concepts/observability-primer/> — Definition of observability and of traces, metrics and logs.
2. Sigelman, B. H. et al. (2010). *Dapper, a Large-Scale Distributed Systems Tracing Infrastructure*. Google Technical Report. <https://research.google/pubs/dapper-a-large-scale-distributed-systems-tracing-infrastructure/> — Distributed tracing design, trace and span model.
3. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 6 "Monitoring Distributed Systems". <https://sre.google/sre-book/monitoring-distributed-systems/> — Golden signals, symptoms vs causes, alerting philosophy.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
