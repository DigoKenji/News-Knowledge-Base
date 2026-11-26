---
type: Concept
title: Limite adaptativo de concorrência
description: Um limite de quantas requisições um serviço atende ao mesmo tempo que se ajusta sozinho pela latência ou pelos erros observados - encolhendo rápido quando o serviço mostra congestionamento e crescendo de volta com cautela quando se recupera - em vez de um número fixo ajustado à mão.
tags:
- backend-systems
- reliability
domain: backend-systems
subdomain: reliability
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
- title: Jacobson, V. (1988). Congestion Avoidance and Control. SIGCOMM 1988
  resource: 'https://ee.lbl.gov/papers/congavoid.pdf'
  kind: paper
- title: Netflix (2018). Performance Under Load. Netflix Technology Blog
  resource: 'https://netflixtechblog.medium.com/performance-under-load-3e6fa9a60581'
  kind: practitioner
- title: Envoy Proxy. Adaptive Concurrency filter
  resource: 'https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/adaptive_concurrency_filter'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/adaptive-concurrency-limit.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Limite adaptativo de concorrência

**Definição.** Um limite de concorrência adaptativo aplica ideias de controle de congestionamento do TCP a serviços. O servidor (ou o cliente) mantém um limite de requisições em andamento; o excesso é rejeitado rapidamente ou fica pouco tempo na fila. O limite é reestimado continuamente a partir de sinais como a latência de ida e volta comparada ao seu mínimo sem carga, ou a partir de timeouts e descartes.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `reliability`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Pela lei de Little, concorrência = throughput × latência. Quando um serviço passa da capacidade, a latência sobe antes de o throughput cair, então latência subindo é um sinal precoce de congestionamento. Algoritmos emprestados de redes: AIMD (aumento aditivo, redução multiplicativa, do trabalho de Jacobson sobre TCP em 1988) e esquemas baseados em atraso no estilo do TCP Vegas ou da abordagem Gradient, que comparam a latência atual com a mínima já vista. A biblioteca concurrency-limits da Netflix e o filtro adaptive concurrency do Envoy implementam isso; requisições acima do limite recebem um 429/503 imediato para que os clientes recuem ou procurem outro destino.

## Por que importa e trade-offs

Limites fixos estão errados na maior parte do tempo: altos demais e o serviço sobrecarrega, baixos demais e a capacidade é desperdiçada, e o valor certo muda com deploys, hardware e mistura de tráfego. Limites adaptativos mantêm a latência controlada e protegem contra falha em cascata sem ajuste manual. Podem oscilar, podem confundir latência causada por uma dependência lenta com sobrecarga local e precisam de mínimos razoáveis para não estrangular até zero.

## Exemplo

Um serviço responde normalmente em 20 ms com 50 requisições em andamento. Quando a latência sobe para 60 ms, o limitador reduz o limite para 35 e descarta o resto com 503 até a latência voltar perto de 20 ms, e então o aumenta um passo de cada vez.

## Fontes

1. Jacobson, V. (1988). *Congestion Avoidance and Control*. SIGCOMM 1988. <https://ee.lbl.gov/papers/congavoid.pdf> — Controle de congestionamento AIMD, a origem da técnica.
2. Netflix (2018). *Performance Under Load*. Netflix Technology Blog. <https://netflixtechblog.medium.com/performance-under-load-3e6fa9a60581> — Aplicar ideias de congestionamento do TCP à concorrência de serviços; limites por gradiente de latência.
3. Envoy Proxy. *Adaptive Concurrency filter*. <https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/adaptive_concurrency_filter> — Implementação independente usando um controlador de gradiente sobre a latência mínima amostrada.

## Relações
- (nenhuma ainda)

## Notas
- [Como elevamos os limites de taxa do cluster Databricks em 7,5×](/pt-br/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md)
