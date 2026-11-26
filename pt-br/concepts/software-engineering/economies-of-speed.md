---
type: Concept
title: 'Economias de velocidade'
description: 'Competir pela velocidade com que a organização aprende e entrega, e não pelo custo de produzir em escala - reduzindo tempo de espera, tamanho de lote e burocracia de aprovação em vez de maximizar a utilização de recursos.'
tags:
- software-engineering
- delivery
domain: software-engineering
subdomain: delivery
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 35 Economies of Speed; ch. 36 The Infinite Loop; ch. 39 Who Likes Standing in Line?'
- title: 'DORA (DevOps Research and Assessment). Research programme'
  resource: 'https://dora.dev/research/'
  kind: docs
- title: 'Reinertsen, D. G. (2009). The Principles of Product Development Flow. Celeritas'
  resource: 'https://books.google.com/books?vid=ISBN9781935401001'
  kind: book
lang: pt-br
translation_of: /concepts/software-engineering/economies-of-speed.md
---

# Economias de velocidade

**Definição.** Empresas tradicionais buscam economias de escala: lotes grandes, alta utilização, descontos por volume. Hohpe contrasta isso com economias de velocidade, em que o objetivo é encurtar o tempo da ideia ao software em produção. Seu exemplo: uma empresa levou cerca de sete meses para padronizar um produto de controle de versão sem adotá-lo, enquanto uma startup tinha repositório e primeiro commit em uns dez minutos. Velocidade vem menos de trabalhar mais e mais de esperar menos; a teoria de filas mostra que levar a utilização perto de 100% faz o tempo de espera explodir, então os atrasos estão entre as atividades, não nelas. Ciclos de feedback rápidos (Build-Measure-Learn) transformam velocidade em aprendizado.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `delivery`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

A ideia é o argumento organizacional por trás de DevOps, CI/CD e autoatendimento na nuvem, e combina com a pesquisa DORA, em que frequência de deploy e lead time de mudanças são medidas centrais de desempenho de entrega, e com os princípios de fluxo de desenvolvimento de produto de Reinertsen sobre filas e tamanho de lote.

## Exemplo

Um time espera três semanas por um ambiente de teste que leva uma hora para ser montado. Trocar o chamado por infraestrutura como código em autoatendimento elimina a espera sem que ninguém trabalhe mais rápido.

## Conceitos relacionados

- [DevOps](/pt-br/concepts/software-engineering/devops.md)
- [Integração contínua e entrega contínua (CI/CD)](/pt-br/concepts/software-engineering/ci-cd.md)
- [Infraestrutura como código](/pt-br/concepts/cloud-infra/infrastructure-as-code.md)

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 35 Economies of Speed; ch. 36 The Infinite Loop; ch. 39 Who Likes Standing in Line?. Escala versus velocidade, o exemplo do controle de versão, Build-Measure-Learn e teoria de filas sobre por que alta utilização desacelera organizações.
2. DORA (DevOps Research and Assessment). Research programme. <https://dora.dev/research/> — Programa de pesquisa DORA: desempenho de entrega medido por frequência de deploy, lead time, taxa de falha de mudanças e tempo de recuperação.
3. Reinertsen, D. G. (2009). *The Principles of Product Development Flow*. Celeritas. <https://books.google.com/books?vid=ISBN9781935401001> — Fluxo de desenvolvimento de produto: o custo das filas, lotes pequenos e por que utilização é a meta errada.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
