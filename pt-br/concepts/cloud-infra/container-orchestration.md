---
type: Concept
title: Orquestração de contêineres (Kubernetes)
description: Automatizar a implantação, escala, rede e recuperação de contêineres em um cluster de máquinas; o Kubernetes é o orquestrador de código aberto dominante, no qual você declara o estado desejado e controladores mantêm a realidade igual a ele.
tags:
- cloud-infra
- core
domain: cloud-infra
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: The Kubernetes Authors. Kubernetes documentation - Overview and Concepts
  resource: 'https://kubernetes.io/docs/concepts/overview/'
  kind: docs
- title: Burns, B., Grant, B., Oppenheimer, D., Brewer, E. and Wilkes, J. (2016). Borg, Omega, and Kubernetes. ACM Queue 14(1)
  resource: 'https://queue.acm.org/detail.cfm?id=2898444'
  kind: paper
- title: Verma, A. et al. (2015). Large-scale cluster management at Google with Borg. EuroSys 2015
  resource: 'https://doi.org/10.1145/2741948.2741964'
  kind: paper
lang: pt-br
translation_of: /concepts/cloud-infra/container-orchestration.md
---

# Orquestração de contêineres (Kubernetes)

**Definição.** Um orquestrador agenda contêineres nas máquinas, reinicia-os quando falham, ajusta o número de réplicas, dá-lhes nomes de rede estáveis e balanceamento de carga, e implanta novas versões. O Kubernetes, descendente do Borg e do Omega do Google, é declarativo: os usuários enviam objetos (Deployments, Services) que descrevem o estado desejado a um servidor de API apoiado no etcd, e laços de controle reconciliam continuamente o estado real em direção a ele.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A orquestração torna gerenciável rodar centenas de contêineres e é a plataforma típica para microsserviços e para servir modelos auto-hospedados. Ela traz complexidade real (rede, armazenamento, políticas de segurança, atualizações), então equipes pequenas frequentemente preferem Kubernetes gerenciado ou plataformas serverless.

## Exemplo

Uma equipe declara "rodar 6 réplicas de api:v2" num Deployment. Quando um nó morre, o Kubernetes percebe que só 4 estão rodando e inicia mais 2 em outro lugar; quando enviam api:v3, ele substitui os pods aos poucos e para se os health checks falharem.

## Conceitos mais específicos

- [Plano de controle](/pt-br/concepts/backend-systems/control-plane.md)
- [Deploy canary / blue-green](/pt-br/concepts/cloud-infra/canary-deploy.md)
- [Infraestrutura como código](/pt-br/concepts/cloud-infra/infrastructure-as-code.md)

## Fontes

1. The Kubernetes Authors. *Kubernetes documentation - Overview and Concepts*. <https://kubernetes.io/docs/concepts/overview/> — O que o Kubernetes faz, objetos, estado desejado e controladores.
2. Burns, B., Grant, B., Oppenheimer, D., Brewer, E. and Wilkes, J. (2016). *Borg, Omega, and Kubernetes*. ACM Queue 14(1). <https://queue.acm.org/detail.cfm?id=2898444> — Linhagem e lições de projeto da orquestração de clusters no Google.
3. Verma, A. et al. (2015). *Large-scale cluster management at Google with Borg*. EuroSys 2015. <https://doi.org/10.1145/2741948.2741964> — Projeto original de orquestrador em grande escala.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
