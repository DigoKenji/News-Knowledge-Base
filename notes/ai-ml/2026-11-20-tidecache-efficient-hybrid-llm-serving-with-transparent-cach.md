---
type: Paper Summary
title: 'TideCache: Efficient Hybrid LLM Serving with Transparent Cache Management and Layer-Wise Streaming'
description: TideCache lets hybrid-model servers reallocate GPU memory across cache types and stream prefixes from host memory,
  completing up to 2.04× more agent turns.
resource: https://arxiv.org/abs/2611.26619
tags:
- ai-ml
- kv-cache
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26619
  title: 'TideCache: Efficient Hybrid LLM Serving with Transparent Cache Management and Layer-Wise Streaming'
  author: arXiv
status: stable
source_url: https://arxiv.org/abs/2611.26619
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: ai-ml
concepts:
- kv-cache
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d8165b5b5ef75b2fc91d5
---

## 🎯 In one sentence
TideCache lets a hybrid LLM server move GPU memory between cache types and stream global-attention prefixes from host memory, completing up to 2.04× as many agent turns than SGLang with HiCache across seven model/platform configurations.

## ❓ The problem
Open-weight models now mix a few global-attention layers, whose KV cache (the stored keys and values of past tokens) grows with context length, with many bounded-state layers such as sliding-window attention or recurrent layers (Mamba, Gated DeltaNet, Kimi Delta Attention), whose memory per request is fixed. Each cache type needs a different share of one GPU's memory, depending on context length and how many requests are running. A static split leaves one pool full while the other sits idle, and dynamic sharing is hard to integrate because it changes the memory layout that attention kernels and cache indexing rely on. Host-memory offloading helps, but existing schemes keep restored prefixes on the GPU for the whole turn, which limits how many sessions run at once.

## 💡 The idea
The key observation is that bounded-state layers create intervals of computation during which the historical KV of the next global-attention layer is not yet needed. During decode the CPU-to-GPU link carries almost nothing, so the system can copy each global-attention layer's host-resident prefix into a small GPU buffer just before that layer runs, then reuse that buffer for the next layer. The GPU memory this frees can hold window caches, recurrent states and more requests. The page-level remapping keeps every cache at fixed virtual addresses, so the attention backends do not need to change.

## ⚙️ How it works
### Transparent Cache Pool (TCP)
TCP manages one arena of GPU pages, allocated once in 2 MiB pages through the CUDA virtual memory API. Each cache component (global-KV, window KV, recurrent state) gets a subpool: a fixed virtual address range backed by a variable number of physical pages. To grow a subpool, TCP maps more arena pages into its range; to shrink one, it unmaps pages that no active request uses. Rebalancing runs once per epoch of E = 8 forward passes, with at most one move in flight. A worker thread does the unmap and map off the forward-pass path, and all ranks apply the move at the same epoch so their batches stay identical. Page sources are tried in order of rising cost: free pages, idle rows in other subpools, then cold cached prefixes untouched for τ seconds. Hot prefixes are never reclaimed, because discarding one reused after a tool gap forces recomputation.

### Cache Streamer
The global-KV subpool has two regions. The resident region holds the prefixes of requests that are not streamed, plus all newly generated KV. The streaming region maps the L global-attention layers onto M = 2 physical slots, with layer ℓ using slot (ℓ − 1) mod M, so a streamed prefix occupies only M/L of its full-layer footprint on the GPU. Each step, the streamer assigns streamed prefixes to contiguous slot rows, coalesces adjacent host rows into copy runs, and submits them with cudaMemcpyBatchAsync on a separate H2D stream. Two flags per layer (FREE and READY), checked inside the CUDA graph, order each copy after the previous reader and each attention read after its copy, so the graph is not split into segments. The paper calls this the Graph-Compatible Handshake; it avoids both the launch bubbles of graph segmentation and the wasted bandwidth of copying a fixed range.

The capacity bound comes from this relation: streaming A rows needs L·A·b bytes per step, and the transfers must hide under computation, so L·A·b ≤ η·BW·T. The paper's Equation 1 gives:

```text
C = η · BW · T / (L · b)        C: streamed rows per step that can be hidden
r = C · (1 − M/L) / D           r: relative capacity gain
```

