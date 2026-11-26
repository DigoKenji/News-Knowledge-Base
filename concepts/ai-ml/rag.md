---
type: Concept
title: Retrieval-augmented generation
description: 'Retrieval-augmented generation - finding relevant documents at query time and putting them in the model''s context so it answers from that material rather than from memory alone.'
tags:
- ai-ml
- rag
domain: ai-ml
subdomain: rag
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 RAG, pp. 253-275 (architecture p. 256; retrieval algorithms p. 257; retrieval optimization p. 267)'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 4 RAG Feature Pipeline, pp. 99-176; ch. 9 RAG Inference Pipeline, pp. 317-354 (query expansion, self-querying, filtered vector search, reranking)'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; short definition added'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
---

# Retrieval-augmented generation

**Definition.** RAG gives a model external knowledge at query time: a retriever finds the most relevant chunks in a document store (term-based such as BM25, embedding-based, or hybrid), and the model answers with those chunks in its context. Huyen frames it as context construction: it reduces hallucination and lets answers use private or recent data without retraining; in her words, "finetuning is for form, and RAG is for facts" (p. 317).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `rag`.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 6 RAG, pp. 253-275 (architecture p. 256; retrieval algorithms p. 257; retrieval optimization p. 267): Term-based vs embedding-based retrievers, hybrid search, chunking and reranking.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 4 RAG Feature Pipeline, pp. 99-176; ch. 9 RAG Inference Pipeline, pp. 317-354 (query expansion, self-querying, filtered vector search, reranking): Advanced RAG split into an ingestion (feature) pipeline and an inference pipeline.

## Sources

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG, pp. 253-275 (architecture p. 256; retrieval algorithms p. 257; retrieval optimization p. 267). Term-based vs embedding-based retrievers, hybrid search, chunking and reranking.
2. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 4 RAG Feature Pipeline, pp. 99-176; ch. 9 RAG Inference Pipeline, pp. 317-354 (query expansion, self-querying, filtered vector search, reranking). Advanced RAG split into an ingestion (feature) pipeline and an inference pipeline.

## Relations
- [GraphRAG](/concepts/ai-ml/graph-rag.md) **extends** this
- [Vectorless RAG](/concepts/ai-ml/vectorless-rag.md) **alternative-to** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
