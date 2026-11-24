---
type: Concept
title: Disponibilidade e confiabilidade
description: Confiabilidade é a capacidade de um sistema de executar sua função sem falhar ao longo do tempo; disponibilidade é a fração do tempo em que ele está pronto para uso quando necessário, frequentemente expressa em "noves" (99,9% = cerca de 8,8 horas fora do ar por ano).
tags:
- backend-systems
- core
domain: backend-systems
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC (2023). ISO/IEC 25010 - Systems and software Quality Requirements and Evaluation (SQuaRE) - Product quality model
  resource: 'https://www.iso.org/standard/78176.html'
  kind: standard
- title: IEC. IEC 60050-192 - International Electrotechnical Vocabulary - Dependability (Electropedia)
  resource: 'https://www.electropedia.org/iev/iev.nsf/index?openform&part=192'
  kind: standard
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 3 "Embracing Risk"
  resource: 'https://sre.google/sre-book/embracing-risk/'
  kind: book
foundation_candidate: true
lang: pt-br
translation_of: /concepts/backend-systems/availability-reliability.md
---

# Disponibilidade e confiabilidade

**Definição.** A ISO/IEC 25010 lista confiabilidade como característica de qualidade, com subcaracterísticas que incluem disponibilidade, tolerância a falhas e recuperabilidade; o vocabulário de dependabilidade IEC 60050-192 define disponibilidade como a capacidade de estar em condições de funcionar como requerido. Avizienis et al. colocam ambas dentro da dependabilidade, ao lado de segurança, integridade e manutenibilidade. Em operações, a disponibilidade é frequentemente medida como requisições bem-sucedidas divididas pelo total de requisições.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Estes são os alvos que tolerância a falhas, redundância e práticas de SRE servem. O SRE do Google argumenta que 100% é o alvo errado: cada nove a mais custa muito mais, e os usuários não percebem a diferença além da confiabilidade da própria rede. A diferença entre o alvo e 100% vira o orçamento de erro que financia lançamentos e experimentos.

## Exemplo

Uma API de pagamentos tem meta de 99,95% de disponibilidade mensal, o que permite cerca de 22 minutos de requisições com falha por mês. Uma interrupção de 15 minutos consome a maior parte desse orçamento, então a equipe congela deploys arriscados até o mês virar.

## Conceitos mais específicos

- [Orçamento de erros de SLO](/pt-br/concepts/cloud-infra/slo-error-budget.md)
- [Tolerância a falhas e redundância](/pt-br/concepts/backend-systems/fault-tolerance.md)
- [Falha correlacionada](/pt-br/concepts/backend-systems/correlated-failure.md)
- [Falha em cascata](/pt-br/concepts/backend-systems/cascading-failure.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. ISO/IEC (2023). *ISO/IEC 25010 - Systems and software Quality Requirements and Evaluation (SQuaRE) - Product quality model*. <https://www.iso.org/standard/78176.html> — Confiabilidade como característica de qualidade com disponibilidade, tolerância a falhas e recuperabilidade (pago; citado, não transcrito).
2. IEC. *IEC 60050-192 - International Electrotechnical Vocabulary - Dependability* (Electropedia). <https://www.electropedia.org/iev/iev.nsf/index?openform&part=192> — Definições padronizadas de disponibilidade e confiabilidade.
3. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 3 "Embracing Risk". <https://sre.google/sre-book/embracing-risk/> — Medição de disponibilidade, por que não 100%, orçamentos de erro.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
