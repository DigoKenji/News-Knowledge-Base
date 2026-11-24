---
type: Concept
title: Grafo de conhecimento
description: Um grafo que representa conhecimento como entidades (nós) ligadas por relações tipadas e com significado (arestas), muitas vezes com um schema ou ontologia, para que fatos possam ser consultados, ligados entre fontes e usados em raciocínio.
tags:
- data
- data-engineering
domain: data
subdomain: data-engineering
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
lang: pt-br
translation_of: /concepts/knowledge-graph.md
---

# Grafo de conhecimento

**Definição.** Hogan et al. definem um grafo de conhecimento como "um grafo de dados destinado a acumular e transmitir conhecimento do mundo real, cujos nós representam entidades de interesse e cujas arestas representam relações entre essas entidades". O que o separa de um simples banco de grafos é a ênfase na semântica: identificadores compartilhados, tipos e uma ontologia que dá significado às relações.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `data-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Dois modelos de dados principais. O RDF guarda fatos como triplas sujeito-predicado-objeto com IRIs globais, consultadas com SPARQL, com schemas em RDFS/OWL que permitem inferência lógica; Wikidata e DBpedia funcionam assim. Grafos de propriedades (Neo4j, Amazon Neptune) colocam propriedades chave-valor em nós e arestas e são consultados com Cypher ou GQL, a linguagem de consulta de grafos da ISO. Grafos de conhecimento são construídos integrando dados estruturados e extraindo entidades e relações de texto, cada vez mais com LLMs. O Google popularizou o termo em 2012 ("things, not strings"). Em sistemas de IA alimentam o GraphRAG e dão aos agentes memória estruturada.

## Por que importa e trade-offs

Conectam dados entre silos e respondem perguntas de múltiplos saltos ("quais serviços dependem de uma biblioteca com CVE conhecida?") que busca por palavra-chave ou vetorial não responde. Custos: desenho e governança da ontologia, resolução de entidades (decidir que dois registros são a mesma coisa), manter o grafo atualizado e erros de extração quando construído automaticamente.

## Exemplo

A ontologia desta KB é um pequeno grafo de conhecimento: conceitos são nós e relações tipadas (is-a, part-of, uses, mitigates) são arestas, então "o que mitiga indirect prompt injection?" pode ser respondido seguindo links.

## Notas de revisão para Kenji

O rascunho dizia que grafos de conhecimento são "guardados como triplas"; isso é só o modelo RDF. A definição agora cobre grafos de propriedades também.

## Fontes

1. Hogan, A. et al. (2021). *Knowledge Graphs*. ACM Computing Surveys 54(4), arXiv:2003.02320. <https://arxiv.org/abs/2003.02320> — Definição, modelos de dados, schema, raciocínio, construção.
2. W3C (2014). *RDF 1.1 Concepts and Abstract Syntax*. W3C Recommendation. <https://www.w3.org/TR/rdf11-concepts/> — Modelo de triplas e IRIs.
3. Singhal, A. (2012). *Introducing the Knowledge Graph - things, not strings*. Google blog. <https://blog.google/products-and-platforms/products/search/introducing-knowledge-graph-things-not/> — Origem do termo popular.

## Relações
- (nenhuma ainda)

## Notas
- [Construindo um grafo de conhecimento agêntico para bilhões de eventos](/pt-br/notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md)
