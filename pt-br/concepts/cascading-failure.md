---
type: Concept
title: Falha em cascata
description: Uma falha que se espalha porque a reação à falha de um componente - carga extra, retentativas, tráfego de failover - sobrecarrega outros componentes, que então falham e repassam sua carga adiante.
tags:
- backend-systems
domain: backend-systems
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 22 Addressing Cascading Failures. O'Reilly / Google
  resource: 'https://sre.google/sre-book/addressing-cascading-failures/'
  kind: book
- title: Huang, L. et al. (2022). Metastable Failures in the Wild. OSDI 2022
  resource: 'https://www.usenix.org/conference/osdi22/presentation/huang-lexiang'
  kind: paper
- title: Amazon Builders' Library (AWS Builder Center). Using load shedding to avoid overload
  resource: 'https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload'
  kind: practitioner
lang: pt-br
translation_of: /concepts/cascading-failure.md
---

# Falha em cascata

**Definição.** Uma falha em cascata cresce por realimentação positiva: a falha de parte de um sistema aumenta a probabilidade de que outras partes falhem. O gatilho clássico é a sobrecarga, em que uma réplica cai, seu tráfego vai para as outras, elas passam da capacidade e caem também, até a camada inteira estar fora do ar.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Mecanismos comuns (livro de SRE do Google): sobrecarga de servidores e a latência resultante; esgotamento de recursos (CPU, memória, threads, descritores de arquivo) que deixa servidores mais lentos ou os derruba; retentativas que multiplicam a carga (veja retry storm); health checks que matam instâncias lentas mas funcionando, removendo mais capacidade; e inicialização lenta ou caches frios que fazem instâncias recuperadas falharem de novo. Alguns sistemas entram num estado metaestável em que a sobrecarga se sustenta mesmo depois de o gatilho sumir. Defesas: descarte de carga e limites de concorrência adaptativos, orçamento de retentativas com backoff e jitter, circuit breakers, propagação de deadlines, folga de capacidade (N+2) e degradação elegante de funcionalidades.

## Por que importa e trade-offs

Cascatas transformam problemas pequenos e locais em quedas totais, e estão por trás de muitos dos maiores incidentes públicos. As defesas custam capacidade (folga), complexidade e às vezes falhar requisições de propósito mais cedo (descarte) para salvar o resto.

## Exemplo

Um de três servidores de API cai; os outros dois recebem 50% a mais de tráfego, ficam lentos, falham nos health checks e são reiniciados; os clientes repetem as chamadas, e os servidores reiniciados, com cache frio, desabam sob o acúmulo.

## Fontes

1. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 22 Addressing Cascading Failures. O'Reilly / Google. <https://sre.google/sre-book/addressing-cascading-failures/> — Definição, causas e defesas.
2. Huang, L. et al. (2022). *Metastable Failures in the Wild*. OSDI 2022. <https://www.usenix.org/conference/osdi22/presentation/huang-lexiang> — Sobrecarga que se sustenta depois de removido o gatilho, com dados de incidentes reais.
3. Amazon Builders' Library (AWS Builder Center). *Using load shedding to avoid overload*. <https://builder.aws.com/content/3Eun1EEyX6p2e3VYNyRLSJzLuMV/using-load-shedding-to-avoid-overload> — Descarte de carga como defesa; colapso do goodput sob sobrecarga.

## Relações
- (nenhuma ainda)

## Notas
- [Interrupção da Microsoft: uma falha em cascata que começou com um bug não detectado](/pt-br/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md)
