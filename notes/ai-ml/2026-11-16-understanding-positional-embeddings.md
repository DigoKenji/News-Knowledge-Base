---
type: Article Summary
title: Understanding Positional Embeddings in Modern LLMs
description: Raschka traces position encoding from learned tables and sinusoids to RoPE, then how 2026 open models mix partial RoPE, NoPE layers and learned bias.
resource: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
tags:
- ai-ml
- llm-engineering
- rope
- kv-cache
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
  title: Understanding Positional Embeddings in Modern LLMs
  author: Sebastian Raschka
status: stable
source_url: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
source: Ahead of AI
source_type: blog
published: '2026-11-15'
domain: ai-ml
subdomain: llm-engineering
concepts:
- rope
- kv-cache
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d814881fcc33b2038e53a
---

# Understanding Positional Embeddings in Modern LLMs

**Source:** https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
**Author:** Sebastian Raschka, PhD
**Date:** 2026-11-15
**Tags:** #llm #architecture #positional-embeddings #rope #attention

---

## 💬 What This Is About
Raschka explains how LLMs encode token position, from the learned absolute embeddings in GPT-2 to [RoPE](/concepts/rope.md) and the choices recent open-weight models make. It is a deliberately technical foundation for a later architecture-trends article, with visual examples, some math and a small PyTorch implementation.

## 1. Where position information enters an LLM
Position information is added at different points. GPT-2 adds a position vector right after the token embedding. MiniMax-M3 injects it inside attention in each block, using RoPE. Plain self-attention can't tell "The red fox jumps over the gray fox" from its swapped version, since the tokens and embeddings are identical. A causal mask supplies some implicit order, but it can't distinguish a gap of one position from ten, so explicit signals still help.

## 2. Adding a position vector to each token embedding

### 2.1 Learned absolute position embeddings in GPT-2
GPT-2 learns one vector per position (1,024 of them) and adds it to the token embedding. The context length is fixed before training, and extending it is costly. Positional weights are 0.30% of GPT-2 Medium's total at 1,024 positions but would exceed 22% at a 100k-token context.

### 2.2 Fixed sinusoidal position encodings in the original transformer
The 2017 transformer adds fixed sine and cosine vectors instead. They can be computed beyond the training length, though that doesn't guarantee good quality there. The paper hypothesized that this structure makes relative offsets easy to learn.

## 3. Encoding relative position with RoPE
[RoPE](/concepts/rope.md) (Su et al., 2021) keeps the sinusoidal idea but moves it into attention. Queries and keys are rotated by an angle set by their position, while values bypass the rotation. The dot product then depends only on the offset between positions. In the worked example, shifting both tokens by three positions leaves the score at 13, while moving only the key one position closer changes it to about 19.92.

## 4. Computing rotation frequencies
Each feature pair gets a frequency θᵢ = b^(−2i/d), with base b = 10,000 in the RoFormer paper. Several frequencies let some pairs change quickly with position and others slowly. For head dimension 8, this gives [1, 0.1, 0.01, 0.001] radians per position.

## 5. Implementing RoPE in PyTorch
The cosine and sine tables are computed once per model and broadcast across batch and heads. Only queries and keys pass through RoPE. The code confirms that shifting all positions by 3 leaves attention scores unchanged. The original RoPE paper and LLaMA pair features in an interleaved layout, while most code uses split-half, which is equivalent after reordering and easier to vectorize.

## 6. Positional choices in modern architectures
Full RoPE in every block has been a staple of open-weight models since LLaMA, but many newer designs vary it.

### 6.1 Partial RoPE
Partial RoPE rotates only part of each head. MiniMax-M3 rotates 64 of 128 dimensions, halving the cosine and sine tables. The [KV cache](/concepts/kv-cache.md) still stores full keys and values, so the saving is modest. Raschka calls partial RoPE "hardly worth it" in his opinion, citing a paper where about 10% of features often matched full RoPE.

### 6.2 NoPE layers
NoPE uses no explicit positional encoding, relying on the causal mask. In a toy example, the first token's weight shrinks as the prefix grows, which a later layer can read as position. A 107M NoPE model ranked first on length generalization across ten tasks, but a 1.3B code model degraded at the longest lengths. SmolLM3 (3B) applies RoPE in three of every four blocks, and its authors reported better long-context performance.

### 6.3 Learned relative position bias
Thinky's Inkling adds a learned number to each scaled query-key score before softmax, depending on the query's hidden state and the distance back to the key. T5 used a simpler version in 2019.

### 6.4 How these choices appear in recent models
In the author's gallery, full RoPE in every softmax layer is still the most common choice, but its share fell from 2025 to 2026. Mixed RoPE/NoPE designs are growing, and partial RoPE went from a minority of RoPE models in 2025 to a majority in 2026. The groups are small, so these trends are tentative.

## Patterns & Techniques Spotted
- **Learned absolute embedding table:** one vector per index, simple but tied to a fixed context length.
- **Rotation inside attention (RoPE):** queries and keys rotated by position, so scores depend on relative offset.
- **Precomputed cos/sin tables:** computed once and reused across batch, heads and queries.
- **Hybrid RoPE/NoPE layering:** RoPE skipped in some blocks, leaning on the causal mask there.
- **Learned relative bias:** a distance-dependent, query-conditioned term added before softmax.

## Key Takeaways
- The causal mask gives some order information, but explicit position signals still help attention judge distance.
- RoPE's key property is that attention scores depend only on relative offset, so a shared prefix shift changes nothing.
- Learned absolute tables are costly to extend; rotation-based methods avoid that cost.
- RoPE remains the most common choice, but partial RoPE and RoPE/NoPE mixes are growing.

## Follow-up Reading
- 📄 RoFormer (Su et al., 2021), no URL in source text → the original RoPE paper, which the article builds on.
- 📄 Attention Is All You Need (2017), no URL in source text → the source of the sinusoidal encodings and the relative-offset hypothesis.
- 📚 The Impact of Positional Encoding on Length Generalization in Transformers, no URL in source text → the NoPE paper and its length-generalization experiments.
- 🔗 Fractional Rotation, Full Potential?, no URL in source text → the study of partial RoPE at 1B to 8B scale.
- 📚 Understanding and Coding the KV Cache in LLMs from Scratch (Raschka), no URL in source text → useful context on how caching affects long-context memory.

---
*Summary generated: 2026-11-16*
