---
type: Paper Summary
title: 'MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management'
description: Consolidating agent memory records into grouped units keeps 99.0% of Mem0 accuracy with 50% fewer records.
resource: https://arxiv.org/abs/2611.26427
tags:
- ai-ml
- ai-agent
- rag
- llm-as-a-judge
- agent-memory
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26427
  title: 'MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management'
  author: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
status: stable
source_url: https://arxiv.org/abs/2611.26427
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: ai-ml
concepts:
- ai-agent
- rag
- llm-as-a-judge
- agent-memory
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d8148b197f948b6a07596
---

## 🎯 In one sentence
MemFolder shrinks an agent's memory store in two steps, merging related records into fewer retrieval units and then compressing each unit's text, and on Mem0 with 50% fewer records its RecordFolder step keeps 99.0% of the original average performance across the three text benchmarks.

## ❓ The problem
Long-running agents save every interaction as a text record in an external memory store, and retrieval pulls records back into the model's prompt for each question. Two costs grow with that store. A larger record count means more storage, a bigger index and a wider search space for each query. Longer retrieved context means more prefill (the model's work to read the input) and more attention work during decoding, on every call. The authors' uncompressed stores hold 6,817 records totaling 61,273 tokens across the ten LoCoMo samples, and 310,120 records totaling 3.7M tokens on LongMemEval. Existing fixes each attack one cost. Pruning drops records and can throw away useful evidence, while merging gives fewer records but longer units: at 50% retention on LoCoMo, MemForest and ToMe units averaged 2.12x and 2.48x the original record length. The hard part is deciding which records belong together without losing dates, names or changes of state.

## 💡 The idea
Records about the same entity or event should share one retrieval unit, and that unit's text should state shared facts once while keeping record-specific details such as dates. The authors separate two decisions: which records form a group (record level) and how to encode each group compactly (context level). The two decisions are linked by the group assignments, but each can be tuned on its own. The authors give no analogy; their central argument is that grouping cuts the number of units, compression cuts the tokens inside each unit, and together they cut both costs without the pruning loss.

## ⚙️ How it works
### 1. RecordFolder: consolidate N records into K units
RecordFolder takes the store's records with their embeddings and timestamps and maps them to K groups, where K = max{1, round(κN)} for a keep ratio κ. An LLM first extracts entities (people, works, activities, topics) and gives them short canonical labels, carrying a label registry across sessions and merging label variants with embedding similarity at least θ_ent = 0.85. Anchors are chosen one at a time: the first by density-peak clustering, later ones by novelty, so each new anchor is locally dense and far from the anchors already chosen. Records are then assigned to their nearest anchor, and Hungarian matching links records into non-branching temporal chains, keeping a link only when its gain over the record's anchor is positive. Each group is merged in one LLM call in time order, and singleton records are left as they are.

The affinity used for linking combines entity overlap with embedding similarity:

```text
affinity(i, j) = α_ij * e_ij + (1 − α_ij) * c_ij
e_ij = µ(E_i ∩ E_j) / µ(E_i ∪ E_j)
```

Here e_ij is the entity similarity between records i and j, E_i is the entity set of record i, and µ(S) is the sum of entity weights w(e) = log(N / df(e)), so rare shared entities count more. c_ij is the cosine similarity of the two record embeddings. α_ij is an adaptive mixing weight set from the signal variances, so the signal with more spread gets more weight. When µ_ij is zero, the affinity equals the cosine similarity.

### 2. ContextFolder: compress each group under a token budget
ContextFolder keeps each group's membership fixed and rewrites its records into a compact encoding. A planner (an LLM) lists atomic facts from the group, rates each as high, medium or low importance, marks facts that can be inferred from other facts, and writes a check question with an expected answer for each. A compressor writes the encoding under a budget that scales with the group's size, and a blind verifier answers the check questions from the encoding alone, while a judge compares those answers with the planner's expected answers. The loop runs at most three rounds, and the encoding is accepted when required facts pass and it fits within (1 + ε_B) times the budget. If no candidate passes, the best weighted recovery is kept, and if compression would not shorten the group, the source texts are kept as one unit.

The budget is set per group:

```text
B_G = max{3, round(τ * S_G)},    S_G = Σ_{m in G} ℓ(m)
```

B_G is the token budget for group G, τ is the target fraction of source tokens to keep, S_G is the group's total source length, and ℓ(m) is the token length of record m. The minimum budget is 3 tokens. The encoding uses a small grammar that every reader is given as a legend: @YYYY-MM-DD marks an event date, A -> B a change of state, E: x an attribute, ~ a negation and ? an uncertainty. Shared facts are written once, and the verifier and the downstream agent decode the same legend.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Mem0, 50% fewer records, average of 3 text benchmarks | MemForest: 59.3 (97.1% retained) | RecordFolder: 60.5 (99.0% retained) | Mean accuracy; % of uncompressed Mem0 |
| Mem0, 50% fewer records, LongMemEval | MemForest: 51.8 | RecordFolder: 55.0 | Overall accuracy (%) |
| Mem0, 50% fewer records, LoCoMo | MemForest: 58.4 | RecordFolder: 59.3 | Overall accuracy (%) |
| Mem0, 70% fewer records, average of 3 text benchmarks | MemForest: 57.0 (93.3% retained) | RecordFolder: 59.0 (96.6% retained) | Mean accuracy; % retained |
| Mem0, 70% fewer records, LongMemEval | MemForest: 52.2; ToMe: 49.6 | RecordFolder: 55.0 | Overall accuracy (%) |
| MemOS, 50% / 70% fewer records | Uncompressed MemOS: 100% | RecordFolder: 91.0% / 84.8% retained | % of original performance |
| M3-Agent, 50% / 70% fewer records | Uncompressed M3-Agent: 39.1 | RecordFolder: 39.6 / 40.1 (101.3% / 102.6%) | Mean accuracy, robot and web subsets |
| LongMemEval, 30% of records kept | Uncompressed: P95 97.5 ms | RecordFolder: P95 15.5 ms; mean speedup 5.34x | Retrieval latency (mean and P95) |
| PersonaMem, 30% of records kept | Uncompressed: mean 1,900 ms | RecordFolder: mean 538 ms (72% lower) | Mean retrieval latency |
| LoCoMo, 30% of records kept | Uncompressed mean latency: 100% | RecordFolder: mean latency 56% lower (2.26x) | Mean retrieval latency |

