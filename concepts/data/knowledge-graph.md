---
type: Concept
title: Knowledge graph
description: A graph that represents knowledge as entities (nodes) connected by typed, meaningful relations (edges), often with a schema or ontology, so that facts can be queried, linked across sources and reasoned over.
tags:
- data
- data-science
domain: data
subdomain: data-science
also_in:
- ai-ml/rag
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Hogan, A. et al. (2021). Knowledge Graphs. ACM Computing Surveys 54(4), arXiv:2003.02320'
  resource: 'https://arxiv.org/abs/2003.02320'
  kind: paper
- title: W3C (2014). RDF 1.1 Concepts and Abstract Syntax. W3C Recommendation
  resource: 'https://www.w3.org/TR/rdf11-concepts/'
  kind: standard
- title: Singhal, A. (2012). Introducing the Knowledge Graph - things, not strings. Google blog
  resource: 'https://blog.google/products-and-platforms/products/search/introducing-knowledge-graph-things-not/'
  kind: practitioner
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 2 Data Models and Query Languages: Graph-Like Data Models (property graphs, triple-stores, Cypher, SPARQL)'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Knowledge graph

**Definition.** Hogan et al. define a knowledge graph as "a graph of data intended to accumulate and convey knowledge of the real world, whose nodes represent entities of interest and whose edges represent relations between these entities". What separates it from a plain graph database is the emphasis on semantics: shared identifiers, types and an ontology that gives the relations meaning.

Domain: [data](/domains/data.md) · subdomain `data-science`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Two main data models. RDF stores facts as subject-predicate-object triples with global IRIs, queried with SPARQL, with schemas in RDFS/OWL that allow logical inference; Wikidata and DBpedia work this way. Property graphs (Neo4j, Amazon Neptune) put key-value properties on nodes and edges and are queried with Cypher or GQL, the ISO graph query language. Knowledge graphs are built by integrating structured data and by extracting entities and relations from text, increasingly with LLMs. Google popularised the term in 2012 ("things, not strings"). In AI systems they power GraphRAG and give agents structured memory.

## Why it matters and trade-offs

They connect data across silos and answer multi-hop questions ("which services depend on a library with a known CVE?") that keyword or vector search cannot. Costs: ontology design and governance, entity resolution (deciding two records are the same thing), keeping the graph current, and extraction errors when built automatically.

## Example

This KB's ontology is a small knowledge graph: concepts are nodes and typed relations (is-a, part-of, uses, mitigates) are edges, so "what mitigates indirect prompt injection?" can be answered by following links.

## Review notes for Kenji

The draft said knowledge graphs are "stored as triples"; that is only the RDF model. The definition now covers property graphs too.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 2 Data Models and Query Languages: Graph-Like Data Models (property graphs, triple-stores, Cypher, SPARQL): The property-graph and triple-store models that knowledge graphs are built on.

## Sources

1. Hogan, A. et al. (2021). *Knowledge Graphs*. ACM Computing Surveys 54(4), arXiv:2003.02320. <https://arxiv.org/abs/2003.02320> — Definition, data models, schema, reasoning, construction.
2. W3C (2014). *RDF 1.1 Concepts and Abstract Syntax*. W3C Recommendation. <https://www.w3.org/TR/rdf11-concepts/> — Triple model and IRIs.
3. Singhal, A. (2012). *Introducing the Knowledge Graph - things, not strings*. Google blog. <https://blog.google/products-and-platforms/products/search/introducing-knowledge-graph-things-not/> — Origin of the popular term.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 2 Data Models and Query Languages: Graph-Like Data Models (property graphs, triple-stores, Cypher, SPARQL). The property-graph and triple-store models that knowledge graphs are built on.

## Relations
- (none yet)

## Notes
- [Building an Agentic Knowledge Graph for Billions of Events](/notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md)
