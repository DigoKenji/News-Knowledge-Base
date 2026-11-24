---
type: Concept
title: Infraestrutura como código
description: Definir servidores, redes, bancos de dados e outra infraestrutura em arquivos versionados que ferramentas aplicam automaticamente, em vez de clicar em consoles - para que os ambientes sejam reproduzíveis, revisáveis e repetíveis.
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
- title: Morris, K. (2020). Infrastructure as Code - Dynamic Systems for the Cloud Age, 2nd edition. O'Reilly
  resource: 'https://infrastructure-as-code.com/book/'
  kind: book
- title: HashiCorp. What is Terraform? (Terraform documentation)
  resource: 'https://developer.hashicorp.com/terraform/intro'
  kind: docs
- title: Amazon Web Services. Introduction to DevOps on AWS - Infrastructure as code (whitepaper)
  resource: 'https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html'
  kind: docs
lang: pt-br
translation_of: /concepts/cloud-infra/infrastructure-as-code.md
---

# Infraestrutura como código

**Definição.** Morris define infraestrutura como código como uma abordagem para automatizar infraestrutura baseada em práticas de desenvolvimento de software: definições em código, controle de versão, testes e entrega contínua. As ferramentas são declarativas (Terraform, CloudFormation, manifestos Kubernetes: descrevem o estado final e a ferramenta calcula as mudanças) ou imperativas (scripts que listam passos). O ciclo planejar-e-aplicar do Terraform mostra as mudanças exatas antes de serem feitas.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A IaC traz revisão de código, CI/CD e histórico de auditoria à infraestrutura e elimina a deriva de configuração entre ambientes. Os riscos também mudam de lugar: uma mudança errada no código pode apagar a produção em escala, e segredos nunca devem ser versionados, então os planos são revisados e os arquivos de estado protegidos.

## Exemplo

Um pull request acrescenta uma nova réplica de banco num arquivo Terraform. A CI roda terraform plan e publica "1 a adicionar, 0 a mudar, 0 a destruir"; após revisão e merge, o pipeline aplica, e o ambiente de homologação recebe a mesma mudança a partir do mesmo código.

## Conceitos mais específicos

- [Plano de controle](/pt-br/concepts/backend-systems/control-plane.md)
- [Well-Architected Framework](/pt-br/concepts/cloud-infra/well-architected.md)

## Fontes

1. Morris, K. (2020). *Infrastructure as Code - Dynamic Systems for the Cloud Age*, 2nd edition. O'Reilly. <https://infrastructure-as-code.com/book/> — Definição e práticas centrais de infraestrutura como código.
2. HashiCorp. *What is Terraform?* (Terraform documentation). <https://developer.hashicorp.com/terraform/intro> — Configuração declarativa e o fluxo planejar/aplicar.
3. Amazon Web Services. *Introduction to DevOps on AWS - Infrastructure as code* (whitepaper). <https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html> — IaC como prática de DevOps na perspectiva de um provedor.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
