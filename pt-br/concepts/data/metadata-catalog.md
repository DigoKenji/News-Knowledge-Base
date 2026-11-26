---
type: Concept
title: Metadados e catálogo de dados
description: Metadados são dados sobre dados - o que um conjunto contém, de onde veio, quem é o dono e como pode ser usado; um catálogo de dados é o inventário pesquisável dos conjuntos de dados de uma organização construído a partir desses metadados, frequentemente com linhagem.
tags:
- data
- data-governance
- core
domain: data
subdomain: data-governance
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 12 "Metadata Management"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: W3C (2024). Data Catalog Vocabulary (DCAT) - Version 3. W3C Recommendation
  resource: 'https://www.w3.org/TR/vocab-dcat-3/'
  kind: standard
- title: OpenLineage. OpenLineage specification and documentation
  resource: 'https://openlineage.io/docs/'
  kind: docs
lang: pt-br
translation_of: /concepts/data/metadata-catalog.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Metadados e catálogo de dados

**Definição.** O DAMA-DMBOK agrupa metadados em de negócio (definições, donos, regras), técnicos (esquemas, tipos, localizações, linhagem) e operacionais (horários de execução, volumes, resultados de qualidade). Um catálogo de dados os reúne para que as pessoas possam descobrir, entender e confiar nos dados. O vocabulário DCAT do W3C padroniza como catálogos e conjuntos de dados são descritos para troca, e o OpenLineage padroniza os eventos de linhagem emitidos por pipelines.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `data-governance`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Os metadados são o que transforma um data lake de pântano em algo utilizável e o que torna a governança aplicável. A linhagem responde "se esta fonte quebrar, quais painéis e modelos são afetados?". O frontmatter desta base de conhecimento (type, status, sources, verified) é um pequeno esquema de metadados para conhecimento em vez de tabelas.

## Exemplo

Uma analista busca "churn" no catálogo, encontra a tabela certificada customer_churn_monthly, lê o dono, o agendamento de atualização e as definições das colunas, e segue a linhagem até as fontes de CRM e faturamento antes de usá-la.

## Conceitos mais específicos

- [Feature store](/pt-br/concepts/ai-ml/feature-store.md)
- [Grafo de conhecimento](/pt-br/concepts/data/knowledge-graph.md)

## Fontes

1. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 12 "Metadata Management". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — Tipos de metadados e gestão de metadados.
2. W3C (2024). *Data Catalog Vocabulary (DCAT) - Version 3*. W3C Recommendation. <https://www.w3.org/TR/vocab-dcat-3/> — Vocabulário padronizado para descrever catálogos e conjuntos de dados.
3. OpenLineage. *OpenLineage specification and documentation*. <https://openlineage.io/docs/> — Padrão aberto para metadados de linhagem emitidos por pipelines.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
