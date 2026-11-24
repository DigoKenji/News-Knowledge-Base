---
type: Concept
title: Pressão de GC
description: A carga que um programa impõe ao coletor de lixo - principalmente pela taxa de alocação e pelo tamanho e densidade de ponteiros do heap vivo - que aparece como CPU gasta coletando, pausas mais longas ou frequentes e maior latência de cauda.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Jones, R., Hosking, A. and Moss, E. (2023). The Garbage Collection Handbook, 2nd ed. CRC Press
  resource: 'https://gchandbook.org/'
  kind: book
- title: The Go Authors. A Guide to the Go Garbage Collector
  resource: 'https://go.dev/doc/gc-guide'
  kind: docs
- title: Oracle. Java SE 21 HotSpot Virtual Machine Garbage Collection Tuning Guide
  resource: 'https://docs.oracle.com/en/java/javase/21/gctuning/'
  kind: docs
lang: pt-br
translation_of: /concepts/gc-pressure.md
---

# Pressão de GC

**Definição.** Pressão de GC é o quanto uma aplicação faz o coletor de lixo trabalhar. Taxas altas de alocação tornam as coletas mais frequentes; um heap vivo grande, com muitos objetos e referências, torna cada coleta (marcação, cópia) mais cara. O resultado é menos CPU para a aplicação e picos de latência durante as pausas.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

A maioria dos runtimes (JVM, .NET, Go, JavaScript) usa coletores por rastreamento cujo custo é aproximadamente proporcional aos dados vivos rastreados mais a frequência ditada pela alocação. Coletores geracionais exploram a observação de que a maioria dos objetos morre cedo, tornando barato o lixo de vida curta; o problema vem de objetos de vida média promovidos à geração antiga, de enormes caches de objetos pequenos e de muitos ponteiros. O guia de GC do Go descreve isso como um trade-off controlado pelo GOGC: mais memória significa menos coletas. Remédios: alocar menos (reutilizar buffers, pools de objetos, tipos por valor), manter menos objetos e maiores ou armazenamento fora do heap para caches, ajustar tamanho do heap e coletor (G1, ZGC, Shenandoah).

## Por que importa e trade-offs

Pressão de GC é uma causa oculta comum de picos de latência p99 e de CPU alta em serviços de linguagens gerenciadas. Reduzi-la muitas vezes deixa o código menos idiomático (pools, layout manual de memória) e pode trocar custo de GC por mais uso de memória.

## Exemplo

Um serviço Java que mantém 50 milhões de objetos pequenos em cache gasta 30% da CPU em GC; guardar o cache em alguns grandes arrays de bytes fora do heap derruba isso para menos de 5%.

## Notas de revisão para Kenji

O rascunho a definia só como "custo de CPU do GC"; agora cobre taxa de alocação, heap vivo e latência de pausa.

## Fontes

1. Jones, R., Hosking, A. and Moss, E. (2023). *The Garbage Collection Handbook*, 2nd ed. CRC Press. <https://gchandbook.org/> — Teoria e custos de coleta por rastreamento e geracional.
2. The Go Authors. *A Guide to the Go Garbage Collector*. <https://go.dev/doc/gc-guide> — Modelo de custo do GC - trade-off CPU vs memória, taxa de alocação, GOGC.
3. Oracle. *Java SE 21 HotSpot Virtual Machine Garbage Collection Tuning Guide*. <https://docs.oracle.com/en/java/javase/21/gctuning/> — Gerações, promoção, metas de pausa vs throughput, escolha do coletor.

## Relações
- (nenhuma ainda)

## Notas
- [Uber reduz em até 90% a memória do cache de tradução mantendo a latência P99 abaixo de 1 ms](/pt-br/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