The record-level results are consistent: RecordFolder leads at each tested ratio on Mem0 and its largest margin over the best baseline is on LongMemEval (55.0 against 51.8 for MemForest at 50%), while the LoCoMo margin is 0.9 points. The ablations on LoCoMo and LongMemEval show that removing cosine similarity costs 3.4 points, the largest drop among the affinity and chaining signals, and that merging beats pruning by 6.1 points on average under the same DPC anchors. The evaluation uses gpt-4o-mini to judge answers on Mem0 and MemOS and gpt-4o on M3-Agent, and each result is averaged over three runs, but the tables report no variance.

## ⚠️ Limitations
- The paper has no dedicated limitations section. The caveats below are the ones the authors state in the text.
- The blind check is imperfect in both directions. The reader can fail to recover a fact that is present in the encoding (Appendix B.4), and an omitted fact can pass verification unnoticed; the authors call the loop an asymmetric failure-detection heuristic, not a guarantee.
- Gains are smaller in multimodal settings: the authors note that tail-latency gains on M3-Bench-robot and M3-Bench-web are smaller than on the text benchmarks, because record retrieval is only one part of latency there.
- Short, information-dense records leave little redundancy for ContextFolder to remove; on M3-Bench-robot the authors report BabelTele reached higher accuracy (32.13%) than ContextFolder at a higher token cost.
- 🔎 Added context: the paper gives two different token reductions for M3-Bench-robot, 31% in the introduction and 34.1% in Section 4.2 (20.9 against 31.7 tokens per record). Treat the exact figure as unsettled.
- 🔎 Added context: the paper does not report the cost of the offline step. Each group can take several LLM calls (grounding, one merge, planner, compressor and up to three verification rounds), and the same gpt-4o-mini model both writes the merged memories and judges answers, so the results depend on that model.

## 🏗️ What it means for building systems
Interpretation, not the authors' claim: if you run a long-lived assistant on a Mem0-style store, MemFolder would sit as an offline compaction job after writes, rewriting groups of records into units before re-embedding and re-indexing, so the online retrieval API could stay the same. Its costs are mostly LLM calls during ingestion or periodic compaction, plus re-indexing, and its latency gains only matter if retrieval is the real bottleneck. Before trusting it on your own data you would replay real traces and check that dates, negations and entity bindings survive, because the paper's blind verifier is a heuristic and not a proof. From a governance angle, the merged units replace the originals as the stored form, so you would need to decide whether raw records are kept for audit and for erasure requests.

## 🔑 Key takeaways
- At 50% fewer records on Mem0, RecordFolder averages 60.5 against 59.3 for MemForest, retaining 99.0% of the original performance against 97.1%.
- The gain is clearest on LongMemEval (55.0 against 51.8 for MemForest at 50%); on LoCoMo the margin is 0.9 points (59.3 against 58.4).
- Cosine similarity is the main assignment signal: removing it cuts the average by 3.4 points, while the entity and temporal terms add smaller gains of 1.9, 1.1 and 0.6 points.
- Density-then-novelty anchor selection (DPC) gives the best average accuracy of 57.2 among the anchor methods tested in Table 4.
- After RecordFolder halves the record count, ContextFolder cuts tokens per record by 40.3% on LoCoMo and 37.9% on LongMemEval, with limited accuracy loss.
- At 30% of records kept, mean retrieval latency speeds up 5.34x on LongMemEval, and tail latency falls from 97.5 to 15.5 ms, so the gain is not only in the average case.

## 📚 Reading plan
### From the paper
- [MemFolder code repository](https://github.com/xuyang-liu16/MemFolder) — the released implementation; compare its prompts with Appendix C before reusing any of them.
- [Token merging: Your ViT but faster (Bolya et al., 2023)](https://openreview.net/forum?id=JroZRaRw7Eu) — the ToMe merging method the paper adapts from visual tokens to records.
- [Know me, respond to me: PersonaMem (Jiang et al., 2025)](https://openreview.net/forum?id=6ox8XZGOqP) — the user-profiling benchmark behind the PersonaMem results.
### To go further
- MemForest: EventTree partitioning and progressive merging (Wang et al., 2026b) — the strongest baseline; read it to see exactly what RecordFolder changes.
- Mem0: scalable long-term memory for production AI agents (Chhikara et al., 2025) — the memory framework behind the main results.
- LoCoMo: evaluating very long-term conversational memory of LLM agents (Maharana et al., 2024) — the main text benchmark, useful for judging whether the gaps matter.
- Clustering by fast search and find of density peaks (Rodriguez and Laio, 2014) — the density-peak criterion used to pick the first anchor.

## Concepts
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [rag](/concepts/ai-ml/rag.md)
- [llm-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md)
- [agent-memory](/concepts/ai-ml/agent-memory.md)
