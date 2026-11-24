---
type: Paper Summary
title: 'Jetriever: Shifting Agent Memory Retrieval from Similarity to Utility'
description: Ranking stored memories by an LLM decision model's usefulness score finds indirectly relevant memories that similarity search misses.
resource: https://arxiv.org/abs/2611.21478
tags:
- ai-ml
- ai-agents
- rag
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.21478
  title: 'Jetriever: Shifting Agent Memory Retrieval from Similarity to Utility'
  author: Ruizhe Li, Licheng Zhang, Mingxuan Du, Zhendong Mao
status: stable
source_url: https://arxiv.org/abs/2611.21478
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-16'
domain: ai-ml
subdomain: ai-agents
concepts:
- rag
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d810d9be8fab230cc53cb
---

# Jetriever: Shifting Agent Memory Retrieval from Similarity to Utility

**Source:** https://arxiv.org/abs/2611.21478
**Author:** Ruizhe Li, Licheng Zhang, Mingxuan Du, Zhendong Mao
**Date:** 2026-11-16
**Concepts:** [Retrieval-augmented generation](/concepts/ai-ml/rag.md) · [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)

---

## 🎯 In one sentence
Jetriever scores each stored memory with the Jev decision model for its usefulness to the current question and ranks by that score, placing the gold exchange first in 84.8% of 125 InMind tasks, where matched BM25 and MiniLM controls reach 1.6% and 0.0%.

## ❓ The problem
Most agent memory systems retrieve whatever text resembles the question, using keyword scoring such as BM25 (a classical formula that rewards query words appearing in a document) or embedding similarity (cosine distance between vectors from a text encoder such as MiniLM). That works when the useful memory shares words or entities with the query, but it fails when the link is world knowledge: a stored nut allergy matters for a macaron-recipe request that never mentions nuts, because macarons commonly contain almond flour. The InMind benchmark shows the cost: a backbone given the decisive memory directly answers 84.0% of indirect queries, while the six tested memory systems reach at most 14.4% end-to-end. Asking an LLM to guess search terms first does not fix this, because the space of hidden associations is too large to enumerate and the answering model never sees a memory the retriever did not return. The hard part is scoring every stored memory for usefulness at a cost low enough to cover the whole pool.

## 💡 The idea
Instead of asking which memory sounds like the question, Jetriever asks, for each real memory, whether it would help answer the question. The authors say this needs three things at once: broad world knowledge to connect a memory to a query through an unstated fact, scores that separate decisive evidence from merely plausible evidence (a retriever cannot rank hundreds of memories if it calls nearly all of them useful), and scoring cheap enough to run over the whole pool. Jev, a decision model built on pretrained LLMs and trained for calibrated decisions with RLCD (a method the paper cites to TypeSafe), is meant to meet all three, and its utility score is used directly as the retrieval score. The authors' one-line summary is "Find the memory that matters to the answer, not just one that sounds similar."

## ⚙️ How it works
### Per-pair utility decision (Jev)
For a query q and one memory candidate m_i, the system sends only the pair to Jev and requests a binary judgment, which the paper calls a "Noul" judgment without defining the term further. For InMind the criterion asks whether the memory chunk contains personal information needed for a correct, personalized response, including indirectly relevant information, and tells Jev to judge only what that chunk contributes. For the broader MAB tasks the criterion asks whether the memory contains information needed to answer correctly, including directly stated facts and necessary supporting information. Jev returns a score s(q, m_i) that is used only as a ranking signal, and the paper notes these scores are rounded to two decimals. Jev produces structured decisions and probabilities rather than free-form text, which is part of why the calls are cheap.

### Ranking rule
```text
pi_q = argsort_i ( -s(q, m_i), id(m_i) )
M_k(q) = { m_pi_q(1), ..., m_pi_q(k) }
```
Here q is the query, m_i is the i-th candidate memory and s(q, m_i) is Jev's score for that pair. argsort_i returns candidate indices ordered by the key pair: negative score first, so higher scores come first, then the memory identifier. pi_q is the resulting ordering for query q, and M_k(q) is the set of its top k memories. The identifier exists only to break ties, which occur because scores are rounded. The answerer sees the top k memories but not Jev's score or any generated rationale.

