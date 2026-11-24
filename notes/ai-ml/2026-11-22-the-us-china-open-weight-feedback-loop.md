---
type: Article Summary
title: The US-China Open-Weight Feedback Loop
description: US and Chinese open-weight models improve each other, and proprietary models benefit too, so competition helps
  everyone.
resource: https://sebastianraschka.com/blog/2026/us-china-open-weight-feedback-loop.html
tags:
- ai-ml
- rope
- kv-cache
- mixture-of-experts
- multi-head-latent-attention
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://sebastianraschka.com/blog/2026/us-china-open-weight-feedback-loop.html
  title: The US-China Open-Weight Feedback Loop
  author: Sebastian Raschka
status: stable
source_url: https://sebastianraschka.com/blog/2026/us-china-open-weight-feedback-loop.html
source: Sebastian Raschka
source_type: blog
published: '2026-11-21'
domain: ai-ml
concepts:
- rope
- kv-cache
- mixture-of-experts
- multi-head-latent-attention
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d81adafc7d665b3bc0c4f
---

## 🎯 Core thesis
US and Chinese open-weight models depend on each other's ideas, and proprietary models are both better and cheaper because open-weight models exist, so competition between them is good.

## 🧭 Why the author wrote it
The post starts from a claim about how architecture ideas spread across labs and countries, and the table of references is the evidence for it. 🔎 Added context: the article says it was originally posted as a Substack Note, so it is a short, opinion-led post rather than a full study. If the thesis holds, the designs in open-weight releases act as a shared commons that proprietary progress also draws on.

## The opening claim
### Open-weight models
The author says US open-weight models "wouldn't be this good without Chinese open-weight models and vice versa." The claim is about mutual dependence, not about one side copying the other. 🔎 Added context: open-weight means the trained parameters are published so others can download, run and build on them. The article does not define the term.

### Proprietary models
The author says proprietary models "wouldn't be this good (and cheap) without open-weight models." The article gives no mechanism or figures for this link, so it should be read as the author's view. 🔎 Added context: proprietary models are offered through an API or product, and their weights are not published.

### Competition
The post closes its opening with a one-line rule: "Competition is a good thing."

## References
The table lists, for each model, the earlier idea it adopted. The author notes the figure it refers to is not comprehensive, only a selection.

### Rotary position embeddings (RoPE)
LLaMA (2023) uses RoFormer's rotary position embeddings to implement the popular RoPE method, which encodes token positions by rotating queries and keys in each attention layer. The author points to his full article on positional embeddings for more detail, but that article is not in the extracted text.
> 🔎 Added context: rotating queries and keys by position-dependent angles makes their dot product depend on the distance between tokens. That is why attention can reason about how far apart things are.

### Grouped-query attention
DeepSeek LLM 67B (2023) is the model cited here. The article says grouped-query attention "is now a standard technique where query heads share sets of keys and values."
> 🔎 Added context: in standard multi-head attention each query head has its own keys and values. Grouped-query attention lets several query heads share one set, which shrinks the KV cache. The article gives no size figure, so none is stated here.

### Fine-grained experts (mixture of experts)
DeepSeekMoE popularized the fine-grained expert design, and OLMoE (2024) and many others adapted it. The article credits the lineage and gives no expert counts.
> 🔎 Added context: a mixture-of-experts layer holds many expert subnetworks, and a router sends each token to only a few of them. Fine-grained designs use more, smaller experts.

### Multi-token prediction
DeepSeek-V3 (2024) adapted Meta's multi-token prediction objective. During training, an extra module predicts one token beyond the usual next-token target.
> 🔎 Added context: the standard objective trains the model to predict only the next token, and this adds a training signal for a token further ahead. The article describes the extra module only as a training component and does not say whether it is used at inference.

### Multi-head latent attention (MLA)
DeepSeek-V2 popularized MLA as an alternative to GQA combined with sliding-window approaches for reducing the KV cache. In Kimi K2 (2025), keys and values share a compressed representation in the cache. The author links his MLA page for detail, which is not in the text.
> 🔎 Added context: MLA caches one compressed representation from which keys and values can be recovered, instead of caching full keys and values per head.

