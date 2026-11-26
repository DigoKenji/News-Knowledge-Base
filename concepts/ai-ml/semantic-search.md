---
type: Concept
title: Semantic search
description: Search that matches the meaning of a query rather than its exact words, usually by turning queries and documents into embedding vectors and returning the nearest ones.
tags:
- ai-ml
- rag
domain: ai-ml
subdomain: rag
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Reimers, N. and Gurevych, I. (2019). Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks. EMNLP 2019, arXiv:1908.10084'
  resource: 'https://arxiv.org/abs/1908.10084'
  kind: paper
- title: 'Karpukhin, V. et al. (2020). Dense Passage Retrieval for Open-Domain Question Answering. EMNLP 2020, arXiv:2004.04906'
  resource: 'https://arxiv.org/abs/2004.04906'
  kind: paper
- title: 'Thakur, N. et al. (2021). BEIR - A Heterogeneous Benchmark for Zero-shot Evaluation of Information Retrieval Models. NeurIPS 2021, arXiv:2104.08663'
  resource: 'https://arxiv.org/abs/2104.08663'
  kind: paper
- title: Manning, C., Raghavan, P. and Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press
  resource: 'https://nlp.stanford.edu/IR-book/'
  kind: book
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Semantic search

**Definition.** Semantic search retrieves documents that are about the same thing as the query even when they share no words with it. Modern systems use dense retrieval: a neural encoder maps text to vectors such that similar meanings land close together, and an approximate nearest-neighbour index finds the closest documents.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `rag`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Pipeline: chunk documents, embed each chunk with a bi-encoder (Sentence-BERT style), store vectors in an index (HNSW, IVF) and, at query time, embed the query and fetch the top-k by cosine similarity. Lexical search (BM25) instead scores exact term overlap. Each fails differently: dense retrieval handles synonyms and paraphrase but can miss exact codes, names and rare terms; BM25 is the reverse. The BEIR benchmark showed BM25 remains a strong baseline out of domain. Production systems therefore often use hybrid search (both, fused with reciprocal rank fusion) plus a cross-encoder reranker.

## Why it matters and trade-offs

It is the retrieval layer behind most RAG systems and makes search work for natural-language questions. Costs: embedding and index infrastructure, sensitivity to the embedding model and chunking, and results that are harder to explain than keyword matches.

## Example

Query "how to stop retries from making an outage worse" finds the note on retry storms even though it never says "stop" or "outage".

## Sources

1. Reimers, N. and Gurevych, I. (2019). *Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks*. EMNLP 2019, arXiv:1908.10084. <https://arxiv.org/abs/1908.10084> — Bi-encoder sentence embeddings for semantic similarity search.
2. Karpukhin, V. et al. (2020). *Dense Passage Retrieval for Open-Domain Question Answering*. EMNLP 2020, arXiv:2004.04906. <https://arxiv.org/abs/2004.04906> — Dense retrieval beating BM25 on in-domain QA.
3. Thakur, N. et al. (2021). *BEIR - A Heterogeneous Benchmark for Zero-shot Evaluation of Information Retrieval Models*. NeurIPS 2021, arXiv:2104.08663. <https://arxiv.org/abs/2104.08663> — BM25 strength out of domain; the case for hybrid search.
4. Manning, C., Raghavan, P. and Schütze, H. (2008). *Introduction to Information Retrieval*. Cambridge University Press. <https://nlp.stanford.edu/IR-book/> — Lexical retrieval fundamentals (term weighting, vector space model) for contrast.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