### Full-pool execution
Every candidate gets its own Jev call, so N candidates mean N independent decisions with no approximate nearest-neighbour index. The paper treats parallelism as what makes this practical: the InMind run averages 518 ms per candidate API request with up to 50 concurrent requests. A separate LongMemEval run scores 6,446 query-exchange pairs in 90.16 s at 100-way concurrency, about 71.5 scores per second. Reported cost is $0.00834 per InMind question (241 candidates) and $0.03103 on average per MAB question (mean 1,258.2 conversations). The authors stress that the 90.16 s figure measures batch throughput, not per-question online latency.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| InMind-125, 241 exchanges per query | BM25, user turn: 1.6 | Jetriever: 84.8 | R@1 (%) |
| InMind-125, 241 exchanges per query | MiniLM, user turn: 0.0 | Jetriever: 84.8 | R@1 (%) |
| InMind-125, 241 exchanges per query | BM25, user turn: 4.8 | Jetriever: 92.0 | R@5 (%) |
| InMind-125, 241 exchanges per query | MiniLM, user turn: 8.8 | Jetriever: 94.4 | R@10 (%) |
| InMind-125, end-to-end Application | text-embedding-3-large: 16.0 | Jetriever, top five: 76.0 | Application (%) |
| InMind-125, end-to-end Application | Backbone, target fact in context: 84.0 | Jetriever, top five: 76.0 | Application (%) |
| 20 InMind questions, 241 candidates | Qwen3.5 9B: 40 (8 of 20) | Jev: 85 (17 of 20) | R@1 (%) |
| 20 InMind questions, 241 candidates | Qwen3.5 9B: 90 (18 of 20) | Jev: 90 (18 of 20) | R@50 (%) |
| 20 InMind questions, 241 candidates | Laya: 30 (6 of 20) | Jev: 90 (18 of 20) | R@50 (%) |
| MAB, 60-question sample | ReFind: 44.3 | Jetriever: 55.4 | Four-dimension macro average (%) |
| InMind-125, retrieval API cost | not applicable | $0.00834 per question | USD, retrieval only |
| MAB 60-question sample, retrieval API cost | not applicable | $0.03103 per question | USD, retrieval only |

The InMind results are the core evidence: on identical 241-exchange pools, Jetriever places the gold exchange first in 106 of 125 tasks and within five in 115, while the user-turn controls reach 6 and 3 of 125 at rank five. Changing the candidate unit from 241 exchanges to 47 sessions drops R@1 from 84.8% to 49.6%, and the paper reads this as a long session burying the fact. On MAB, the sampled macro average is 55.4% against 44.3% for ReFind, with the four dimensions at AR 65.5 vs 49.8, TTL 40.0 vs 30.0, LRU 59.0 vs 55.5 and SF 57.1 vs 41.7. The results do not show behaviour on larger stores, per-question latency, or a comparison run through the same answer pipeline.

## ⚠️ Limitations
- The authors state that the MAB sample is 60 of 3,671 questions with no SH-QA questions, so its averages are not the official full-benchmark scores.
- The authors state that the ReFind comparison is system-level: ReFind's answers come from an earlier run and were not rerun with Jetriever's answer pipeline.
- The authors state that full-pool scoring is practical at the tested sizes but not free at any scale, and that a cheaper first-stage cascade, per-question latency distributions and direct calibration tests remain untested.
- The authors describe the paper as "a modest paper" that does not clear the bar for a mainstream AI conference, and mark it as a research draft.
- 🔎 Added context: the InMind setup plants one decisive memory among 240 background exchanges. Real stores may hold several near-duplicate or contradictory memories, and the paper does not test ranking under those conditions.
- 🔎 Added context: InMind is the same group's earlier benchmark (reference 1 shares three authors with this paper), so the main evidence uses a benchmark the authors designed for the phenomenon they study.

