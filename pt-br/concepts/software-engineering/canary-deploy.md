---
type: Concept
title: Deploy canary / blue-green
description: 'Liberar uma nova versão para uma pequena parte do tráfego (canary) ou para um ambiente paralelo trocado de uma vez (blue-green), para detectar problemas e reverter rápido.'
tags:
- software-engineering
- delivery
domain: software-engineering
subdomain: delivery
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Sato, D. (2014). CanaryRelease (martinfowler.com)'
  resource: 'https://martinfowler.com/bliki/CanaryRelease.html'
  kind: practitioner
- title: 'Fowler, M. (2010). BlueGreenDeployment'
  resource: 'https://martinfowler.com/bliki/BlueGreenDeployment.html'
  kind: practitioner
- title: 'Google SRE. Canarying Releases (The Site Reliability Workbook, ch. 16)'
  resource: 'https://sre.google/workbook/canarying-releases/'
  kind: docs
lang: pt-br
translation_of: /concepts/software-engineering/canary-deploy.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; movido para software-engineering; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Deploy canary / blue-green

**Definição.** Canary e blue-green são estratégias de release que limitam o estrago de uma versão ruim. Em um canary release, a nova versão recebe primeiro uma pequena parte do tráfego ou dos usuários reais enquanto as métricas são comparadas com a versão antiga, e a parte cresce apenas se continuarem saudáveis. Em um deploy blue-green existem dois ambientes idênticos; a nova versão é implantada no ocioso e o tráfego é trocado de uma vez, com o ambiente antigo pronto para reversão imediata.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `delivery`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Ambas pertencem à entrega contínua: transformam o deploy em um passo reversível e observável. Canaries precisam de boa observabilidade e análise automatizada para decidir; blue-green precisa de capacidade em dobro e cuidado com mudanças de schema de banco que as duas versões precisam tolerar.

## Exemplo

A versão 2.4 vai para 1% do tráfego por 30 minutos; sua taxa de erro é 0,8% contra 0,2% da 2.3, então o pipeline interrompe o rollout e devolve todo o tráfego.

## Fontes

1. Sato, D. (2014). CanaryRelease (martinfowler.com). <https://martinfowler.com/bliki/CanaryRelease.html> — Definição de canary releases.
2. Fowler, M. (2010). BlueGreenDeployment. <https://martinfowler.com/bliki/BlueGreenDeployment.html> — Definição de deploy blue-green.
3. Google SRE. Canarying Releases (The Site Reliability Workbook, ch. 16). <https://sre.google/workbook/canarying-releases/> — Análise de canary e escolha de métricas no Google.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
