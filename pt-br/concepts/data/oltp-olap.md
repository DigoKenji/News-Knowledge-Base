---
type: Concept
title: OLTP versus OLAP
description: OLTP (processamento transacional online) atende muitas leituras e escritas pequenas e rápidas para operar o negócio; OLAP (processamento analítico online) atende menos consultas, grandes, que varrem e agregam o histórico para analisá-lo.
tags:
- data
- databases
- core
domain: data
subdomain: databases
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Kimball, R. and Ross, M. (2013). The Data Warehouse Toolkit, 3rd edition. Wiley
  resource: 'https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/'
  kind: book
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 3 "Transaction Processing or Analytics?"
  resource: 'https://dataintensive.net/'
  kind: book
- title: Abadi, D. J., Madden, S. R. and Hachem, N. (2008). Column-Stores vs. Row-Stores - How Different Are They Really? SIGMOD 2008
  resource: 'https://doi.org/10.1145/1376616.1376712'
  kind: paper
lang: pt-br
translation_of: /concepts/data/oltp-olap.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# OLTP versus OLAP

**Definição.** Cargas OLTP tocam poucas linhas por requisição, por chave, precisam de baixa latência e garantias transacionais fortes, e costumam ser armazenadas linha a linha. Cargas OLAP leem milhões de linhas mas poucas colunas, agregam-nas (somas, contagens, group by), toleram segundos de latência e são melhor atendidas por armazenamento colunar e esquemas estrela ou floco de neve. Codd introduziu o termo OLAP em 1993; a modelagem dimensional de Kimball é a forma clássica de projetar para ele.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `databases`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Rodar análises no banco operacional deixa os clientes lentos, então as organizações copiam os dados dos sistemas OLTP para um armazenamento analítico (warehouse ou lakehouse) via ETL/ELT ou captura de dados de mudança. Sistemas híbridos (HTAP) tentam atender os dois, mas os padrões de acesso continuam diferentes.

## Exemplo

"Inserir este pedido e baixar o estoque" é OLTP, feito milhares de vezes por segundo. "Receita por região e mês nos últimos três anos" é OLAP, executado num warehouse colunar durante a noite ou sob demanda.

## Conceitos mais específicos

- [Data warehouse, data lake e lakehouse](/pt-br/concepts/data/data-warehouse-lake-lakehouse.md)
- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)

## Fontes

1. Kimball, R. and Ross, M. (2013). *The Data Warehouse Toolkit*, 3rd edition. Wiley. <https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/> — Modelagem dimensional para análise e a divisão operacional versus analítico.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 3 "Transaction Processing or Analytics?". <https://dataintensive.net/> — Contraste dos padrões de acesso OLTP e OLAP e armazenamento colunar.
3. Abadi, D. J., Madden, S. R. and Hachem, N. (2008). *Column-Stores vs. Row-Stores - How Different Are They Really?* SIGMOD 2008. <https://doi.org/10.1145/1376616.1376712> — Por que armazenamento colunar serve a consultas analíticas.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
