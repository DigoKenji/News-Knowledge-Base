---
type: Concept
title: Parent-child chunking
description: A RAG retrieval pattern that indexes small child chunks for precise matching but returns the larger parent section they belong to, so the model gets enough surrounding context.
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
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: LangChain. ParentDocumentRetriever (API reference)
  resource: 'https://reference.langchain.com/python/langchain-classic/retrievers/parent_document_retriever/ParentDocumentRetriever'
  kind: docs
- title: LlamaIndex. Auto Merging Retriever
  resource: 'https://developers.llamaindex.ai/python/framework/integrations/retrievers/auto_merging_retriever/'
  kind: docs
- title: 'Lewis, P. et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS 2020, arXiv:2005.11401'
  resource: 'https://arxiv.org/abs/2005.11401'
  kind: paper
relations:
- type: part-of
  target: /concepts/ai-ml/rag.md
---

# Parent-child chunking

**Definition.** Parent-child chunking (also called small-to-big retrieval, or the parent document retriever) splits each document twice: into large parent chunks (sections, pages) and, inside each, small child chunks (sentences, short paragraphs). Only the children are embedded and searched; when a child matches, the system returns its parent to the LLM.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `rag`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

It resolves a tension in chunk size. Small chunks give sharp embeddings, because one idea per vector matches queries well, but lack context. Large chunks carry context but their embeddings blur several topics, so they match worse. Storing a child-to-parent mapping lets you search small and read big. Variants: LlamaIndex's auto-merging retriever returns the parent only when enough of its children match; sentence-window retrieval returns a fixed window of sentences around the hit; deduplicate when several children share a parent.

## Why it matters and trade-offs

It usually improves answer quality for long, structured documents (manuals, standards, contracts) at little cost. It uses more context tokens per hit, needs a document store beside the vector index, and depends on parents being meaningful units; a bad section split gives bad parents.

## Example

Index each sentence of the TOGAF PDF, but when "What is a building block?" matches one sentence, send the model the whole subsection it belongs to.

## Review notes for Kenji

This is an engineering pattern documented by frameworks, not a research result; the sources are two independent framework docs plus the original RAG paper for the underlying retrieval idea.

## Sources

1. LangChain. *ParentDocumentRetriever* (API reference). <https://reference.langchain.com/python/langchain-classic/retrievers/parent_document_retriever/ParentDocumentRetriever> — Small chunks for embedding, parent documents returned; child-parent store.
2. LlamaIndex. *Auto Merging Retriever*. <https://developers.llamaindex.ai/python/framework/integrations/retrievers/auto_merging_retriever/> — Hierarchical chunks and merging children into parents.
3. Lewis, P. et al. (2020). *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*. NeurIPS 2020, arXiv:2005.11401. <https://arxiv.org/abs/2005.11401> — The RAG architecture this pattern refines.

## Relations
- **part-of** [Retrieval-augmented generation](/concepts/ai-ml/rag.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
