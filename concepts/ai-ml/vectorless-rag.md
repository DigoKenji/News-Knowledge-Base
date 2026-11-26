---
type: Concept
title: Vectorless RAG
description: 'RAG that retrieves without an embedding index - for example by letting the LLM navigate a document''s structure or table of contents, or by keyword search - to keep context and avoid chunking errors.'
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
  locator: 'ch. 6 RAG and Agents: Retrieval algorithms (term-based versus embedding-based retrieval)'
- title: 'Robertson, S. and Zaragoza, H. (2009). The Probabilistic Relevance Framework: BM25 and Beyond. Foundations and Trends in IR 3(4)'
  resource: 'https://doi.org/10.1561/1500000019'
  kind: paper
- title: 'PageIndex (Vectify AI). PageIndex documentation - vectorless, reasoning-based RAG'
  resource: 'https://docs.pageindex.ai/'
  kind: docs
relations:
- type: alternative-to
  target: /concepts/ai-ml/rag.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Vectorless RAG

**Definition.** Vectorless RAG retrieves context without an embedding index. One variant uses classic term-based retrieval (BM25); a newer one lets the LLM navigate a document's own structure - a generated tree or table of contents - and reason about which sections to open, as PageIndex does. The goal is retrieval that follows how a person reads long, structured documents and that can cite exactly where an answer came from.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `rag`. Expanded on 2026-11-26 with independent sources.

## How it fits

It is an alternative retrieval strategy inside RAG, not a replacement for retrieval. It avoids chunking that cuts tables or sections apart and embedding similarity that misses exact terms, but tree navigation costs more LLM calls per query and works best on well-structured documents such as reports, manuals and filings.

## Example

For a 300-page annual report, the system builds a section tree once; asked about lease liabilities, the LLM walks Notes to the financial statements → Leases and quotes that page.

## Sources

1. Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG and Agents: Retrieval algorithms (term-based versus embedding-based retrieval). Term-based retrieval as a strong, cheap alternative to embeddings.
2. Robertson, S. and Zaragoza, H. (2009). The Probabilistic Relevance Framework: BM25 and Beyond. Foundations and Trends in IR 3(4). <https://doi.org/10.1561/1500000019> — BM25, the standard term-based ranking function.
3. PageIndex (Vectify AI). PageIndex documentation - vectorless, reasoning-based RAG. <https://docs.pageindex.ai/> — Tree-search retrieval over a document's structure.

## Relations
- **alternative-to** [Retrieval-augmented generation](/concepts/ai-ml/rag.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
