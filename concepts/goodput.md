---
type: Concept
title: Goodput
description: The rate of useful work a system delivers - requests (or tokens, or training steps) that complete correctly and within their latency target - as opposed to raw throughput, which also counts work that was late, failed or repeated.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Newman, D. (1999). RFC 2647 - Benchmarking Terminology for Firewall Performance, section 3.17 Goodput. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc2647'
  kind: standard
- title: 'Zhong, Y. et al. (2024). DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving. OSDI 2024, arXiv:2401.09670'
  resource: 'https://arxiv.org/abs/2401.09670'
  kind: paper
- title: Google Cloud (2024). Goodput metric as measure of ML productivity
  resource: 'https://cloud.google.com/blog/products/ai-machine-learning/goodput-metric-as-measure-of-ml-productivity'
  kind: practitioner
---

# Goodput

**Definition.** Goodput is throughput counted only for work that actually served its purpose. The term comes from networking, where it means application data delivered per second excluding protocol overhead and retransmissions. In LLM serving, DistServe defined it as the maximum request rate a system can sustain while meeting its latency SLOs (time to first token and time per output token).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Measure it by setting a success criterion per unit of work (status OK, latency under the SLO, result not discarded) and counting only units that meet it. Under overload, throughput can stay high while goodput collapses: the server is busy but every response arrives after the client has timed out, or retries repeat the same work. In ML training, Google uses "ML goodput" for the share of time spent on productive training steps rather than restarts, checkpoint reloads and idle accelerators.

## Why it matters and trade-offs

Optimising for throughput alone hides overload and invites batching or queueing that hurts users. Goodput ties capacity planning to what users experience. It requires an explicit SLO and good measurement; with a loose SLO goodput just equals throughput.

## Example

An LLM server processes 100 requests per second, but only 60 return their first token within the 1-second SLO: throughput is 100 req/s, goodput 60 req/s.

## Review notes for Kenji

The draft limited goodput to "requests that finish within their timeout"; it is now the general idea with the networking origin and the LLM-serving definition.

## Sources

1. Newman, D. (1999). *RFC 2647 - Benchmarking Terminology for Firewall Performance*, section 3.17 Goodput. IETF. <https://www.rfc-editor.org/rfc/rfc2647> — Networking definition - bits forwarded to the correct destination, minus lost or retransmitted ones.
2. Zhong, Y. et al. (2024). *DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving*. OSDI 2024, arXiv:2401.09670. <https://arxiv.org/abs/2401.09670> — Goodput for LLM serving as request rate meeting TTFT and TPOT SLOs.
3. Google Cloud (2024). *Goodput metric as measure of ML productivity*. <https://cloud.google.com/blog/products/ai-machine-learning/goodput-metric-as-measure-of-ml-productivity> — ML goodput for training - productive time vs lost time.

## Relations
- (none yet)

## Notes
- [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
