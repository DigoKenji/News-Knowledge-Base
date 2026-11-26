---
type: Concept
title: Latência, vazão e latência de cauda
description: Latência é quanto tempo uma requisição leva; vazão é quantas requisições o sistema conclui por unidade de tempo; latência de cauda é a ponta lenta da distribuição de latência (p99, p99,9), que usuários de sistemas grandes encontram muito mais do que as médias sugerem.
tags:
- backend-systems
- performance
- core
domain: backend-systems
subdomain: performance
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Dean, J. and Barroso, L. A. (2013). The Tail at Scale. Communications of the ACM 56(2)
  resource: 'https://doi.org/10.1145/2408776.2408794'
  kind: paper
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 6 "Monitoring Distributed Systems" (the four golden signals)
  resource: 'https://sre.google/sre-book/monitoring-distributed-systems/'
  kind: book
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 1 "Describing Performance"
  resource: 'https://dataintensive.net/'
  kind: book
lang: pt-br
translation_of: /concepts/backend-systems/latency-throughput.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Latência, vazão e latência de cauda

**Definição.** Latência é o tempo entre enviar uma requisição e receber a resposta; vazão é a taxa de trabalho concluído (requisições por segundo, tokens por segundo). Como a latência varia, ela é descrita por percentis: p50 é a mediana, p99 é o valor que 99% das requisições superam. Dean e Barroso mostraram que, quando uma requisição de usuário se espalha por muitos servidores, o servidor mais lento decide o tempo de resposta, então a lentidão rara se torna comum em escala.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `performance`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Este é o vocabulário básico de desempenho de todo backend e do serviço de LLMs. Eles competem entre si: lotes e filas aumentam a vazão mas acrescentam latência; perto da utilização total, a teoria de filas diz que a latência sobe bruscamente. SLOs costumam ser escritos sobre latência de cauda, não médias, porque médias escondem os usuários que estão tendo uma experiência ruim.

## Exemplo

Uma página de busca chama 100 shards de backend. Cada shard responde em 10 ms em 99% das vezes, mas leva 1 s em 1% das chamadas. A chance de pelo menos um dos 100 ser lento é de cerca de 63%, então a maioria dos carregamentos leva um segundo, mesmo que cada shard pareça rápido "na média".

## Conceitos mais específicos

- [Goodput](/pt-br/concepts/ai-ml/goodput.md)
- [Pressão de GC](/pt-br/concepts/backend-systems/gc-pressure.md)
- [KV cache](/pt-br/concepts/ai-ml/kv-cache.md)

## Fontes

1. Dean, J. and Barroso, L. A. (2013). *The Tail at Scale*. Communications of the ACM 56(2). <https://doi.org/10.1145/2408776.2408794> — Latência de cauda, amplificação por fan-out e técnicas tolerantes à cauda.
2. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 6 "Monitoring Distributed Systems" (the four golden signals). <https://sre.google/sre-book/monitoring-distributed-systems/> — Latência e tráfego como sinais de ouro; por que percentis são melhores que médias.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 1 "Describing Performance". <https://dataintensive.net/> — Latência versus tempo de resposta, percentis e definições de vazão.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
