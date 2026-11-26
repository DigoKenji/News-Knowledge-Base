---
type: Concept
title: Análise de trade-offs ATAM
description: 'Architecture Tradeoff Analysis Method - um método do SEI que avalia uma arquitetura contra cenários priorizados de atributos de qualidade para revelar riscos, pontos de sensibilidade e trade-offs.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kazman, R., Klein, M. and Clements, P. (2000). ATAM: Method for Architecture Evaluation. CMU/SEI-2000-TR-004'
  resource: 'https://insights.sei.cmu.edu/library/atam-method-for-architecture-evaluation/'
  kind: standard
- title: 'Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th edition. Addison-Wesley'
  resource: 'https://books.google.com/books?vid=ISBN9780136886099'
  kind: book
relations:
- type: part-of
  target: /concepts/architecture/adr.md
lang: pt-br
translation_of: /concepts/architecture/atam.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; movido para architecture; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Análise de trade-offs ATAM

**Definição.** O Architecture Tradeoff Analysis Method é a avaliação estruturada de arquitetura de software do SEI. Os stakeholders montam uma árvore de utilidade com cenários de atributos de qualidade ordenados por importância e dificuldade; os arquitetos mostram como a arquitetura atende cada um; os avaliadores registram riscos, não riscos, pontos de sensibilidade (decisões que afetam fortemente um atributo) e pontos de trade-off (decisões que afetam vários atributos em sentidos opostos).

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `architecture-practice`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Transforma metas de qualidade vagas em cenários testáveis e explicita os trade-offs antes que mudá-los fique caro. Seus resultados alimentam registros de decisão; um ATAM completo leva dias e muitas pessoas, então variantes mais leves são comuns em sistemas menores.

## Exemplo

O cenário 'uma região cai no horário de pico e o checkout se recupera em 5 minutos' revela que a replicação síncrona entre regiões é um ponto de trade-off entre disponibilidade e latência.

## Fontes

1. Kazman, R., Klein, M. and Clements, P. (2000). ATAM: Method for Architecture Evaluation. CMU/SEI-2000-TR-004. <https://insights.sei.cmu.edu/library/atam-method-for-architecture-evaluation/> — A definição do método e seus passos.
2. Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th edition. Addison-Wesley. <https://books.google.com/books?vid=ISBN9780136886099> — Cenários de atributos de qualidade e avaliação de arquitetura em contexto.

## Relações
- **parte-de** [Registro de decisão de arquitetura](/pt-br/concepts/architecture/adr.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
