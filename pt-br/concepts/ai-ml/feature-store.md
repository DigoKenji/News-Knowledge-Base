---
type: Concept
title: Feature store
description: 'Um sistema compartilhado que calcula, armazena e serve features de ML de forma consistente para treino e para inferência online, para que ambos usem as mesmas definições.'
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 4 RAG Feature Pipeline: The feature store, p. 128'
- title: 'Feast. Feast documentation (open-source feature store)'
  resource: 'https://docs.feast.dev/'
  kind: docs
- title: 'Hopsworks. Feature Store (MLOps dictionary)'
  resource: 'https://www.hopsworks.ai/dictionary/feature-store'
  kind: docs
lang: pt-br
translation_of: /concepts/ai-ml/feature-store.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Feature store

**Definição.** Um feature store é o sistema de dados entre os dados brutos e os modelos: calcula features com definições versionadas, armazena-as em um repositório offline para treino (histórico correto no ponto no tempo) e em um repositório online para inferência de baixa latência, e permite que times as descubram e reutilizem. Sua função principal é evitar o training-serving skew, quando uma feature é calculada de um jeito no notebook e de outro em produção.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `mlops`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

É a camada de features de uma plataforma de MLOps, alimentada por pipelines em lote e em streaming. Em sistemas com LLM, o mesmo papel é feito pelo armazenamento que guarda documentos limpos, fragmentados e com embeddings para RAG, que o LLM Engineer's Handbook chama de seu feature store.

## Exemplo

'Pedidos nos últimos 7 dias' é definido uma vez; o job de treino lê seus valores históricos na data de cada rótulo, e o serviço de checkout lê o valor atual em milissegundos no repositório online.

## Fontes

1. Iusztin, P. and Labonne, M. (2024). LLM Engineer's Handbook. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 4 RAG Feature Pipeline: The feature store, p. 128. Feature store como o estado versionado e compartilhado das features (e dos dados de RAG).
2. Feast. Feast documentation (open-source feature store). <https://docs.feast.dev/> — Repositórios offline/online e joins no ponto no tempo em uma implementação de código aberto.
3. Hopsworks. Feature Store (MLOps dictionary). <https://www.hopsworks.ai/dictionary/feature-store> — Definição neutra e o problema do training-serving skew.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
