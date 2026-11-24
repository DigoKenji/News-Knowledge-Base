---
type: Concept
title: Modelos de serviço (IaaS, PaaS, SaaS)
description: As três formas padronizadas de oferecer capacidade de nuvem - infraestrutura (IaaS, alugar máquinas virtuais, armazenamento e redes), plataforma (PaaS, implantar seu código num runtime gerenciado) e software (SaaS, usar uma aplicação pronta) - que diferem em quanto o provedor gerencia.
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
- title: Mell, P. and Grance, T. (2011). NIST SP 800-145 - The NIST Definition of Cloud Computing
  resource: 'https://doi.org/10.6028/NIST.SP.800-145'
  kind: standard
- title: ISO/IEC (2023). ISO/IEC 22123-2 - Information technology - Cloud computing - Part 2 Concepts
  resource: 'https://www.iso.org/standard/80352.html'
  kind: standard
- title: Amazon Web Services. Shared Responsibility Model
  resource: 'https://aws.amazon.com/compliance/shared-responsibility-model/'
  kind: docs
foundation_candidate: true
lang: pt-br
translation_of: /concepts/cloud-infra/cloud-service-models.md
---

# Modelos de serviço (IaaS, PaaS, SaaS)

**Definição.** A NIST SP 800-145 define Software como Serviço (o consumidor usa as aplicações do provedor), Plataforma como Serviço (o consumidor implanta as próprias aplicações usando linguagens, bibliotecas e ferramentas do provedor, sem gerenciar a infraestrutura subjacente) e Infraestrutura como Serviço (o consumidor provisiona processamento, armazenamento e redes e controla sistemas operacionais e aplicações). A ISO/IEC 22123 descreve as mesmas capacidades em termos de tipos de capacidade e categorias de serviço de nuvem.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Os modelos correspondem ao modelo de responsabilidade compartilhada: quanto mais alto (SaaS), mais o provedor protege e opera; no IaaS o cliente aplica patches no sistema operacional e configura a rede. Ofertas mais novas (contêineres como serviço, funções como serviço, APIs de modelos de IA) ficam entre e além dos três originais.

## Exemplo

Alugar uma máquina virtual e instalar o PostgreSQL nela é IaaS; implantar uma aplicação web numa plataforma gerenciada que a executa e escala é PaaS; usar Gmail ou Notion é SaaS.

## Conceitos mais específicos

- [Serverless (sem servidor)](/pt-br/concepts/cloud-infra/serverless.md)
- [Virtualização e contêineres](/pt-br/concepts/cloud-infra/virtualization-containers.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Mell, P. and Grance, T. (2011). *NIST SP 800-145 - The NIST Definition of Cloud Computing*. <https://doi.org/10.6028/NIST.SP.800-145> — Definições de SaaS, PaaS e IaaS.
2. ISO/IEC (2023). *ISO/IEC 22123-2 - Information technology - Cloud computing - Part 2 Concepts*. <https://www.iso.org/standard/80352.html> — Tipos de capacidade e categorias de serviço de nuvem (pago; citado, não transcrito).
3. Amazon Web Services. *Shared Responsibility Model*. <https://aws.amazon.com/compliance/shared-responsibility-model/> — Como a responsabilidade muda entre provedor e cliente conforme o tipo de serviço.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
