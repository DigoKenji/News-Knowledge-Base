---
type: Concept
title: Desagregação prefill-decode
description: Executa as fases de prefill e decode de LLMs em pools de GPU separados, movendo o cache KV entre elas.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
relations:
- type: uses
  target: /concepts/kv-cache.md
- type: part-of
  target: /concepts/inference-tradeoffs.md
lang: pt-br
translation_of: /concepts/prefill-decode-disaggregation.md
---

# Desagregação prefill-decode

Executa as fases de prefill e decode de LLMs em pools de GPU separados, movendo o cache KV entre elas.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Proposto pela execução diária de 2026-11-16, aguardando a certificação de Kenji.

## Relações
- **usa** [KV cache](/pt-br/concepts/kv-cache.md)
- **parte de** [Throughput/latência/custo de inferência](/pt-br/concepts/inference-tradeoffs.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