### Hybrid attention with the gated delta rule
Qwen3-Next (2025) was one of the first architectures to use an attention hybrid design with the gated delta rule from Gated DeltaNet. The author links a page on hybrid attention, which is not in the text.
> 🔎 Added context: a hybrid design mixes attention layers with other sequence-mixing layers. The gated delta rule is an update rule for a recurrent memory state. The article gives no layer ratios, so none are stated.

### Learned gates on attention output
Arcee's Trinity Large (2026) uses a learned sigmoid gate, taken from Qwen's gated attention work, to scale the attention output before the output projection. AMD's Instella-MoE (2026) combines DeepSeek-style MLA with attention output gating, where the input determines how much of the MLA output passes through.
> 🔎 Added context: a sigmoid squashes any value into the range between 0 and 1, so the gate can scale each attention output from nothing up to full strength. In Instella the gate value depends on the input, so the amount passed through varies per token.

### Quantile balancing for router bias
Marin Mochi (2026) uses Jianlin Su's quantile balancing method, as the table attributes it to Open Athena. The method bins the raw router scores to estimate how to adjust each expert's routing bias.
> 🔎 Added context: a routing bias shifts which experts a router prefers, so adjusting it can spread load across experts. The article does not say how the bins are formed or how large the adjustments are.

## 🧠 Mental models
### Feedback loop between open and proprietary models
**The model:** Ideas move between open-weight and proprietary models, and between countries. Each release becomes a building block for others, so quality rises on both sides together.
**How to apply it:** When assessing a model family, trace which techniques it borrowed and which ones it released for others to reuse.

### Competition as a driver
**The model:** Competition between labs is what keeps the loop going, in the author's words "a good thing."
**How to apply it:** Treat a landscape with several model sources as healthier than one dominated by a single vendor, and check that your architecture does not depend on one source.

## 🔑 Key takeaways
- The author argues that US and Chinese open-weight models would not be this good without each other.
- The author says proprietary models are both better and cheaper because open-weight models exist, without giving figures.
- Techniques travel along named lineages: RoPE from RoFormer to LLaMA (2023), grouped-query attention in DeepSeek LLM 67B (2023), and fine-grained experts from DeepSeekMoE to OLMoE (2024).
- Later models keep borrowing: MLA from DeepSeek-V2 to Kimi K2 (2025), the gated delta rule from Gated DeltaNet in Qwen3-Next (2025), and Qwen's gate in Arcee Trinity (2026).
- The table is a selection of borrowings, not a complete map.
- The author's closing claim is that competition is a good thing.

## 🏛️ For Kenji
Interpretation, not the author's: the table works as a practical map of which attention, cache and routing techniques now recur across open and closed model families, which matters when you evaluate model vendors for an architecture. Question this raises: if open-weight releases from one region slowed down, which of the techniques your systems rely on would still be maintained, and how would your model-selection criteria change?

## 📚 Reading plan
### From the article
- No link in the article text carries a URL, so none can be listed. The author cites his own pages on positional embeddings, GQA, MLA and hybrid attention, but the extracted text has no addresses for them.

### To go further
- Understanding Positional Embeddings in Modern LLMs (the author's full article) — deeper background on RoPE and related methods.
- Grouped-query attention (the author's GQA page) — how query heads share keys and values.
- Multi-head latent attention (the author's MLA page) — details of the compressed KV cache.
- Hybrid Attention (the author's page) — hybrid designs such as Gated DeltaNet.
- Open October LLM Architecture Roundup (Read Next item) — ten open-weight architecture releases from October 2026.
- Implementing LLM Architectures From Scratch (Read Next item) — comparing open-weight implementations against reference code.

## Concepts
- [Rotary position embedding (RoPE)](/concepts/ai-ml/rope.md)
- [KV cache](/concepts/ai-ml/kv-cache.md)
- [Mixture of experts](/concepts/ai-ml/mixture-of-experts.md)
- [Multi-head latent attention](/concepts/ai-ml/multi-head-latent-attention.md)
