---
type: Concept
title: Cache híbrido memória-disco
description: Um cache que mantém o índice e os itens mais quentes na RAM e a parte maior e mais fria em SSD, guardando muito mais dados por servidor do que só a memória, ao custo de leituras mais lentas para itens que estão no disco.
tags:
- backend-systems
- performance
domain: backend-systems
subdomain: performance
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Berg, B. et al. (2020). The CacheLib Caching Engine - Design and Experiences at Scale. OSDI 2020
  resource: 'https://www.usenix.org/conference/osdi20/presentation/berg'
  kind: paper
- title: McAllister, S. et al. (2021). Kangaroo - Caching Billions of Tiny Objects on Flash. SOSP 2021
  resource: 'https://dl.acm.org/doi/10.1145/3477132.3483568'
  kind: paper
- title: Memcached. Extstore (wiki)
  resource: 'https://github.com/memcached/memcached/wiki/Extstore'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/hybrid-memory-disk-cache.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Cache híbrido memória-disco

**Definição.** Um cache híbrido (em camadas) coloca DRAM sobre flash. As consultas verificam a memória primeiro; faltas na memória ainda podem acertar no SSD, muito mais barato por gigabyte e muito mais rápido do que ir ao banco de dados de origem. Itens são promovidos, rebaixados ou descartados entre as camadas por políticas de admissão e descarte.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `performance`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O principal problema de projeto são os metadados: manter um índice completo em memória para bilhões de objetos pequenos custa RAM demais, e escritas pequenas ingênuas desgastam o flash. O CacheLib da Meta usa motores separados para objetos grandes (regiões log-structured) e pequenos (buckets set-associative), com políticas de admissão que evitam gravar no flash itens acessados uma única vez. O Kangaroo combinou um pequeno log com um grande cache set-associative para reduzir tanto DRAM quanto amplificação de escrita. O extstore do Memcached mantém chaves e metadados na RAM e move os valores para o disco. A hybrid memory architecture do Aerospike mantém o índice primário na RAM e os registros em SSD.

## Por que importa e trade-offs

Taxas de acerto e capacidade por dólar muito maiores que caches só em RAM, o que pode significar menos servidores e menos carga no banco. Custos: latência maior e mais variável para acertos no SSD, limites de durabilidade do flash, ajuste mais complexo (admissão, tamanho de segmentos) e aquecimento mais lento após reinícios, a menos que o conteúdo do SSD sobreviva.

## Exemplo

Um servidor com 64 GB de RAM e 2 TB de NVMe guarda em cache 30× mais itens do que só com RAM; 80% dos acertos vêm da RAM em microssegundos, o resto do SSD em cerca de 100 microssegundos, ainda muito mais rápido que uma consulta de 5 ms ao banco.

## Fontes

1. Berg, B. et al. (2020). *The CacheLib Caching Engine - Design and Experiences at Scale*. OSDI 2020. <https://www.usenix.org/conference/osdi20/presentation/berg> — Cache híbrido DRAM mais flash na Meta, motores para objetos pequenos vs grandes, políticas de admissão.
2. McAllister, S. et al. (2021). *Kangaroo - Caching Billions of Tiny Objects on Flash*. SOSP 2021. <https://dl.acm.org/doi/10.1145/3477132.3483568> — Projeto acadêmico independente que reduz metadados em DRAM e escritas no flash.
3. Memcached. *Extstore* (wiki). <https://github.com/memcached/memcached/wiki/Extstore> — Chaves na RAM, valores no disco num cache open source muito usado.

## Relações
- (nenhuma ainda)

## Notas
- [Uber reduz em até 90% a memória do cache de tradução mantendo a latência P99 abaixo de 1 ms](/pt-br/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
