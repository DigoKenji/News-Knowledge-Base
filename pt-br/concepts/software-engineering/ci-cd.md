---
type: Concept
title: Integração contínua e entrega contínua (CI/CD)
description: Integração contínua integra as mudanças de cada desenvolvedor ao branch principal com frequência e verifica cada uma com build e testes automáticos; entrega contínua mantém o software sempre pronto para entrega, com um pipeline automatizado até a produção.
tags:
- software-engineering
- core
domain: software-engineering
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Fowler, M. (2024). Continuous Integration. martinfowler.com
  resource: 'https://martinfowler.com/articles/continuousIntegration.html'
  kind: practitioner
- title: Humble, J. and Farley, D. (2010). Continuous Delivery. Addison-Wesley
  resource: 'https://continuousdelivery.com/'
  kind: book
- title: DORA. Capabilities - Continuous delivery
  resource: 'https://dora.dev/capabilities/continuous-delivery/'
  kind: practitioner
lang: pt-br
translation_of: /concepts/software-engineering/ci-cd.md
---

# Integração contínua e entrega contínua (CI/CD)

**Definição.** Martin Fowler descreve CI como membros de uma equipe integrando seu trabalho com frequência, pelo menos diariamente, com cada integração verificada por um build automatizado que inclui testes. A entrega contínua (Humble e Farley) estende isso para que qualquer versão aprovada no pipeline possa ser entregue com um botão; o deploy contínuo a entrega automaticamente.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

CI/CD é o motor do DevOps: encurta o caminho do código ao usuário, reduz o risco de cada entrega tornando-a pequena, e é medido pelas métricas DORA (frequência de deploy, lead time, taxa de falha de mudanças, tempo de recuperação). Deploys canário e blue-green limitam o impacto de uma entrega ruim.

## Exemplo

Um pull request dispara o GitHub Actions para fazer build, rodar testes e lint; após a integração, o pipeline faz deploy em staging e depois, após checagens, em produção.

## Conceitos mais específicos

- [Deploy canary / blue-green](/pt-br/concepts/cloud-infra/canary-deploy.md)
- [Teste de software](/pt-br/concepts/software-engineering/software-testing.md)

## Fontes

1. Fowler, M. (2024). *Continuous Integration*. martinfowler.com. <https://martinfowler.com/articles/continuousIntegration.html> — Definição e práticas de CI.
2. Humble, J. and Farley, D. (2010). *Continuous Delivery*. Addison-Wesley. <https://continuousdelivery.com/> — Pipelines de deploy e software sempre pronto para entrega.
3. DORA. *Capabilities - Continuous delivery*. <https://dora.dev/capabilities/continuous-delivery/> — Pesquisa que liga a entrega contínua ao desempenho de entrega.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
