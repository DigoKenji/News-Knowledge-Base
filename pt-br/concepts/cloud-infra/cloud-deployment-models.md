---
type: Concept
title: Modelos de implantação (pública, privada, híbrida)
description: Para quem a infraestrutura de nuvem é provisionada e onde ela roda - pública (aberta a qualquer um, operada por um provedor), privada (uma organização), comunitária (um grupo com interesses comuns) ou híbrida (duas ou mais combinadas) - além da prática comum de multinuvem.
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
- title: Microsoft. Azure Architecture Center - Hybrid architecture design
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/hybrid/hybrid-start-here'
  kind: docs
foundation_candidate: true
lang: pt-br
translation_of: /concepts/cloud-infra/cloud-deployment-models.md
---

# Modelos de implantação (pública, privada, híbrida)

**Definição.** A NIST SP 800-145 define quatro modelos de implantação: nuvem privada (uso exclusivo de uma organização), nuvem comunitária (uso exclusivo de uma comunidade com interesses comuns), nuvem pública (uso aberto ao público, nas instalações do provedor) e nuvem híbrida (duas ou mais infraestruturas distintas ligadas por tecnologia que permite portabilidade de dados e aplicações). Multinuvem, usar vários provedores públicos, é uma prática posterior descrita na ISO/IEC 22123.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A escolha é guiada por regulação e residência de dados, latência para sistemas locais, investimentos existentes e custo. A híbrida é comum em bancos e governos; a multinuvem promete menos aprisionamento, mas multiplica competências, ferramentas e custos de saída de dados, então deve ser uma decisão deliberada, não um padrão.

## Exemplo

Um hospital mantém os prontuários no próprio data center (privada) por razões regulatórias, roda o site público e as análises numa nuvem pública e conecta as duas por um link de rede privado (híbrida).

## Conceitos mais específicos

- [Well-Architected Framework](/pt-br/concepts/cloud-infra/well-architected.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Mell, P. and Grance, T. (2011). *NIST SP 800-145 - The NIST Definition of Cloud Computing*. <https://doi.org/10.6028/NIST.SP.800-145> — Definições de nuvem privada, comunitária, pública e híbrida.
2. ISO/IEC (2023). *ISO/IEC 22123-2 - Information technology - Cloud computing - Part 2 Concepts*. <https://www.iso.org/standard/80352.html> — Modelos de implantação incluindo multinuvem (pago; citado, não transcrito).
3. Microsoft. *Azure Architecture Center - Hybrid architecture design*. <https://learn.microsoft.com/en-us/azure/architecture/hybrid/hybrid-start-here> — Orientação prática sobre quando e como adotar nuvem híbrida e multinuvem.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
