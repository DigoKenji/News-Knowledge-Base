---
type: Concept
title: GraphRAG
description: 'Retrieval-augmented generation that retrieves from a knowledge graph of entities and relations (often built by an LLM from the documents) instead of, or alongside, text chunks.'
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
- title: 'Edge, D. et al. (2024). From Local to Global: A Graph RAG Approach to Query-Focused Summarization. arXiv:2404.16130'
  resource: 'https://arxiv.org/abs/2404.16130'
  kind: paper
- title: 'Microsoft. GraphRAG documentation'
  resource: 'https://microsoft.github.io/graphrag/'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 RAG and Agents: RAG architecture and retrieval algorithms'
relations:
- type: extends
  target: /concepts/ai-ml/rag.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# GraphRAG

**Definition.** GraphRAG builds a knowledge graph from the corpus - an LLM extracts entities and relations, the graph is clustered into communities and each community is summarised - and answers questions by retrieving from that structure. Microsoft's 2024 paper shows it helps with global questions ('what are the main themes?') that plain chunk retrieval answers poorly, because no single chunk holds the answer.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `rag`. Expanded on 2026-11-26 with independent sources.

## How it fits

It extends ordinary RAG and trades a costly indexing step (many LLM calls) for better multi-hop and summary answers. Local search starts from entities that match the query; global search maps over community summaries and reduces the partial answers.

## Example

Over a year of incident reports, 'which services are most often involved in cascading failures?' is answered from the graph's service and dependency communities rather than from the few reports that happen to match the words.

## Sources

1. Edge, D. et al. (2024). From Local to Global: A Graph RAG Approach to Query-Focused Summarization. arXiv:2404.16130. <https://arxiv.org/abs/2404.16130> — The GraphRAG method and its evaluation on global questions.
2. Microsoft. GraphRAG documentation. <https://microsoft.github.io/graphrag/> — Indexing pipeline, local and global search.
3. Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG and Agents: RAG architecture and retrieval algorithms. Where graph-based retrieval sits among RAG retrieval options.

## Relations
- **extends** [Retrieval-augmented generation](/concepts/ai-ml/rag.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
