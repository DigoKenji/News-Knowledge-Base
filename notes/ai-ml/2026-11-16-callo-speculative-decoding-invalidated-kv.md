---
type: Paper Summary
title: 'Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation'
description: Callo drafts tokens on a stale KV cache while the exact prefill runs, committing only verified tokens, for 1.05-1.19x speedups over vLLM on a SWE-agent workload.
resource: https://arxiv.org/abs/2611.18459
tags:
- ai-ml
- llm-engineering
- kv-cache
- speculative-decoding
- prefill-decode-disaggregation
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.18459
  title: 'Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation'
  author: Jianjun Ou, Matthew Lentz
status: stable
source_url: https://arxiv.org/abs/2611.18459
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-13'
domain: ai-ml
subdomain: llm-engineering
concepts:
- kv-cache
- speculative-decoding
- prefill-decode-disaggregation
- inference-tradeoffs
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d8191acbce9e684f19975
---

# Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation

**Source:** https://arxiv.org/abs/2611.18459
**Authors:** Jianjun Ou, Matthew Lentz
**Institution:** Duke University
**Published:** November 2026 (submitted 13 Nov 2026, arXiv:2611.18459v1) | **Venue:** Preprint
**Tags:** #llm-serving #speculative-decoding #agents #prefill-decode-disaggregation

---

## 📄 Abstract (in plain language)

Agent sessions often edit their context, which invalidates the [KV cache](/concepts/kv-cache.md) of the edited suffix and forces a re-prefill before decoding. CALLO drafts tokens from this invalidated KV while exact KV is recomputed, then verifies drafts against exact KV before committing them. On a two-GPU SWE-agent testbed it reports all-request speedups of 1.05–1.19× over vLLM at moderate load.

## 🎯 Core Contribution

The authors argue that the miss penalty is a wait on the decode instance, and that this instance already holds the invalidated KV, which is close enough to the exact KV to draft from [Kwon et al., 2023]. Existing remedies do not fit this case: [Prefill-decode disaggregation](/concepts/prefill-decode-disaggregation.md) [Zhong et al., 2024] moves prefill off the decode path but does not help the affected session, and approximate-KV methods such as [Ma et al., 2026] can change outputs. CALLO stays lossless by using invalidated KV only to propose tokens and committing only what exact KV confirms.

## 🏗️ Methodology

### Problem Formulation

An edit-induced miss puts exact re-prefill on the critical path. The goal is to shorten that wait without changing outputs; the miss penalty is the wait until exact KV lands on the D-instance.

### Proposed Method: CALLO

CALLO runs on disaggregated vLLM with a P-instance (prefill) and a D-instance (decode). The D-instance drafts with the target model over repositioned invalidated KV, using [Speculative decoding](/concepts/speculative-decoding.md) in the draft-and-verify sense of [Leviathan et al., 2023] and [Chen et al., 2023].

### Provisional KV Staging

The D-instance aligns the previous request's tokens (including its reply) with the new prompt, copies KV for mapped positions to their new positions, and re-rotates keys with a fused Triton kernel. Unmapped spans are computed as ordinary prefill chunks of at most 112 tokens; the median is 424 tokens per speculating miss request.

### Overlapped Drafting and Streaming

The P-instance pushes each layer's exact KV as it is computed, and the D-instance drafts on the same blocks without locks.

### Verification

Once exact KV is complete, one forward pass over the drafts accepts them up to the first rejection, using exact KV only.

### Key Design Decisions

- **Lossless commit only:** approximate-KV drafts are never committed, so outputs match the target's decoding rule.
- **Layer-major delivery:** drafts accept significantly more once half the layers are exact; position-major delivery gives no significant gain over invalidated KV.
- **Cumulative early stop (θ = 0.3):** drafting stops when the product of per-draft agreement probabilities falls below 0.3. This cuts discarded drafts by 30% while keeping 97% of accepted ones.
- **Yielding under contention:** drafting requests with the lowest cumulative acceptance give up slots first, keeping their drafts.

## 🧪 Experimental Setup

