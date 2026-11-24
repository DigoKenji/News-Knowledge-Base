---
type: Concept
title: Data warehouse, data lake e lakehouse
description: Um data warehouse guarda dados limpos e estruturados, modelados para análise; um data lake guarda dados brutos de qualquer formato de forma barata em armazenamento de objetos; um lakehouse acrescenta recursos de warehouse (transações, esquemas, SQL rápido) sobre arquivos abertos em um lake.
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
- title: Inmon, W. H. (2005). Building the Data Warehouse, 4th edition. Wiley
  resource: 'https://www.wiley.com/en-us/Building+the+Data+Warehouse%2C+4th+Edition-p-9780764599446'
  kind: book
- title: Armbrust, M., Ghodsi, A., Xin, R. and Zaharia, M. (2021). Lakehouse - A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics. CIDR 2021
  resource: 'https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf'
  kind: paper
- title: Apache Software Foundation. Apache Iceberg documentation
  resource: 'https://iceberg.apache.org/docs/latest/'
  kind: docs
lang: pt-br
translation_of: /concepts/data/data-warehouse-lake-lakehouse.md
---

# Data warehouse, data lake e lakehouse

**Definição.** Inmon definiu o data warehouse como uma coleção de dados orientada a assuntos, integrada, variante no tempo e não volátil, em apoio às decisões gerenciais. Os data lakes, popularizados nos anos 2010, guardam arquivos brutos (CSV, JSON, Parquet, imagens) em armazenamento barato, com esquema aplicado na leitura. Armbrust et al. propuseram o lakehouse: formatos de tabela abertos (Delta Lake, Apache Iceberg, Apache Hudi) acrescentam transações ACID, imposição de esquema e indexação aos arquivos do lake para que uma única cópia sirva BI, ciência de dados e ML.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Estes são os principais destinos dos dados analíticos. Warehouses dão SQL rápido e governado, mas custam mais e lidam mal com dados não estruturados; lakes são flexíveis, mas viram facilmente "pântanos de dados" sem governança e metadados; lakehouses tentam combinar os dois. Feature stores de ML e corpora de RAG frequentemente leem da mesma plataforma.

## Exemplo

Um varejista deposita JSON bruto de clickstream e arquivos de fornecedores em armazenamento de objetos (lake), converte-os em tabelas Iceberg com esquemas e viagem no tempo (lakehouse) e publica um esquema estrela de vendas curado que o financeiro consulta na ferramenta de BI (camada de warehouse).

## Conceitos mais específicos

- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Feature store](/pt-br/concepts/ai-ml/feature-store.md)
- [ETL / ELT e pipeline de dados](/pt-br/concepts/data/etl-elt-pipeline.md)

## Fontes

1. Inmon, W. H. (2005). *Building the Data Warehouse*, 4th edition. Wiley. <https://www.wiley.com/en-us/Building+the+Data+Warehouse%2C+4th+Edition-p-9780764599446> — Definição clássica de data warehouse.
2. Armbrust, M., Ghodsi, A., Xin, R. and Zaharia, M. (2021). *Lakehouse - A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics*. CIDR 2021. <https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf> — Definição de lakehouse e sua relação com warehouses e lakes.
3. Apache Software Foundation. *Apache Iceberg documentation*. <https://iceberg.apache.org/docs/latest/> — Formato de tabela aberto que fornece transações e evolução de esquema sobre arquivos do lake.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
