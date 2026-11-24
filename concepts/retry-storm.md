---
type: Concept
title: Retry storm
description: A surge of retried requests that hits a struggling dependency all at once - often multiplied across layers - adding load exactly when it can least cope and keeping it overloaded after the original problem is gone.
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
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Amazon Builders' Library (AWS Builder Center). Timeouts, retries, and backoff with jitter
  resource: 'https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter'
  kind: practitioner
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 22 Addressing Cascading Failures. O'Reilly / Google
  resource: 'https://sre.google/sre-book/addressing-cascading-failures/'
  kind: book
- title: gRPC. A6 - gRPC Retry Design (proposal)
  resource: 'https://github.com/grpc/proposal/blob/master/A6-client-retries.md'
  kind: standard
- title: Huang, L. et al. (2022). Metastable Failures in the Wild. OSDI 2022
  resource: 'https://www.usenix.org/conference/osdi22/presentation/huang-lexiang'
  kind: paper
---

# Retry storm

**Definition.** A retry storm happens when many clients respond to errors or timeouts by retrying, so the failing service receives a multiple of its normal traffic. Retries at several layers multiply: if each of 3 layers tries 3 times, one user action can become 27 calls to the bottom service.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `system-design`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Retries help with transient, independent failures, but during overload the failures are correlated and retries only add load. Synchronised clients make it worse, because they all retry at the same moments. Defences: exponential backoff with jitter (randomised delay) to spread retries out; retry budgets or token buckets that cap retries to a fraction of normal traffic (for example 10%, as in gRPC retry throttling and Google's SRE guidance); retrying at only one layer; not retrying on overload errors; circuit breakers; and server-side load shedding. Retry storms are a classic trigger of metastable failures.

## Why it matters and trade-offs

Well-meant resilience code can turn a brief blip into a long outage. Limiting retries means some requests fail that a retry would have saved; the trade-off is accepted because it protects the whole system.

## Example

A database slows for 10 seconds; 10,000 clients each retry 5 times without jitter, sending 50,000 extra queries that keep the database saturated for 20 minutes.

## Review notes for Kenji

The draft wording ("retries from accepted work") was unclear; rewritten around the standard explanation.

## Sources

1. Amazon Builders' Library (AWS Builder Center). *Timeouts, retries, and backoff with jitter*. <https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter> — Retry amplification across layers, backoff, jitter, token-bucket retry limits.
2. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 22 Addressing Cascading Failures. O'Reilly / Google. <https://sre.google/sre-book/addressing-cascading-failures/> — Retries as a cause of cascading failure; per-request and per-client retry budgets.
3. gRPC. *A6 - gRPC Retry Design* (proposal). <https://github.com/grpc/proposal/blob/master/A6-client-retries.md> — Retry throttling built into a widely used RPC framework.
4. Huang, L. et al. (2022). *Metastable Failures in the Wild*. OSDI 2022. <https://www.usenix.org/conference/osdi22/presentation/huang-lexiang> — Retries as a sustaining effect in real metastable incidents.

## Relations
- (none yet)

## Notes
- [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md)
