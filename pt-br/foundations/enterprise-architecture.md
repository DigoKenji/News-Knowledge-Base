---
type: Foundation
title: Arquitetura Empresarial
description: Arquitetura no escopo de uma empresa inteira (ou de uma grande área dela), usada para alinhar processos, informação e tecnologia com a estratégia e para conduzir a mudança.
tags:
- foundation
- architecture
- enterprise-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §1.1 Executive Overview, "Why is an Enterprise Architecture Needed?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §3 (Enterprise Agility), "Enterprise Architecture provides a framework for change"
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: 'no'
  gaps:
  - TOGAF 10 has no formal entry for Enterprise Architecture in its Definitions chapter (§4); it explains it in §1.1 instead
  - 'A second body: Gartner IT glossary (tier D, blocked from cloud; Kenji can open it), FEAF v2 (US federal, public), or Ross, Weill & Robertson (2006)'
related:
- enterprise
- architecture
- architecture-domain
- solution-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/enterprise-architecture.md
---

# Arquitetura Empresarial

## Definição de trabalho
A Arquitetura Empresarial (EA) é a arquitetura da empresa como um todo: descreve como negócio, dados, aplicações e tecnologia se encaixam em toda a empresa, e é usada para conduzir a mudança, de modo que a empresa permaneça integrada e apoie sua estratégia. *Segue o TOGAF 10 §1.1, o único texto tier B disponível até agora; a confirmar por um segundo órgão.*

## Definições autoritativas
> The purpose of Enterprise Architecture is to optimize across the enterprise the often fragmented legacy of processes (both manual and automated) into an integrated environment that is responsive to change and supportive of the delivery of the business strategy.
> *Tradução livre:* O propósito da Arquitetura Empresarial é otimizar, em toda a empresa, o legado muitas vezes fragmentado de processos (manuais e automatizados), transformando-o em um ambiente integrado, responsivo a mudanças e que apoie a entrega da estratégia de negócio.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §1.1 Executive Overview

> Enterprise Architecture provides a framework for change, linked to both strategic direction and business value.
> *Tradução livre:* A Arquitetura Empresarial oferece um framework para a mudança, vinculado tanto à direção estratégica quanto ao valor de negócio.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), section on enterprise agility

## Em que diferem
- O TOGAF descreve a Arquitetura Empresarial pelo seu **propósito** (otimizar, integrar, possibilitar a mudança) em vez de definir o que ela **é**. A definição de trabalho preenche essa lacuna combinando §4.8 Architecture com §4.43 Enterprise; essa combinação é a interpretação da KB, não uma citação.
- O TOGAF 10 enfatiza que EA não é "apenas outro nome para arquitetura de TI" (§1.1).

## Onde se encaixa
Termo de escopo no domínio [Arquitetura](/pt-br/domains/architecture.md), acima da [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md). O TOGAF 10 também nomeia níveis intermediários: Arquitetura Estratégica (§4.78) e Arquitetura de Segmento (§4.68).

## No curso
Técnicas de Arquitetura (TOGAF). Aulas ainda não ingeridas.

## Questões em aberto
- Aceitar uma definição de trabalho baseada em propósito até que um segundo órgão seja citado?
