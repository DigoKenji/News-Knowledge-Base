---
type: Concept
title: Teste de simulação determinística
description: Testar um sistema distribuído rodando todos os seus nós num único processo contra tempo, rede, disco e aleatoriedade simulados e controlados por uma semente, injetando falhas, para que qualquer falha possa ser reproduzida exatamente.
tags:
- backend-systems
- distributed-systems
domain: backend-systems
subdomain: distributed-systems
also_in:
- software-engineering/testing
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Zhou, J. et al. (2021). FoundationDB - A Distributed Unbundled Transactional Key Value Store. SIGMOD 2021
  resource: 'https://www.foundationdb.org/files/fdb-paper.pdf'
  kind: paper
- title: FoundationDB. Simulation and Testing
  resource: 'https://apple.github.io/foundationdb/testing.html'
  kind: docs
- title: Antithesis. Deterministic simulation testing (docs and resources)
  resource: 'https://antithesis.com/docs/introduction/welcome/'
  kind: practitioner
lang: pt-br
translation_of: /concepts/backend-systems/deterministic-simulation-testing.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Teste de simulação determinística

**Definição.** Teste por simulação determinística (DST) remove toda fonte de não determinismo do sistema testado (threads, relógios, tempo de rede, I/O, números aleatórios) fazendo-as passar por um simulador guiado por uma única semente pseudoaleatória. O simulador então explora milhões de intercalações e combinações de falhas, e uma semente que falha reproduz exatamente a mesma execução sempre.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `distributed-systems`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O FoundationDB foi pioneiro: o banco foi escrito para que o cluster inteiro rode numa única thread dentro de um simulador com rede e discos falsos, e os engenheiros rodavam o equivalente a anos de operação por noite com falhas de máquina, partições, erros de disco e saltos de relógio injetados (ganchos "buggify"). Como o tempo é simulado, os testes rodam mais rápido que o tempo real. TigerBeetle (VOPR), Turso e Antithesis (um hipervisor que torna determinístico qualquer software) levaram a ideia adiante. Exige projetar para isso: todo I/O atrás de interfaces que o simulador controla, sem threads ocultas nem chamadas ao relógio do sistema.

## Por que importa e trade-offs

Bugs distribuídos que aparecem uma vez em um milhão de execuções passam a ser encontráveis e depuráveis. O FoundationDB atribui a isso o fato de pouquíssimos bugs chegarem aos usuários. O custo é arquitetural: adaptar determinismo num sistema existente é difícil, bibliotecas de terceiros podem quebrá-lo e o simulador só encontra as falhas que modela.

## Exemplo

A semente 48213 perde uma mensagem durante a eleição de líder e o cluster perde uma escrita confirmada; rodar a semente 48213 de novo com um depurador reproduz o bug passo a passo.

## Fontes

1. Zhou, J. et al. (2021). *FoundationDB - A Distributed Unbundled Transactional Key Value Store*. SIGMOD 2021. <https://www.foundationdb.org/files/fdb-paper.pdf> — Framework de teste por simulação, injeção de falhas, buggify, resultados.
2. FoundationDB. *Simulation and Testing*. <https://apple.github.io/foundationdb/testing.html> — Como funciona o simulador determinístico e o que ele injeta.
3. Antithesis. *Deterministic simulation testing* (docs and resources). <https://antithesis.com/docs/introduction/welcome/> — Implementação independente e de uso geral da abordagem.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