## 🏗️ What it would mean for building systems
This is interpretation, not the authors' claim. Jetriever would sit as a rerank stage over a candidate memory set: for a per-user store of a few hundred items, the reported cost per query is small next to an answer call, but a store with tens of thousands of memories would multiply decisions and latency. The authors suggest a cheap first stage, but it must keep indirect candidates, which lexical or embedding filters may drop. Before trusting this you would need your own gold-labelled set of indirect queries, a check that scores separate candidates in your domain, and latency measured per request at production concurrency, since the paper does not supply that. The question it raises for you is which of your memory stores hold facts that matter through world knowledge rather than shared words, and whether a missed memory is costly enough to justify per-item LLM scoring.

## 🔑 Key takeaways
- Similarity retrieval fails when the link to the query is world knowledge: with user-turn indexing, BM25 places the gold exchange first in 1.6% of InMind tasks and MiniLM in 0.0%.
- Jetriever ranks each stored memory with Jev's binary judgment score and reaches 84.8% at rank one (106 of 125), 92.0% within five and 94.4% within ten.
- Finding the gold memory in the top 50 is not the same as ranking it first: Jev and Qwen3.5 9B both find 90% by rank 50, but at rank one Jev reaches 85% and Qwen3.5 9B 40%.
- The unit of scoring matters: R@1 falls from 84.8% for 241 exchanges to 49.6% for 47 sessions.
- On a 60-question MAB sample the four-dimension macro average is 55.4%, against 44.3% for ReFind on the same questions.
- Retrieval cost is small at these pool sizes ($0.00834 per InMind question, $0.03103 per MAB question), but the paper leaves scaling to much larger stores open.

## 📚 Reading plan
### From the paper
- [Keep It InMind (reference 1)](https://arxiv.org/abs/2607.24368) — the benchmark behind the 125 tasks and the blind-spot argument.
- [Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions (reference 2)](https://arxiv.org/abs/2507.05257) — MemoryAgentBench, the second benchmark.
- [When Your Agent Opens the Chat App (reference 3)](https://arxiv.org/abs/2608.12888) — ReFind, the source of the MAB comparison.
- [BM25 Wins at Scale (reference 4)](https://arxiv.org/abs/2607.26497) — evidence that BM25 is a strong RAG baseline.
- [Is Complex Agentic Memory Always Needed? (reference 5)](https://arxiv.org/abs/2610.30755) — naive RAG versus memory systems.
- [A-RAG (reference 7)](https://arxiv.org/abs/2602.03442) — agent-chosen keyword and semantic search, a baseline.
- [TypeSafe, AI primer: RLCD and calibrated decisions (reference 8)](https://docs.typesafe.ai/introduction/machine-learning-primer) — how Jev is trained for calibrated decisions.
- [Introducing System One Models & Jev (reference 9)](https://typesafe.ai/blog/introducing-system-one-models-and-jev) — the decision model Jetriever uses.
- [Laya Typed-Decisions model card (reference 10)](https://huggingface.co/convaiinnovations/laya-typed-decisions) — the BERT-based scorer in the 20-question comparison.
- [Qwen3.5-9B model card (reference 11)](https://huggingface.co/Qwen/Qwen3.5-9B) — the LLM scorer control.
- [all-MiniLM-L6-v2 model card (reference 16)](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) — the similarity control's encoder.
- [LongMemEval (reference 20)](https://arxiv.org/abs/2410.10813) — the long-term memory benchmark used in the batch throughput run.
- [Jetriever repository](https://github.com/imlrz/Jetriever) — the project's code and data, as linked from the paper's cover page.

### To go further
- The Probabilistic Relevance Framework: BM25 and Beyond (Robertson and Zaragoza, 2009) — why keyword scoring is such a strong baseline.
- Calibration of LLM decision probabilities — the paper says direct calibration tests are still needed.
- Cascade retrieval: a cheap first stage before expensive per-item scoring — the paper's proposed next experiment.
- Approximate nearest-neighbour indexes — the alternative the paper's complete-reranking design avoids.
