---
type: Concept
title: Teste de software
description: Executar software sob condições controladas para encontrar defeitos e ganhar confiança de que atende aos requisitos - em vários níveis (unidade, integração, sistema/ponta a ponta) e com testes automatizados rodando a cada mudança.
tags:
- software-engineering
- testing
- core
domain: software-engineering
subdomain: testing
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC/IEEE (2022). ISO/IEC/IEEE 29119-1 - Software testing - General concepts
  resource: 'https://www.iso.org/standard/81291.html'
  kind: standard
- title: ISTQB. ISTQB Glossary
  resource: 'https://glossary.istqb.org/'
  kind: standard
- title: Vocke, H. (2018). The Practical Test Pyramid. martinfowler.com
  resource: 'https://martinfowler.com/articles/practical-test-pyramid.html'
  kind: practitioner
foundation_candidate: true
lang: pt-br
translation_of: /concepts/software-engineering/software-testing.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Teste de software

**Definição.** A ISO/IEC/IEEE 29119 é o padrão internacional para conceitos, processos e técnicas de teste de software. Testes de unidade verificam uma pequena parte isoladamente, testes de integração verificam se as partes funcionam juntas, e testes de sistema ou ponta a ponta verificam a aplicação inteira como um usuário faria. Testes podem mostrar a presença de bugs, nunca provar sua ausência (Dijkstra).

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `testing`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A pirâmide de testes recomenda muitos testes de unidade rápidos, menos testes de integração e poucos testes ponta a ponta lentos. Testes automatizados são o que torna seguros o CI/CD e a refatoração. Formas especializadas estendem os testes a falhas parecidas com as de produção: simulação determinística, chaos engineering e releases canário.

## Exemplo

O validador da KB é um teste: a cada execução verifica se todos os documentos têm frontmatter e se todos os links funcionam, e falha a execução se não.

## Conceitos mais específicos

- [Teste de simulação determinística](/pt-br/concepts/backend-systems/deterministic-simulation-testing.md)
- [Engenharia do caos](/pt-br/concepts/backend-systems/chaos-engineering.md)
- [Deploy canary / blue-green](/pt-br/concepts/software-engineering/canary-deploy.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. ISO/IEC/IEEE (2022). *ISO/IEC/IEEE 29119-1 - Software testing - General concepts*. <https://www.iso.org/standard/81291.html> — Conceitos e níveis de teste padronizados (pago; citado).
2. ISTQB. *ISTQB Glossary*. <https://glossary.istqb.org/> — Terminologia de testes muito usada (unidade, integração, sistema, aceitação).
3. Vocke, H. (2018). *The Practical Test Pyramid*. martinfowler.com. <https://martinfowler.com/articles/practical-test-pyramid.html> — Pirâmide de testes e testes automatizados na prática.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
