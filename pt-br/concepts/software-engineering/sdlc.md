---
type: Concept
title: Ciclo de vida de desenvolvimento de software (SDLC)
description: A sequência de atividades que leva o software da ideia à aposentadoria - requisitos, projeto, implementação, testes, implantação, operação e manutenção - e o modelo de processo (cascata, iterativo, ágil) que as organiza.
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
- title: ISO/IEC/IEEE (2017). ISO/IEC/IEEE 12207 - Software life cycle processes
  resource: 'https://www.iso.org/standard/63712.html'
  kind: standard
- title: IEEE Computer Society (2024). SWEBOK Guide V4.0, Software Engineering Process
  resource: 'https://www.computer.org/education/bodies-of-knowledge/software-engineering'
  kind: standard
- title: Boehm, B. (1988). A Spiral Model of Software Development and Enhancement. IEEE Computer 21(5)
  resource: 'https://doi.org/10.1109/2.59'
  kind: paper
foundation_candidate: true
lang: pt-br
translation_of: /concepts/software-engineering/sdlc.md
---

# Ciclo de vida de desenvolvimento de software (SDLC)

**Definição.** A ISO/IEC/IEEE 12207 define os processos de ciclo de vida de software que uma organização pode usar, da aquisição e requisitos ao desenvolvimento, operação, manutenção e descarte. Um modelo de ciclo de vida escolhe como essas atividades são sequenciadas: a cascata as executa uma vez em ordem, modelos iterativos e em espiral as repetem em ciclos com checagens de risco, e métodos ágeis as executam em incrementos curtos.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O SDLC situa cada prática: engenharia de requisitos no começo, testes e segurança o tempo todo, CI/CD e DevOps ligando desenvolvimento e operação. A prática moderna prefere ciclos curtos porque o retorno de usuários reais e da produção reduz o risco de construir a coisa errada.

## Exemplo

Cascata - 12 meses de requisitos, projeto e construção, depois uma entrega. Ágil - um incremento funcionando a cada duas semanas, com os requisitos refinados no caminho.

## Conceitos mais específicos

- [Requisitos e atributos de qualidade](/pt-br/concepts/software-engineering/software-requirements.md)
- [Teste de software](/pt-br/concepts/software-engineering/software-testing.md)
- [Integração contínua e entrega contínua (CI/CD)](/pt-br/concepts/software-engineering/ci-cd.md)
- [Desenvolvimento seguro de software (SSDLC)](/pt-br/concepts/security/secure-software-development.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. ISO/IEC/IEEE (2017). *ISO/IEC/IEEE 12207 - Software life cycle processes*. <https://www.iso.org/standard/63712.html> — Conjunto padronizado de processos de ciclo de vida (pago; citado, não transcrito).
2. IEEE Computer Society (2024). *SWEBOK Guide V4.0*, Software Engineering Process. <https://www.computer.org/education/bodies-of-knowledge/software-engineering> — Modelos de ciclo de vida e definições de processo.
3. Boehm, B. (1988). *A Spiral Model of Software Development and Enhancement*. IEEE Computer 21(5). <https://doi.org/10.1109/2.59> — Alternativa iterativa e guiada por risco à cascata.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