Symbols: η is a scheduling slack factor below 1; BW is sustained link bandwidth; T is computation time per decode step; L is the number of global-attention layers; b is bytes per row and layer; M is the number of streaming slots; D is the row capacity of a fully resident global-KV pool with the same GPU budget. Slots cost the equivalent of M·C/L resident rows, so the net gain is C·(1 − M/L), and effective capacity reaches (1 + r)·D. The authors note this is an aggregate bandwidth bound: each layer's transfer must also meet its own deadline.

Worked example from the GPT-OSS-120B ablation: a batch of 29 requests with 8K contexts on GH200 holds 8.76 GB of BF16 global-KV across 18 global-attention layers. Moving the whole batch at 400 GB/s takes 21.9 ms per step, more than the measured 19.7 ms decode step, so only a subset of requests can be streamed. With L = 18, b = 2 KiB, M = 2, D = 147,648, T = 13.16 ms, BW = 400 GB/s and η = 0.9, the bound is C ≈ 128.5K and the capacity gain is 77%. The configured setup uses 126,976 streaming rows plus 133,056 resident rows, giving 260,032 tokens after page rounding, 1.76× the original capacity.

### Request Scheduler
The scheduler decides how to admit the head request of SGLang's waiting queue. For that request it reads its state slots ns, its uncached global-KV rows nf, and the length h of its host-cached prefix. Resident execution needs nf + h rows in the resident region. Streamed execution keeps nf rows resident and uses h streaming rows, subject to the total streamed rows staying within C. A request is streamed only when the resident region is contended: a PRESSURE estimate projects demand from waiting requests, decode growth over the mean output length, and arrivals during one turn. Prefixes shorter than hmin are not worth streaming. If neither option fits, the scheduler names the blocking subpool and TCP grows it between forward passes. A request selected for streaming skips that iteration if the slots are full, so requests needing other resources can proceed.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Seven configurations, GB300 and GH200, chat and coding-agent mixtures | SGLang w/ HiCache (Tuned) | Up to 2.04× (GH200); up to 1.44× (GB300) | Agent turns/s speedup |
| Seven configurations | SGLang w/ HiCache (Tuned) | 20–59% lower | Overall P50 per-token latency |
| Seven configurations | SGLang w/ HiCache (Tuned) | 1.40–3.05× | Agent P50 TTFT |
| Shifting workload, Qwen3.8-27B, GH200, 900 s | SGLang w/ HiCache | 183 vs 115 turns (59% more) | Agent turns completed |
| Shifting workload, same run | SGLang w/ HiCache | 39.3 ms vs 63.4 ms (38% lower) | Overall P50 latency per token |
| Shifting workload, same run | SGLang w/ HiCache | 5.68 s vs 25.70 s (78% lower) | Agent P50 TTFT |
| Kimi K3, split-ratio sweep | Best tested HiCache split (ratio 3) | 94 vs 67 turns (40% above) | Turns completed |
| GPU-only, Kimi K3 and GLM-5.3-Flash | Better of two fixed partitions | 96.3–113.6% | Turns/s |
| GPT-OSS-120B, GH200, 8K contexts | SGLang w/ HiCache | Batch 16 to 29; 148K to 260K tokens | Largest sustained batch; effective global-KV capacity |
| Same ablation | SGLang w/ HiCache | 1.19K to 1.47K tokens/s (24% gain) | Peak decode throughput |
| Equal batch size, all sustained batches | SGLang w/ HiCache | −1.1% to +6.3% | Decode-step time |

The results come from synthetic mixtures of ShareGPT and Claude Code traces on one 4-GPU GB300 node and one GH200, with 600 s measurement windows (900 s for the shifting workload) after warm-up. The baselines are tuned: the authors sweep split ratios and report the best HiCache setting, and the Kimi K3 sweep shows the best baseline still trails TideCache by 40% on turns. The ablations show that TCP alone tracks the better fixed partition and that streaming adds turns on the shifting workload (183 vs 174). The paper reports three runs only for the decode-throughput measurement; the end-to-end numbers come from single runs per configuration.

