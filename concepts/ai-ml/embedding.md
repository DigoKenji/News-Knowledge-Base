---
type: Concept
title: Embedding
description: A learned list of numbers (a vector) that represents an item - a word, sentence, image or user - so that items with similar meaning end up close together, letting software compare meaning with simple distance calculations.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 3 Introduction to Embedding, pp. 134-136; ch. 6 embedding-based retrieval, p. 257'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 9 Using Embeddings as a Lookup Table, p. 275'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 15 Embedding layers for sentence encoding, p. 517'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Embedding

**Definition.** An embedding maps a discrete or complex object into a continuous vector space of a few hundred to a few thousand dimensions. The mapping is learned, so geometry carries meaning: distance (often cosine similarity) reflects similarity, and directions can encode relations. Word2vec (2013) made word embeddings famous; today transformer encoders produce embeddings for whole sentences and documents.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Embeddings are the bridge between unstructured data and computation: they power semantic search, RAG retrieval, recommendation, clustering and deduplication, and they are the internal representation inside every neural network. Quality depends on the embedding model and the domain; embeddings from different models are not comparable, so changing models means re-embedding everything.

## Example

"Car" and "automobile" get vectors close together, far from "banana"; in word2vec, vector("king") − vector("man") + vector("woman") lands near vector("queen").

## Narrower concepts

- [Semantic search](/concepts/ai-ml/semantic-search.md)
- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [GraphRAG](/concepts/ai-ml/graph-rag.md)
- [Knowledge graph](/concepts/data/knowledge-graph.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 3 Introduction to Embedding, pp. 134-136; ch. 6 embedding-based retrieval, p. 257: Embeddings for similarity measurement and retrieval.
- **Tunstall, L., von Werra, L. and Wolf, T. (2022)**, *Natural Language Processing with Transformers, revised edition*, ch. 9 Using Embeddings as a Lookup Table, p. 275: Embeddings as a zero-label classification trick.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 15 Embedding layers for sentence encoding, p. 517: How an embedding layer is learned inside a network.

## Sources

1. Mikolov, T. et al. (2013). *Efficient Estimation of Word Representations in Vector Space*. arXiv:1301.3781. <https://arxiv.org/abs/1301.3781> — Word2vec and vector arithmetic on word embeddings.
2. Reimers, N. and Gurevych, I. (2019). *Sentence-BERT - Sentence Embeddings using Siamese BERT-Networks*. EMNLP 2019, arXiv:1908.10084. <https://arxiv.org/abs/1908.10084> — Sentence-level embeddings for similarity.
3. Google. *Machine Learning Crash Course - Embeddings*. <https://developers.google.com/machine-learning/crash-course/embeddings> — Practitioner introduction to embedding spaces.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 3 Introduction to Embedding, pp. 134-136; ch. 6 embedding-based retrieval, p. 257. Embeddings for similarity measurement and retrieval.
5. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 9 Using Embeddings as a Lookup Table, p. 275. Embeddings as a zero-label classification trick.
6. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 15 Embedding layers for sentence encoding, p. 517. How an embedding layer is learned inside a network.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
