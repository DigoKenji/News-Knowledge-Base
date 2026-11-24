---
type: Concept
title: Desenvolvimento seguro de software (SSDLC)
description: Incorporar segurança em todas as fases do ciclo de vida de desenvolvimento de software - requisitos, projeto, codificação, testes, entrega e manutenção - em vez de testá-la só no final.
tags:
- security
- core
domain: security
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2022). SP 800-218 - Secure Software Development Framework (SSDF) Version 1.1
  resource: 'https://csrc.nist.gov/pubs/sp/800/218/final'
  kind: standard
- title: OWASP. Software Assurance Maturity Model (SAMM)
  resource: 'https://owaspsamm.org/'
  kind: standard
- title: Microsoft. Security Development Lifecycle (SDL)
  resource: 'https://www.microsoft.com/en-us/securityengineering/sdl'
  kind: practitioner
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/secure-software-development.md
---

# Desenvolvimento seguro de software (SSDLC)

**Definição.** Um ciclo de vida de desenvolvimento seguro acrescenta práticas de segurança a cada etapa: requisitos de segurança, modelagem de ameaças no projeto, padrões de codificação segura, revisão de código, análise estática e dinâmica (SAST, DAST), varredura de dependências, build e entrega seguros, e resposta a vulnerabilidades após a entrega. O Secure Software Development Framework do NIST (SSDF, SP 800-218) agrupa isso em quatro áreas: preparar a organização, proteger o software, produzir software bem protegido e responder a vulnerabilidades.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Corrigir uma falha no projeto custa muito menos do que corrigi-la em produção, que é o argumento do "shift left". O SDL da Microsoft (2004) popularizou a abordagem; o OWASP SAMM é um modelo de maturidade aberto para ela. Com a IA escrevendo mais código, práticas de SSDLC como varredura automática e etapas obrigatórias de revisão importam mais, porque código gerado pode carregar vulnerabilidades em escala.

## Exemplo

Um time faz a modelagem de ameaças de uma nova funcionalidade de pagamento já no projeto, roda SAST e varredura de dependências em todo pull request e tem um processo documentado para corrigir e divulgar vulnerabilidades reportadas depois do lançamento.

## Conceitos mais específicos

- [Modelagem de ameaças](/pt-br/concepts/security/threat-modeling.md)
- [Ataque à cadeia de suprimentos](/pt-br/concepts/security/supply-chain-attack.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. NIST (2022). *SP 800-218 - Secure Software Development Framework (SSDF) Version 1.1*. <https://csrc.nist.gov/pubs/sp/800/218/final> — Os quatro grupos de práticas e as práticas individuais.
2. OWASP. *Software Assurance Maturity Model (SAMM)*. <https://owaspsamm.org/> — Modelo de maturidade aberto para desenvolvimento seguro.
3. Microsoft. *Security Development Lifecycle (SDL)*. <https://www.microsoft.com/en-us/securityengineering/sdl> — O processo da indústria que popularizou o SSDLC.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
