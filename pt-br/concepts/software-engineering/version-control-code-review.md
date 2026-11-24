---
type: Concept
title: Controle de versão e revisão de código
description: O controle de versão registra cada mudança no código (quem, o quê, quando, por quê) para que equipes trabalhem em paralelo e possam reverter; a revisão de código faz outro engenheiro examinar cada mudança antes de ser integrada, para pegar defeitos e compartilhar conhecimento.
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
- title: Chacon, S. and Straub, B. (2014). Pro Git, 2nd ed. Apress
  resource: 'https://git-scm.com/book/en/v2'
  kind: book
- title: Bacchelli, A. and Bird, C. (2013). Expectations, Outcomes, and Challenges of Modern Code Review. ICSE 2013
  resource: 'https://doi.org/10.1109/ICSE.2013.6606617'
  kind: paper
- title: Google. Google Engineering Practices - Code Review
  resource: 'https://google.github.io/eng-practices/review/'
  kind: practitioner
lang: pt-br
translation_of: /concepts/software-engineering/version-control-code-review.md
---

# Controle de versão e revisão de código

**Definição.** Um sistema de controle de versão (o Git é o padrão de fato) guarda o histórico de uma base de código como commits, suporta branches para trabalho em paralelo e os integra de volta. A revisão de código moderna acontece em pull requests: um revisor lê o diff, comenta e aprova antes da integração. Bacchelli e Bird viram na Microsoft que encontrar defeitos é o objetivo declarado, mas transferência de conhecimento e consciência da equipe são resultados reais importantes.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O controle de versão é a base para CI/CD, auditoria e colaboração. A revisão de código é uma etapa de qualidade, e o gargalo quando a IA gera código mais rápido do que humanos conseguem ler (imposto de verificação). Pull requests pequenos recebem revisões melhores.

## Exemplo

Toda mudança neste projeto passa por um branch e um pull request no GitHub que Kenji revisa antes da integração.

## Conceitos mais específicos

- [Imposto de verificação](/pt-br/concepts/software-engineering/verification-tax.md)
- [Integração contínua e entrega contínua (CI/CD)](/pt-br/concepts/software-engineering/ci-cd.md)

## Fontes

1. Chacon, S. and Straub, B. (2014). *Pro Git*, 2nd ed. Apress. <https://git-scm.com/book/en/v2> — Conceitos de controle de versão, branches e integração.
2. Bacchelli, A. and Bird, C. (2013). *Expectations, Outcomes, and Challenges of Modern Code Review*. ICSE 2013. <https://doi.org/10.1109/ICSE.2013.6606617> — Estudo empírico de por que equipes revisam código e o que isso alcança.
3. Google. *Google Engineering Practices - Code Review*. <https://google.github.io/eng-practices/review/> — Padrões de revisão de código usados em escala.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
