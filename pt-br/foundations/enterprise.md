---
type: Foundation
title: Empresa (enterprise)
description: Qualquer conjunto de organizações com objetivos comuns, descrito no nível mais alto que cobre todas as suas missões e funções.
tags:
- foundation
- architecture
- enterprise-architecture
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
  section: Part I §3.38 Enterprise; §1.3 Executive Overview, "What is an enterprise?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.43 Enterprise; §1.1 Executive Overview, "What is an Enterprise?"
  tier: B
- title: 'SEBoK - Guide to the Systems Engineering Body of Knowledge, glossary: Enterprise'
  publisher: 'BKCASE Editorial Board (INCOSE, IEEE Systems Council, Stevens Institute/SERC)'
  edition: 'v2.x (online, accessed 2026-11-25)'
  section: 'Glossary entry, quoting the standards listed'
  resource: 'https://sebokwiki.org/wiki/Enterprise_(glossary)'
  tier: B
evidence:
  bodies:
  - The Open Group
  - ISO (ISO 15704, via SEBoK)
  - US Federal CIO Council (FEAF, via SEBoK)
  meets_standard: yes
  gaps:
  - 'Both quotes are via SEBoK; the ISO 15704 original is paywalled'
related:
- enterprise-architecture
- architecture
domain: architecture
lang: pt-br
translation_of: /foundations/enterprise.md
---

# Empresa (enterprise)

## Definição de trabalho
Uma empresa (enterprise) é qualquer conjunto de organizações que compartilham objetivos comuns: uma companhia, uma divisão, um órgão de governo ou uma parceria. *Segue o TOGAF 9.2, Parte I §1.3 e §3.38, a referência do curso: a empresa é aquilo que a arquitetura pretende cobrir, e pode abranger várias organizações jurídicas.*

## Definições autoritativas
> The highest level (typically) of description of an organization and typically covers all missions and functions. An enterprise will often span multiple organizations.
> *Tradução livre:* O nível mais alto (tipicamente) de descrição de uma organização, que normalmente cobre todas as missões e funções. Uma empresa frequentemente abrange múltiplas organizações.
> — The Open Group, TOGAF Standard 9.2, Part I §3.38 Enterprise

> The TOGAF standard considers an "enterprise" to be any collection of organizations that have common goals.
> *Tradução livre:* O TOGAF Standard considera "empresa" (enterprise) qualquer conjunto de organizações que têm objetivos comuns.
> — The Open Group, TOGAF Standard 9.2, Part I §1.3 Executive Overview

> The highest level (typically) of description of an organization and typically covers all missions and functions. An enterprise will often span multiple organizations.
> *Tradução livre:* O nível mais alto (tipicamente) de descrição de uma organização, que normalmente cobre todas as missões e funções. Uma empresa frequentemente abrange múltiplas organizações.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.43 Enterprise

> The TOGAF Standard considers an “enterprise” to be any collection of organizations that have common goals.
> *Tradução livre:* O TOGAF Standard considera "empresa" (enterprise) qualquer conjunto de organizações que têm objetivos comuns.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §1.1 Executive Overview

> one or more organizations sharing a definite mission, goals, and objectives to offer an output such as a product or service
> — ISO 15704:2000, as reproduced in the [SEBoK glossary: Enterprise](https://sebokwiki.org/wiki/Enterprise_(glossary))

> An organization (or cross organizational entity) supporting a defined business scope and mission that includes interdependent resources (people, organizations and technologies) that must coordinate their functions and share information in support of a common mission (or set of related missions).
> — US Federal CIO Council, Federal Enterprise Architecture Framework (1999), as reproduced in the SEBoK glossary

## Em que diferem
- **Mais dois órgãos concordam com a ideia central do TOGAF.** A ISO 15704 e o framework federal de EA dos EUA definem a empresa como uma ou mais organizações unidas por missão e objetivos comuns, o que corresponde à "coleção de organizações com objetivos comuns" do TOGAF. O FEAF acrescenta os recursos interdependentes (pessoas, organizações, tecnologia) que precisam compartilhar informação.
- A definição formal é palavra por palavra a mesma no 9.2 e no 10.
- O TOGAF 10 §1.1 acrescenta a regra de escopo: "Enterprise" em Arquitetura Empresarial pode significar a empresa inteira ou "uma ou mais áreas específicas de interesse dentro da empresa", podendo incluir parceiros, fornecedores e clientes.
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Define o escopo da [Arquitetura Empresarial](/pt-br/foundations/enterprise-architecture.md). Não é sinônimo de *companhia*: uma companhia pode conter várias empresas, e uma empresa pode abranger várias companhias.

## No curso
A Arquitetura de Negócio enquadra a empresa por meio de propósito, missão, visão e estratégia. Aulas ainda não ingeridas.

## Questões em aberto
- Para estudos de caso do MBA (por exemplo, Tem Tudo para Carro), tratar cada empresa do caso como uma única empresa?
