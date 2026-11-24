---
type: Concept
title: Goodput
description: A taxa de trabalho útil que um sistema entrega - requisições (ou tokens, ou passos de treino) que terminam corretamente e dentro da meta de latência - em oposição ao throughput bruto, que também conta trabalho atrasado, com falha ou repetido.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Newman, D. (1999). RFC 2647 - Benchmarking Terminology for Firewall Performance, section 3.17 Goodput. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc2647'
  kind: standard
- title: 'Zhong, Y. et al. (2024). DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving. OSDI 2024, arXiv:2401.09670'
  resource: 'https://arxiv.org/abs/2401.09670'
  kind: paper
- title: Google Cloud (2024). Goodput metric as measure of ML productivity
  resource: 'https://cloud.google.com/blog/products/ai-machine-learning/goodput-metric-as-measure-of-ml-productivity'
  kind: practitioner
lang: pt-br
translation_of: /concepts/ai-ml/goodput.md
---

# Goodput

**Definição.** Goodput é o throughput contado apenas para o trabalho que de fato cumpriu seu propósito. O termo vem de redes, onde significa dados da aplicação entregues por segundo excluindo overhead de protocolo e retransmissões. Em serviço de LLM, o DistServe o definiu como a taxa máxima de requisições que um sistema consegue sustentar cumprindo seus SLOs de latência (tempo até o primeiro token e tempo por token de saída).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Mede-se definindo um critério de sucesso por unidade de trabalho (status OK, latência abaixo do SLO, resultado não descartado) e contando só as unidades que o atendem. Sob sobrecarga, o throughput pode continuar alto enquanto o goodput desaba: o servidor está ocupado mas toda resposta chega depois de o cliente ter desistido, ou retentativas repetem o mesmo trabalho. Em treino de ML, o Google usa "ML goodput" para a parcela do tempo gasta em passos de treino produtivos, em vez de reinícios, recarga de checkpoints e aceleradores ociosos.

## Por que importa e trade-offs

Otimizar só para throughput esconde sobrecarga e incentiva batching ou filas que prejudicam os usuários. Goodput liga o planejamento de capacidade ao que os usuários vivenciam. Ele exige um SLO explícito e boa medição; com um SLO frouxo, goodput é igual a throughput.

## Exemplo

Um servidor de LLM processa 100 requisições por segundo, mas só 60 devolvem o primeiro token dentro do SLO de 1 segundo: throughput é 100 req/s, goodput 60 req/s.

## Notas de revisão para Kenji

O rascunho limitava goodput a "requisições que terminam dentro do timeout"; agora é a ideia geral com a origem em redes e a definição de serviço de LLM.

## Fontes

1. Newman, D. (1999). *RFC 2647 - Benchmarking Terminology for Firewall Performance*, section 3.17 Goodput. IETF. <https://www.rfc-editor.org/rfc/rfc2647> — Definição de redes - bits encaminhados ao destino correto, menos os perdidos ou retransmitidos.
2. Zhong, Y. et al. (2024). *DistServe - Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving*. OSDI 2024, arXiv:2401.09670. <https://arxiv.org/abs/2401.09670> — Goodput em serviço de LLM como taxa de requisições que cumprem SLOs de TTFT e TPOT.
3. Google Cloud (2024). *Goodput metric as measure of ML productivity*. <https://cloud.google.com/blog/products/ai-machine-learning/goodput-metric-as-measure-of-ml-productivity> — ML goodput em treino - tempo produtivo vs tempo perdido.

## Relações
- (nenhuma ainda)

## Notas
- [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/pt-br/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
