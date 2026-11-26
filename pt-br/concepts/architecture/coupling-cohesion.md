---
type: Concept
title: Acoplamento e coesão
description: Duas medidas de projeto modular - acoplamento é o quanto um módulo depende de outros (buscar baixo), coesão é o quanto os elementos dentro de um módulo pertencem juntos (buscar alto).
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Stevens, W. P., Myers, G. J. and Constantine, L. L. (1974). Structured Design. IBM Systems Journal 13(2)
  resource: 'https://doi.org/10.1147/sj.132.0115'
  kind: paper
- title: Parnas, D. L. (1972). On the Criteria to Be Used in Decomposing Systems into Modules. Communications of the ACM 15(12)
  resource: 'https://dl.acm.org/doi/10.1145/361598.361623'
  kind: paper
- title: ISO/IEC/IEEE (2017). ISO/IEC/IEEE 24765 - Systems and software engineering - Vocabulary
  resource: 'https://www.iso.org/standard/71952.html'
  kind: standard
lang: pt-br
translation_of: /concepts/architecture/coupling-cohesion.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Acoplamento e coesão

**Definição.** Stevens, Myers e Constantine introduziram os dois termos em 1974 como parte do projeto estruturado. Baixo acoplamento significa que uma mudança num módulo raramente obriga mudanças em outros; alta coesão significa que um módulo faz um trabalho bem definido. A ocultação de informação de Parnas (1972) é o princípio por trás: esconder cada decisão de projeto que provavelmente vai mudar atrás de uma interface estável.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Essas duas ideias avaliam quase toda escolha estrutural: onde traçar fronteiras de módulos, serviços ou bounded contexts, e se dividir em microservices vai ajudar ou criar um "monólito distribuído" (alto acoplamento pela rede). Valem em todas as escalas, de classes a sistemas empresariais.

## Exemplo

Se mudar o layout das páginas do Notion obriga a editar o resumidor, o buscador e o código do digest, esses módulos estão fortemente acoplados; colocar os detalhes do Notion atrás de um único módulo de sincronização reduz o acoplamento.

## Conceitos mais específicos

- [Monólito modular](/pt-br/concepts/architecture/modular-monolith.md)
- [Contexto delimitado](/pt-br/concepts/architecture/bounded-context.md)
- [Microservices](/pt-br/concepts/architecture/microservices.md)

## Fontes

1. Stevens, W. P., Myers, G. J. and Constantine, L. L. (1974). *Structured Design*. IBM Systems Journal 13(2). <https://doi.org/10.1147/sj.132.0115> — Definições originais de acoplamento e coesão.
2. Parnas, D. L. (1972). *On the Criteria to Be Used in Decomposing Systems into Modules*. Communications of the ACM 15(12). <https://dl.acm.org/doi/10.1145/361598.361623> — Ocultação de informação como base do baixo acoplamento.
3. ISO/IEC/IEEE (2017). *ISO/IEC/IEEE 24765 - Systems and software engineering - Vocabulary*. <https://www.iso.org/standard/71952.html> — Verbetes padronizados de acoplamento e coesão (pago; citado).

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
