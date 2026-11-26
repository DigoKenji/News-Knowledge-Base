---
type: Concept
title: FinOps e custo de nuvem
description: Um framework operacional e prática cultural que reúne engenharia, finanças e negócio para gerenciar o gasto variável de nuvem - tornando os custos visíveis, alocando-os às equipes e otimizando-os em função do valor para o negócio.
tags:
- cloud-infra
- cloud-platforms
- core
domain: cloud-infra
subdomain: cloud-platforms
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: FinOps Foundation. What is FinOps? and FinOps Framework
  resource: 'https://www.finops.org/introduction/what-is-finops/'
  kind: standard
- title: Storment, J. R. and Fuller, M. (2023). Cloud FinOps, 2nd edition. O'Reilly
  resource: 'https://www.oreilly.com/library/view/cloud-finops-2nd/9781492098348/'
  kind: book
- title: Amazon Web Services. Cost Optimization Pillar - AWS Well-Architected Framework
  resource: 'https://docs.aws.amazon.com/wellarchitected/latest/framework/cost-optimization.html'
  kind: docs
lang: pt-br
translation_of: /concepts/cloud-infra/finops.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# FinOps e custo de nuvem

**Definição.** A FinOps Foundation (parte da Linux Foundation) define FinOps como um framework operacional e prática cultural que maximiza o valor de negócio da nuvem, permite decisões oportunas baseadas em dados e cria responsabilidade financeira por meio da colaboração entre engenharia, finanças e negócio. Seu ciclo de vida tem três fases - informar (visibilidade e alocação), otimizar (dimensionamento correto, compromissos, eliminação de desperdício) e operar (governança e melhoria contínua).

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `cloud-platforms`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Na nuvem todo engenheiro pode gastar dinheiro com uma chamada de API, então o custo vira uma métrica de engenharia ao lado de latência e confiabilidade. Tags, showback/chargeback, custos unitários (custo por cliente ou por 1.000 chamadas de LLM) e orçamentos são as ferramentas básicas; cargas de IA e preços de GPU tornaram o FinOps mais urgente. O orçamento de US$20/mês deste projeto é uma restrição de FinOps.

## Exemplo

Um relatório de custos mensal, dividido por tags de equipe, mostra que o gasto com GPU da equipe de ML dobrou. A investigação encontra instâncias de notebook ociosas rodando o fim de semana todo; uma política de desligamento automático e um compromisso de um ano para a base estável cortam a conta em 35%.

## Conceitos mais específicos

- [Gateway de IA](/pt-br/concepts/ai-ml/ai-gateway.md)
- [Well-Architected Framework](/pt-br/concepts/cloud-infra/well-architected.md)

## Fontes

1. FinOps Foundation. *What is FinOps?* and *FinOps Framework*. <https://www.finops.org/introduction/what-is-finops/> — Definição de FinOps, princípios e o ciclo informar-otimizar-operar.
2. Storment, J. R. and Fuller, M. (2023). *Cloud FinOps*, 2nd edition. O'Reilly. <https://www.oreilly.com/library/view/cloud-finops-2nd/9781492098348/> — Referência prática sobre alocação de custos, otimização e cultura.
3. Amazon Web Services. *Cost Optimization Pillar - AWS Well-Architected Framework*. <https://docs.aws.amazon.com/wellarchitected/latest/framework/cost-optimization.html> — Orientação do provedor sobre práticas de gestão financeira de nuvem.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
