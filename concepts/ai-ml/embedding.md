---
type: Concept
title: Embedding
description: A learned list of numbers (a vector) that represents an item - a word, sentence, image or user - so that items with similar meaning end up close together, letting software compare meaning with simple distance calculations.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: 'Mikolov, T. et al. (2013). Efficient Estimation of Word Representations in Vector Space. arXiv:1301.3781'
  resource: 'https://arxiv.org/abs/1301.3781'
  kind: paper
- title: 'Reimers, N. and Gurevych, I. (2019). Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks. EMNLP 2019, arXiv:1908.10084'
  resource: 'https://arxiv.org/abs/1908.10084'
  kind: paper
- title: Google. Machine Learning Crash Course - Embeddings
  resource: 'https://developers.google.com/machine-learning/crash-course/embeddings'
  kind: docs
---

# Embedding

**Definition.** An embedding maps a discrete or complex object into a continuous vector space of a few hundred to a few thousand dimensions. The mapping is learned, so geometry carries meaning: distance (often cosine similarity) reflects similarity, and directions can encode relations. Word2vec (2013) made word embeddings famous; today transformer encoders produce embeddings for whole sentences and documents.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Embeddings are the bridge between unstructured data and computation: they power semantic search, RAG retrieval, recommendation, clustering and deduplication, and they are the internal representation inside every neural network. Quality depends on the embedding model and the domain; embeddings from different models are not comparable, so changing models means re-embedding everything.

## Example

"Car" and "automobile" get vectors close together, far from "banana"; in word2vec, vector("king") − vector("man") + vector("woman") lands near vector("queen").

## Narrower concepts

- [Semantic search](/concepts/ai-ml/semantic-search.md)
- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [GraphRAG](/concepts/ai-ml/graph-rag.md)
- [Knowledge graph](/concepts/data/knowledge-graph.md)

## Sources

1. Mikolov, T. et al. (2013). *Efficient Estimation of Word Representations in Vector Space*. arXiv:1301.3781. <https://arxiv.org/abs/1301.3781> — Word2vec and vector arithmetic on word embeddings.
2. Reimers, N. and Gurevych, I. (2019). *Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks*. EMNLP 2019, arXiv:1908.10084. <https://arxiv.org/abs/1908.10084> — Sentence-level embeddings for similarity.
3. Google. *Machine Learning Crash Course - Embeddings*. <https://developers.google.com/machine-learning/crash-course/embeddings> — Practitioner introduction to embedding spaces.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
