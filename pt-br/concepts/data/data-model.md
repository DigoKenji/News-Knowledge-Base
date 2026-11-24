---
type: Concept
title: Modelo de dados (relacional, documento, grafo)
description: A forma como os dados são estruturados e relacionados para serem armazenados e consultados - mais comumente como tabelas relacionais, documentos aninhados ou grafos de nós e arestas - cada um adequado a formatos diferentes de dados e consultas.
tags:
- data
- core
domain: data
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Codd, E. F. (1970). A Relational Model of Data for Large Shared Data Banks. Communications of the ACM 13(6)
  resource: 'https://doi.org/10.1145/362384.362685'
  kind: paper
- title: ISO/IEC (2023). ISO/IEC 9075-1 - Information technology - Database languages SQL - Part 1 Framework
  resource: 'https://www.iso.org/standard/76583.html'
  kind: standard
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 2 "Data Models and Query Languages"
  resource: 'https://dataintensive.net/'
  kind: book
foundation_candidate: true
lang: pt-br
translation_of: /concepts/data/data-model.md
---

# Modelo de dados (relacional, documento, grafo)

**Definição.** Um modelo de dados define as estruturas, operações e regras de integridade usadas para representar dados. O modelo relacional de Codd (1970) representa dados como relações (tabelas de linhas) manipuladas por operações de conjunto, independentes do armazenamento físico; o SQL, padronizado como ISO/IEC 9075, é sua linguagem dominante. O modelo de documentos guarda registros aninhados autocontidos (JSON); o modelo de grafo guarda entidades como nós e relacionamentos como arestas (grafos de propriedades, RDF). O DAMA-DMBOK distingue os níveis conceitual, lógico e físico de modelagem.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O modelo de dados é a primeira e mais duradoura decisão de projeto: ele molda o que é fácil consultar e o que exige junções ou duplicação. O relacional serve a relacionamentos muitos-para-muitos e consultas ad hoc; o de documentos serve a dados lidos como unidade; o de grafo serve a dados muito conectados e consultas de caminho. Esta própria base de conhecimento é um grafo de conceitos guardado como documentos.

## Exemplo

Um e-commerce mantém pedidos e clientes em tabelas relacionais (junções, relatórios), guarda os atributos variáveis de cada produto como documento JSON e modela "quem comprou X também comprou Y" como grafo.

## Conceitos mais específicos

- [Grafo de conhecimento](/pt-br/concepts/data/knowledge-graph.md)
- [Banco de dados e SGBD](/pt-br/concepts/data/database-dbms.md)
- [OLTP versus OLAP](/pt-br/concepts/data/oltp-olap.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Codd, E. F. (1970). *A Relational Model of Data for Large Shared Data Banks*. Communications of the ACM 13(6). <https://doi.org/10.1145/362384.362685> — Definição original do modelo relacional e da independência de dados.
2. ISO/IEC (2023). *ISO/IEC 9075-1 - Information technology - Database languages SQL - Part 1 Framework*. <https://www.iso.org/standard/76583.html> — Estrutura do padrão SQL (pago; citado, não transcrito).
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 2 "Data Models and Query Languages". <https://dataintensive.net/> — Trade-offs entre relacional, documento e grafo.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
