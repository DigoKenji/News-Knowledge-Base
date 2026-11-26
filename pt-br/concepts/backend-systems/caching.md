---
type: Concept
title: Cache (caching)
description: Manter uma cópia dos dados em um armazenamento mais rápido ou mais próximo para que leituras repetidas evitem a fonte original lenta - trocando memória e o risco de servir dados desatualizados por menor latência e menos carga.
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
- title: Fielding, R., Nottingham, M. and Reschke, J. (eds.) (2022). RFC 9111 - HTTP Caching. IETF
  resource: 'https://datatracker.ietf.org/doc/html/rfc9111'
  kind: standard
- title: Nishtala, R. et al. (2013). Scaling Memcache at Facebook. NSDI 2013
  resource: 'https://www.usenix.org/conference/nsdi13/technical-sessions/presentation/nishtala'
  kind: paper
- title: Hennessy, J. L. and Patterson, D. A. (2017). Computer Architecture - A Quantitative Approach, 6th edition, ch. 2 "Memory Hierarchy Design"
  resource: 'https://shop.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1'
  kind: book
lang: pt-br
translation_of: /concepts/backend-systems/caching.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Cache (caching)

**Definição.** Um cache guarda os resultados de leituras ou cálculos anteriores para que requisições posteriores sejam servidas a partir dele. Funciona porque os padrões de acesso têm localidade: dados usados recente ou frequentemente tendem a ser usados de novo. As escolhas principais são o que guardar, onde (CPU, memória do processo, um cache compartilhado como Redis ou memcached, uma CDN, o navegador), como despejar (LRU, LFU, TTL) e como manter atualizado (expiração, invalidação, write-through ou write-back). O cache HTTP é padronizado na RFC 9111.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `performance`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O cache aparece em todas as camadas, dos caches de CPU aos KV caches de LLM e CDNs. Suas dificuldades clássicas são a invalidação (saber quando a cópia está desatualizada), a manada (thundering herd) quando uma entrada quente expira e as partidas a frio após um reinício. Um cache sem o qual o sistema não sobrevive virou dependência, não otimização.

## Exemplo

Uma página de produto lê preços de um banco em 20 ms. O serviço guarda cada produto no memcached por 60 segundos; 95% das leituras agora levam 1 ms, e quando um preço muda a atualização também apaga a entrada em cache.

## Conceitos mais específicos

- [Cache híbrido memória-disco](/pt-br/concepts/backend-systems/hybrid-memory-disk-cache.md)
- [KV cache](/pt-br/concepts/ai-ml/kv-cache.md)

## Fontes

1. Fielding, R., Nottingham, M. and Reschke, J. (eds.) (2022). *RFC 9111 - HTTP Caching*. IETF. <https://datatracker.ietf.org/doc/html/rfc9111> — Modelo padronizado de frescor, validação e controle de cache para HTTP.
2. Nishtala, R. et al. (2013). *Scaling Memcache at Facebook*. NSDI 2013. <https://www.usenix.org/conference/nsdi13/technical-sessions/presentation/nishtala> — Cache look-aside em escala, invalidação e mitigação de manada.
3. Hennessy, J. L. and Patterson, D. A. (2017). *Computer Architecture - A Quantitative Approach*, 6th edition, ch. 2 "Memory Hierarchy Design". <https://shop.elsevier.com/books/computer-architecture/hennessy/978-0-12-811905-1> — Princípio da localidade e fundamentos de cache.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
