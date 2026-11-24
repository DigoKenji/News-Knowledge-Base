---
type: Concept
title: DevOps
description: Uma cultura e um conjunto de práticas que unem desenvolvimento de software e operações de TI - responsabilidade compartilhada, automação, retorno rápido e medição - para que equipes entreguem mudanças com rapidez e confiabilidade.
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
- title: Forsgren, N., Humble, J. and Kim, G. (2018). Accelerate - The Science of Lean Software and DevOps. IT Revolution
  resource: 'https://itrevolution.com/product/accelerate/'
  kind: book
- title: DORA. DORA research program
  resource: 'https://dora.dev/'
  kind: practitioner
- title: AWS. What is DevOps?
  resource: 'https://aws.amazon.com/devops/what-is-devops/'
  kind: docs
lang: pt-br
translation_of: /concepts/software-engineering/devops.md
---

# DevOps

**Definição.** O DevOps surgiu por volta de 2009 para derrubar o muro entre desenvolvedores (que querem mudança) e operações (que querem estabilidade). Suas práticas incluem CI/CD, infraestrutura como código, monitoramento e observabilidade, postmortems sem culpados e a responsabilidade "quem constrói, opera". O programa de pesquisa DORA (o livro Accelerate) mostrou que os times de elite fazem deploy com mais frequência e também falham menos.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O SRE (a abordagem do Google) é uma implementação concreta de DevOps com SLOs e orçamentos de erros. DevOps muitas vezes é mal entendido como cargo ou ferramenta; a pesquisa diz que cultura e capacidades, não ferramentas, preveem o desempenho.

## Exemplo

A equipe que escreve o resumidor também é dona da tarefa agendada, dos alertas e das correções de incidentes, em vez de passá-lo para uma equipe de operações separada.

## Conceitos mais específicos

- [Integração contínua e entrega contínua (CI/CD)](/pt-br/concepts/software-engineering/ci-cd.md)
- [Orçamento de erros de SLO](/pt-br/concepts/cloud-infra/slo-error-budget.md)
- [Deploy canary / blue-green](/pt-br/concepts/cloud-infra/canary-deploy.md)
- [Infraestrutura como código](/pt-br/concepts/cloud-infra/infrastructure-as-code.md)

## Fontes

1. Forsgren, N., Humble, J. and Kim, G. (2018). *Accelerate - The Science of Lean Software and DevOps*. IT Revolution. <https://itrevolution.com/product/accelerate/> — Capacidades baseadas em pesquisa e as quatro métricas-chave.
2. DORA. *DORA research program*. <https://dora.dev/> — Pesquisa contínua State of DevOps.
3. AWS. *What is DevOps?*. <https://aws.amazon.com/devops/what-is-devops/> — Definição prática e práticas.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
