---
type: Foundation
title: Capacidade de negócio
description: Uma habilidade específica que um negócio pode possuir ou trocar para alcançar uma finalidade determinada — o que o negócio faz, independentemente de quem faz ou de como faz.
tags:
- foundation
- architecture
- business-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
  updated: '2026-11-19T19:00:00-03:00'
baseline: TOGAF 9.2
sources:
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.25 Business Capability; §3.30 Capability
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.28 Business Capability; §4.33 Capability
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Capability (PDF p. 1029); the Guild cites Ulrich Homann, 2006'
  tier: B
- title: 'Business Architecture: A Practical Guide'
  publisher: 'Whelan, J. and Meaden, G. (2012). Gower'
  edition: 1st (2012)
  section: 'ch. 1 Terminology, pp. 6-7; Glossary, p. 255'
  tier: C
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'BIZBOK v9.0 (2020) is not the current Guild version (v15+); re-check the wording if Kenji gets the current glossary'
  - 'TOGAF Series Guide G211 Business Capabilities v2 (same body; useful for the ''what not how'' principle)'
related:
- business-architecture
- application-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/business-capability.md
---

# Capacidade de negócio

## Definição de trabalho
Uma capacidade de negócio é algo que o negócio consegue fazer para alcançar uma finalidade específica (por exemplo, *gerenciar pedidos de clientes*), descrito de forma independente da organização, dos processos e dos sistemas que a entregam. *Segue o TOGAF 9.2 §3.25, a referência do curso (idêntica no TOGAF 10); a parte "independente de como" é o resumo da KB, apoiado por Whelan e Meaden (2012), mas não dito literalmente por nenhum dos órgãos.*

## Definições autoritativas
> A particular ability that a business may possess or exchange to achieve a specific purpose.
> *Tradução livre:* Uma habilidade específica que um negócio pode possuir ou trocar para alcançar uma finalidade determinada.
> — The Open Group, TOGAF Standard 9.2, Part I §3.25 Business Capability

> A particular ability that a business may possess or exchange to achieve a specific purpose.
> *Tradução livre:* Uma habilidade específica que um negócio pode possuir ou trocar para alcançar uma finalidade determinada.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.28 Business Capability

> An ability that an organization, person, or system possesses.
> Note: This a general-purpose definition. See Section 4.28, "Business Capability" for how this concept is refined for usage in Business Architecture.
> *Tradução livre:* Uma habilidade que uma organização, pessoa ou sistema possui. Nota: esta é uma definição de propósito geral. Veja a Seção 4.28, "Business Capability", para saber como este conceito é refinado para uso em Arquitetura de Negócio.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.33 Capability

> A particular ability or capacity that a business may possess or exchange to achieve a specific purpose or outcome.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Capability* (the Guild cites Ulrich Homann, 2006)

## Em que diferem
- **O BIZBOK v9 (Business Architecture Guild)** usa a mesma redação central do TOGAF 9.2 e acrescenta "or capacity" e "or outcome"; ambos remontam a Homann (2006). Whelan e Meaden (2012, Glossário p. 255) usam o sentido geral (uma habilidade que uma organização, pessoa ou sistema possui) e acrescentam que capacidades são de alto nível e precisam de uma combinação de organização, pessoas, processos, sistemas e tecnologia para serem entregues (nível C), o que apoia o resumo "independente de como" da base.
- A capacidade de negócio é idêntica no 9.2 e no 10. A entrada geral *Capability* mudou a nota: a 9.2 dava exemplos ("Enterprise Architecture, marketing, customer contact, or outbound telemarketing"), e a 10 passa a remeter a Business Capability.
- A expressão *possess or exchange* (possuir ou trocar) significa que uma capacidade pode ser comprada ou obtida de um parceiro, e não apenas construída internamente.
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Bloco de construção central da [Arquitetura de Negócio](/pt-br/foundations/business-architecture.md); aplicações sustentam capacidades ([Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md)).

## No curso
Arquitetura de Negócio no MBA. Aulas ainda não ingeridas.

## Questões em aberto
- Nenhuma.
