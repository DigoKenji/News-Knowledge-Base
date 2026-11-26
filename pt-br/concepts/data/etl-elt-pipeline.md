---
type: Concept
title: ETL / ELT e pipeline de dados
description: Um pipeline de dados é uma cadeia de etapas automatizadas que move dados das fontes aos destinos; ETL transforma os dados antes de carregá-los no destino, ELT carrega os dados brutos primeiro e os transforma dentro do destino.
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
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 8 "Data Integration and Interoperability"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: Apache Software Foundation. Apache Airflow documentation - Core Concepts (DAGs, tasks)
  resource: 'https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/index.html'
  kind: docs
- title: dbt Labs. What is dbt? (ELT transformations in the warehouse)
  resource: 'https://docs.getdbt.com/docs/introduction'
  kind: docs
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 10 Batch Processing (MapReduce and Distributed Filesystems; beyond MapReduce); ch. 12 Derived data'
lang: pt-br
translation_of: /concepts/data/etl-elt-pipeline.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# ETL / ELT e pipeline de dados

**Definição.** Extrair, transformar e carregar (ETL) puxa dados dos sistemas operacionais, limpa-os e os reformata numa camada de processamento separada, e depois os carrega num warehouse. Com warehouses em nuvem escaláveis, o ELT se tornou comum: os dados brutos são carregados como estão e transformados com SQL dentro do warehouse (por exemplo com dbt). Um pipeline costuma ser orquestrado como um grafo acíclico dirigido (DAG) de tarefas com agendamentos, dependências, novas tentativas e monitoramento (por exemplo Apache Airflow).

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Pipelines são o encanamento entre sistemas OLTP e análise ou ML. Suas falhas típicas são silenciosas: uma mudança de esquema na origem, um arquivo atrasado ou uma carga duplicada produzem números errados em vez de erros, por isso checagens de qualidade, linhagem e cargas idempotentes importam. O digest diário deste projeto é ele próprio um pequeno pipeline.

## Exemplo

Toda noite um DAG do Airflow extrai os pedidos de ontem do PostgreSQL, carrega-os brutos no warehouse, roda modelos dbt que os juntam a clientes e moedas, testa que nenhum id de pedido está duplicado e atualiza o painel de vendas.

## Conceitos mais específicos

- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Processamento em lote versus em fluxo (streaming)](/pt-br/concepts/data/batch-stream-processing.md)
- [Falha silenciosa](/pt-br/concepts/backend-systems/silent-failure.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 10 Batch Processing (MapReduce and Distributed Filesystems; beyond MapReduce); ch. 12 Derived data: Pipelines como derivação de conjuntos de dados a partir de um registro oficial, e o desenho inspirado na filosofia Unix por trás dos jobs em lote.

## Fontes

1. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 8 "Data Integration and Interoperability". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — ETL/ELT como padrões de integração de dados.
2. Apache Software Foundation. *Apache Airflow documentation - Core Concepts (DAGs, tasks)*. <https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/index.html> — Pipelines como DAGs de tarefas com agendamento e novas tentativas.
3. dbt Labs. *What is dbt?* (ELT transformations in the warehouse). <https://docs.getdbt.com/docs/introduction> — Transformação dentro do warehouse (o T do ELT) com testes.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 10 Batch Processing (MapReduce and Distributed Filesystems; beyond MapReduce); ch. 12 Derived data. Pipelines como derivação de conjuntos de dados a partir de um registro oficial, e o desenho inspirado na filosofia Unix por trás dos jobs em lote.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
