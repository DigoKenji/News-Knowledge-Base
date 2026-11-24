---
type: Concept
title: Balanceamento de carga e contrapressão (backpressure)
description: O balanceamento de carga distribui as requisições entre muitos servidores; a contrapressão permite que um componente sobrecarregado sinalize para cima que é preciso desacelerar, de modo que a carga excedente seja rejeitada ou adiada cedo em vez de causar colapso.
tags:
- backend-systems
- core
domain: backend-systems
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 20 "Load Balancing in the Datacenter" and ch. 21 "Handling Overload"
  resource: 'https://sre.google/sre-book/load-balancing-datacenter/'
  kind: book
- title: Reactive Streams. Reactive Streams Specification for the JVM (1.0.4)
  resource: 'https://www.reactive-streams.org/'
  kind: standard
- title: Yanacek, D. Using load shedding to avoid overload. Amazon Builders' Library
  resource: 'https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload'
  kind: blog
lang: pt-br
translation_of: /concepts/backend-systems/load-balancing-backpressure.md
---

# Balanceamento de carga e contrapressão (backpressure)

**Definição.** Um balanceador de carga (DNS, proxy de camada 4 ou 7, ou biblioteca no cliente) escolhe um backend para cada requisição usando políticas como round robin, menos conexões ou escolha ponderada, e retira backends doentes usando health checks. Contrapressão é controle de fluxo na direção oposta: um consumidor que não dá conta limita sua fila e faz os produtores esperarem ou receberem um erro (descarte de carga). A especificação Reactive Streams formaliza a contrapressão baseada em demanda para fluxos assíncronos.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Juntos, eles mantêm o sistema perto da capacidade útil: o balanceamento usa todos os servidores por igual, e a contrapressão com descarte de carga impede que filas ilimitadas transformem um pico em falha em cascata. O SRE do Google enfatiza que um servidor deve rejeitar de forma barata o que não consegue atender, e os clientes devem limitar novas tentativas.

## Exemplo

Um API gateway envia tráfego a 10 instâncias usando menos-requisições-pendentes. Durante um pico, cada instância limita as requisições em andamento a 200 e devolve HTTP 503 com Retry-After acima disso, então a latência das requisições aceitas fica em 80 ms em vez de todas estourarem o tempo.

## Conceitos mais específicos

- [Limite adaptativo de concorrência](/pt-br/concepts/backend-systems/adaptive-concurrency-limit.md)
- [Tempestade de retentativas](/pt-br/concepts/backend-systems/retry-storm.md)
- [Falha em cascata](/pt-br/concepts/backend-systems/cascading-failure.md)
- [Goodput](/pt-br/concepts/ai-ml/goodput.md)

## Fontes

1. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 20 "Load Balancing in the Datacenter" and ch. 21 "Handling Overload". <https://sre.google/sre-book/load-balancing-datacenter/> — Políticas de balanceamento, subconjuntos, descarte de carga e limitação no cliente.
2. Reactive Streams. *Reactive Streams Specification for the JVM* (1.0.4). <https://www.reactive-streams.org/> — Definição formal de contrapressão não bloqueante por sinalização de demanda.
3. Yanacek, D. *Using load shedding to avoid overload*. Amazon Builders' Library. <https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload> — Relato prático de descarte de carga e sua relação com goodput.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