## ⚠️ Limitations
- The authors state that the streaming bound is aggregate and that each layer's transfer must still meet its deadline.
- In the evaluated revision, SGLang's Unified Memory Pool did not integrate with HiCache, so its comparison is GPU-only; UMP is also unsupported on GLM's DSA backend (N/A).
- Kimi K3 and Qwen3.8-2.4T were run with only their first 48 and 36 layers, to fit in HBM.
- Figure 9 uses one load point per configuration, chosen where session prefixes exceed resident global-KV capacity.
- Repartitioning cannot raise the total GPU cache budget; when combined demand exceeds HBM, it only changes which requests wait.
- 🔎 Added context: all workloads are synthetic mixtures built from public traces on two single-node superchip platforms. The paper does not test multi-node, multi-tenant or production traffic.
- 🔎 Added context: the bound in Equation 1 is linear in link bandwidth. Table 1 lists 128 GB/s for PCIe 5.0 ×16 against 900 GB/s for NVLink-C2C, so the same design would hide far fewer streamed rows on a PCIe host. The paper does not evaluate PCIe systems.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. The design sits in the memory-pool and admission path of an inference engine, beneath the existing radix cache and host-cache tier, so it would apply to a team that runs hybrid open-weight models on hardware with a fast CPU-GPU link. The costs are a remapping worker, double-buffered slots, and about 9K lines added to SGLang, plus a scheduler that needs accurate runtime estimates of demand. To trust it you would need to reproduce the gains on your own traffic mix and measure the host-to-GPU bandwidth your hardware actually sustains. The transferable lesson is to size memory for mixed-cache models by each cache type's growth rule (per token or per request) and to count idle links as capacity.

## 🔑 Key takeaways
- Hybrid models have two cache shapes: global-KV grows with context, while bounded states are fixed per request. One static GPU split can block requests that the other pool could serve.
- A single arena with fixed virtual addresses lets TCP move pages between cache types without changing attention backends or HiCache.
- Decode leaves the CPU-GPU link mostly idle, and bounded-state layers supply the compute needed to hide the next global-attention prefix's transfer.
- Streaming trades link bandwidth for GPU memory. Equation 1 sizes the hidden bound; in the GPT-OSS-120B ablation, effective global-KV capacity rises from 148K to 260K tokens.
- The gains are measured against a tuned baseline on two superchip platforms with synthetic traffic, so they apply most clearly to memory-bound agent workloads.
- Measured decode-step overhead is −1.1% to +6.3% at batch sizes both systems sustain, so the reported runtime cost is small.

## 📚 Reading plan
### From the paper
- [SGLang HiCache](https://www.lmsys.org/blog/2025-09-10-sglang-hicache/) — the host-memory cache tier TideCache extends and uses as its main baseline.
- [Pie: Pooling CPU memory for LLM inference](https://arxiv.org/abs/2411.09317) — layer-wise KV swapping between host and device; the closest offloading design in the comparison.
- [HiSparse](https://arxiv.org/abs/2608.07009) — fetches selected KV entries during decode, specialized for sparse attention.
- [Jenga](https://arxiv.org/abs/2503.18292) — a two-level allocator for heterogeneous caches, the nearest software-layout alternative.
- [SuperInfer](https://arxiv.org/abs/2601.20309) — rotates requests through the GPU on superchips; another superchip-oriented offloading reference.
- [Ring-2.6 technical report](https://arxiv.org/abs/2606.15079) — the 1T hybrid model whose cache sizes motivate the design.
- [SGLang](https://arxiv.org/abs/2312.07104) — the serving engine and radix cache TideCache is built on.

### To go further
- vAttention: Dynamic memory management for serving LLMs without PagedAttention (ASPLOS 2025) — the CUDA virtual-memory primitive TideCache reuses across cache components.
- vLLM Hybrid KV Cache Manager (vLLM design documentation) — the competing dynamic-sharing design, with uniformly sized shared pages.
- "Unified memory in SGLang" (SGLang blog, 2026) — the byte-buffer sharing approach the paper audits against HiCache.
- CUDA driver API: Virtual memory management (NVIDIA documentation) — needed to follow the map and unmap steps in TCP.
- Prism: Unleashing GPU sharing for cost-efficient multi-LLM serving (2025) — remaps physical pages between models on one GPU.
