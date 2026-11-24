---
type: Concept
title: Prefill-decode disaggregation
description: Running the two phases of LLM inference - prefill (processing the prompt) and decode (generating tokens one by one) - on separate GPU pools, transferring the KV cache between them, so each can be sized and tuned for its own bottleneck.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Zhong, Y. et al. (2024). DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving. OSDI 2024, arXiv:2401.09670'
  resource: 'https://arxiv.org/abs/2401.09670'
  kind: paper
- title: 'Patel, P. et al. (2024). Splitwise - Efficient Generative LLM Inference Using Phase Splitting. ISCA 2024, arXiv:2311.18677'
  resource: 'https://arxiv.org/abs/2311.18677'
  kind: paper
- title: 'Qin, R. et al. (2024). Mooncake - A KVCache-centric Disaggregated Architecture for LLM Serving. arXiv:2407.00079'
  resource: 'https://arxiv.org/abs/2407.00079'
  kind: paper
- title: vLLM. Disaggregated Prefilling (experimental)
  resource: 'https://docs.vllm.ai/en/latest/features/disagg_prefill/'
  kind: docs
relations:
- type: uses
  target: /concepts/ai-ml/kv-cache.md
- type: part-of
  target: /concepts/ai-ml/inference-tradeoffs.md
---

# Prefill-decode disaggregation

**Definition.** In disaggregated serving, a request first goes to a prefill instance that runs the whole prompt in one parallel pass and builds the KV cache. The cache is then shipped over a fast interconnect to a decode instance, which generates the output token by token.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The phases stress hardware differently. Prefill is compute-bound (big matrix multiplications over many tokens) and sets time to first token (TTFT). Decode is memory-bandwidth-bound (one token at a time, reading all weights and the cache) and sets time per output token (TPOT). On shared GPUs a long prefill stalls everyone's decoding, so latency targets conflict. Separating them removes the interference and lets operators pick parallelism, batch sizes and even GPU types per phase. DistServe reported up to 7.4× more requests within SLO; Splitwise and Moonshot's Mooncake showed similar gains in production-like settings; vLLM, SGLang and NVIDIA Dynamo now support it.

## Why it matters and trade-offs

It raises goodput under strict latency SLOs at large scale. Costs: moving the KV cache needs high-bandwidth networking (NVLink, RDMA), the scheduler gets more complex, and for small deployments or short prompts the transfer overhead can outweigh the benefit; chunked prefill on shared GPUs is the simpler alternative.

## Example

A chat service with 20,000-token RAG prompts puts prefill on a few compute-heavy GPUs and decode on many memory-rich ones, keeping streaming smooth while big prompts arrive.

## Sources

1. Zhong, Y. et al. (2024). *DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving*. OSDI 2024, arXiv:2401.09670. <https://arxiv.org/abs/2401.09670> — Interference between phases, disaggregation, goodput gains.
2. Patel, P. et al. (2024). *Splitwise - Efficient Generative LLM Inference Using Phase Splitting*. ISCA 2024, arXiv:2311.18677. <https://arxiv.org/abs/2311.18677> — Independent work (Microsoft) on phase splitting, including different hardware per phase.
3. Qin, R. et al. (2024). *Mooncake - A KVCache-centric Disaggregated Architecture for LLM Serving*. arXiv:2407.00079. <https://arxiv.org/abs/2407.00079> — Production deployment (Kimi) built around a disaggregated KV cache.
4. vLLM. *Disaggregated Prefilling (experimental)*. <https://docs.vllm.ai/en/latest/features/disagg_prefill/> — Support in a widely used open-source serving engine.

## Relations
- **uses** [KV cache](/concepts/ai-ml/kv-cache.md)
- **part-of** [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