Two H200 NVL GPUs (one P-instance, one D-instance) run vLLM 0.29 with NIXL transfer. Models are Llama-3.3-70B-Instruct (FP8), Gemma-4-31B and Mistral-Small-3.2-24B (bf16). The workload is SWE-agent 1.1 on 100 SWE-bench Verified issues, keeping the last five (L5) or ten (L10) tool outputs, at loads of 12%, 52% and 100% of capacity, derived from Azure coding traffic [Stojkovic et al., 2025]. Baselines are unmodified vLLM and CALLO-Stream (CALLO's transfer path with speculation disabled). The metric is the ratio of vLLM to CALLO mean latency, reported for miss requests and for all requests.

## 📊 Results

### Main Results

Mean latency ratios (vLLM / CALLO), D-instance batch limit 16:

| Workload | Light Miss | Light All | Moderate Miss | Moderate All | Full Miss | Full All |
|---|---|---|---|---|---|---|
| Llama L5 | 1.13 | 1.10 | 1.19 | 1.14 | 1.32 | 1.27 |
| Llama L10 | 1.24 | 1.17 | 1.24 | 1.19 | 1.21 | 1.17 |
| Gemma L5 | 1.07 | 1.05 | 1.06 | 1.05 | 1.10 | 1.09 |
| Gemma L10 | 1.11 | 1.07 | 1.11 | 1.08 | 1.12 | 1.10 |
| Mistral L5 | 1.15 | 1.14 | 1.16 | 1.14 | 1.16 | 1.15 |
| Mistral L10 | 1.21 | 1.15 | 1.19 | 1.14 | 1.15 | 1.13 |

For Llama L5, the all-request speedup rises from 1.10× at light load to 1.27× at full load. Speculation alone accounts for 8% of mean all-request latency at light load and 10% at full load, relative to CALLO-Stream. At a batch limit of two, the speedup stays near 1.10–1.12×, mostly from the transfer path. On speculating miss requests at moderate load, exact KV arrives 0.4 s sooner and the remaining decode shrinks by another 0.4 s, folding 47% of the miss penalty.

### Ablation Studies

Single-request runs on Llama-3.3-70B (Table 4):

- Fixed-tie CALLO hides 49% of the miss penalty (1.15×); CALLO-Stream hides 4.5% (1.01×).
- Any-max verification hides 54% (1.17×); speculative sampling at T = 0.8 hides 45% (1.14×).
- Prompt-lookup n-grams [Saxena, 2023] hide only 18% (1.05×), with 7.5 accepted drafts versus 22.6.

### Correctness and Overheads

Across 1,928 verification events and 44,792 committed tokens, instrumented runs show no KV-fingerprint mismatches and no acceptance, output or rollback violations. Non-speculating requests on the same D-instance are about 2% slower at low load and 6% slower near capacity; the reported gains include this cost. The result is a latency-versus-capacity trade-off of the kind discussed in [Inference throughput/latency/cost](/concepts/inference-tradeoffs.md).

### Cluster-Scale Simulation

In a simulated 96-GPU cluster, speculation's miss-request speedup over CALLO-Stream is 1.110–1.131× at light load across P:D ratios. Weighted by Azure's weekly load, it is 1.130×, 1.095×, 1.044× and 1.027× for P:D ratios of 1:2, 1:1, 2:1 and 3:1. These are against CALLO-Stream, not vLLM.

### Limitations Acknowledged by Authors

- Testbed results cover two GPUs and a small issue set; cluster results are simulated.
- CALLO needs session affinity, the previous request's KV still cached, and spare decode capacity; otherwise requests take the ordinary path.
- Drafts fail where the model hesitates, so gains depend on model confidence; for very long reasoning outputs the relative gain is small.
- Beyond capacity, queues keep growing and the ratios are not steady-state gains.
- Whether caches computed before an adapter switch draft well is left open.

## 💡 Key Insights

- Invalidated KV misses only the edit, so its error is structured: the predicted next token matched the exact KV at 98.3% of positions.
- Draft failures are foreseeable from the target's own logit gap. Where the gap is at least one, agreement with the exact model is 97–100%; below 0.5 it is 65%. Near-ties are 2.4% of draft positions but 60% of first mismatches.
- The authors position CALLO for latency-sensitive tasks with short replies, where the re-prefill wait is a large share of remaining latency.

## 🔍 Follow-up References

Drawn from the paper's own bibliography.

### Foundational Works This Paper Builds On

- 📚 Leviathan et al., 2023 — *Fast inference from transformers via speculative decoding* (ICML) → The draft-and-verify scheme that CALLO's verification rules depend on.
- 📚 Chen et al., 2023 — *Accelerating large language model decoding with speculative sampling* (arXiv:2302.01318) → Speculative sampling, one of CALLO's verification modes.
- 📚 Kwon et al., 2023 — *Efficient memory management for large language model serving with PagedAttention* → The vLLM engine and prefix-cache semantics CALLO builds on.
- 📚 Zhong et al., 2024 — *DistServe: Disaggregating prefill and decoding* (OSDI) → Prefill-decode disaggregation, which CALLO extends.

### Directly Cited Competing or Related Methods

- 📄 Ma et al., 2026 — *Leyline: KV-cache edit directives for efficient agentic reasoning* → Re-rotates the suffix after a span edit but decodes on approximate KV.
- 📄 Mao et al., 2026 — *Contiguity, not importance* → Budgeted repair of edit-local positions; also approximate-KV decoding.
- 📄 Yang et al., 2026 — *PatchKV* → Recomputes edit-local positions; also approximate-KV decoding.
- 📄 Anonymous, 2026 — *Speculating on the past: Lossless reuse of stale KV caches via self-speculative verification* (https://openreview.net/forum?id=96mkAG7gh1) → Concurrent work with the same core idea; the paper says CALLO goes further on streaming and scheduling.

### Cited Datasets, Benchmarks, or Tools

- 🔗 SWE-agent (Yang et al., 2024) → Harness and the L5/L10 context-management settings used in every workload.
- 🔗 Azure coding traffic trace, via Stojkovic et al., 2025 → Load levels and weekly distribution for the cluster simulation.
- 🔗 SWE-chat (Baumann et al., 2026, https://arxiv.org/abs/2604.20779) → Source of the Claude Code session statistics in the introduction.

### Suggested External Reading (Not in Paper)

None added; this summary uses only the paper's text and bibliography.

---
*Summary generated: 2026-11-16*
