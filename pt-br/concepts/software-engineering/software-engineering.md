---
type: Concept
title: Engenharia de software
description: A abordagem sistemática, disciplinada e mensurável para desenvolver, operar e manter software - aplicando princípios de engenharia para que o software seja construído de forma confiável, no prazo e em escala, por equipes.
tags:
- software-engineering
- process
- core
domain: software-engineering
subdomain: process
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: IEEE Computer Society (2024). SWEBOK Guide V4.0 - Guide to the Software Engineering Body of Knowledge
  resource: 'https://www.computer.org/education/bodies-of-knowledge/software-engineering'
  kind: standard
- title: ISO/IEC/IEEE (2017). ISO/IEC/IEEE 24765 - Systems and software engineering - Vocabulary
  resource: 'https://www.iso.org/standard/71952.html'
  kind: standard
- title: Naur, P. and Randell, B. (eds.) (1969). Software Engineering - Report on a conference sponsored by the NATO Science Committee, Garmisch 1968
  resource: 'http://homepages.cs.ncl.ac.uk/brian.randell/NATO/nato1968.PDF'
  kind: book
foundation_candidate: true
lang: pt-br
translation_of: /concepts/software-engineering/software-engineering.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Engenharia de software

**Definição.** A ISO/IEC/IEEE 24765 e o SWEBOK do IEEE definem engenharia de software como a aplicação de uma abordagem sistemática, disciplinada e quantificável ao desenvolvimento, operação e manutenção de software. O termo foi popularizado na conferência da OTAN de 1968, convocada para tratar da "crise do software": projetos atrasados, cheios de bugs e acima do orçamento.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `process`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

É o guarda-chuva de tudo neste tema: ciclos de vida, requisitos, projeto, construção, testes, manutenção, gestão de configuração e gestão de engenharia (as áreas de conhecimento do SWEBOK). Difere de programar pelo foco em equipes, tempo e mudança: a maior parte do custo de software está em manter e evoluir o código, não em escrevê-lo pela primeira vez.

## Exemplo

Um estudante escreve um script que funciona uma vez; uma equipe de engenharia de software constrói um sistema de pagamentos com requisitos, revisões de projeto, testes, CI/CD, monitoramento e um plano para os próximos cinco anos de mudanças.

## Conceitos mais específicos

- [Ciclo de vida de desenvolvimento de software (SDLC)](/pt-br/concepts/software-engineering/sdlc.md)
- [Requisitos e atributos de qualidade](/pt-br/concepts/software-engineering/software-requirements.md)
- [Teste de software](/pt-br/concepts/software-engineering/software-testing.md)
- [Padrão de projeto (design pattern)](/pt-br/concepts/software-engineering/design-pattern.md)
- [Imposto de verificação](/pt-br/concepts/software-engineering/verification-tax.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. IEEE Computer Society (2024). *SWEBOK Guide V4.0 - Guide to the Software Engineering Body of Knowledge*. <https://www.computer.org/education/bodies-of-knowledge/software-engineering> — Definição e áreas de conhecimento da disciplina.
2. ISO/IEC/IEEE (2017). *ISO/IEC/IEEE 24765 - Systems and software engineering - Vocabulary*. <https://www.iso.org/standard/71952.html> — Verbete do vocabulário padronizado (pago; citado, não transcrito).
3. Naur, P. and Randell, B. (eds.) (1969). *Software Engineering - Report on a conference sponsored by the NATO Science Committee, Garmisch 1968*. <http://homepages.cs.ncl.ac.uk/brian.randell/NATO/nato1968.PDF> — Origem histórica do termo e a "crise do software".

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
