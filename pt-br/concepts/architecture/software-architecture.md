---
type: Concept
title: Arquitetura de software
description: A estrutura fundamental de um sistema de software - seus principais elementos, suas relações e os princípios que regem seu projeto e evolução - e a disciplina de tomar essas decisões de alto impacto.
tags:
- architecture
- core
domain: architecture
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC/IEEE (2022). ISO/IEC/IEEE 42010 - Architecture description
  resource: 'https://www.iso.org/standard/74393.html'
  kind: standard
- title: Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th ed. Addison-Wesley
  resource: 'https://www.oreilly.com/library/view/software-architecture-in/9780136885979/'
  kind: book
- title: Perry, D. E. and Wolf, A. L. (1992). Foundations for the Study of Software Architecture. ACM SIGSOFT Software Engineering Notes 17(4)
  resource: 'https://doi.org/10.1145/141874.141884'
  kind: paper
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 8 Is This Architecture? (Defining Software Architecture; Architectural Decisions)'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/architecture/software-architecture.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Arquitetura de software

**Definição.** A ISO/IEC/IEEE 42010 define arquitetura como os conceitos ou propriedades fundamentais de um sistema em seu ambiente, incorporados em seus elementos, relações e nos princípios de seu projeto e evolução (a mesma redação citada no fundamento de TOGAF da KB). Bass, Clements e Kazman definem arquitetura de software como o conjunto de estruturas necessárias para raciocinar sobre o sistema, composto por elementos de software, as relações entre eles e as propriedades de ambos.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A arquitetura de software fica entre a arquitetura empresarial (os domínios de negócio, dados, aplicações e tecnologia do TOGAF, veja os fundamentos) e o projeto detalhado. É guiada principalmente por atributos de qualidade, documentada em visões e registros de decisão (ADRs) e avaliada com métodos como ATAM. Um teste prático: arquitetura é o conjunto de decisões caras de mudar depois.

## Exemplo

Escolher dois repositórios (código e conhecimento), tarefas agendadas na nuvem e um bundle OKF para o Project_News foram decisões de arquitetura, registradas como ADRs.

## Conceitos mais específicos

- [Estilo arquitetural](/pt-br/concepts/architecture/architectural-style.md)
- [Atributos de qualidade e trade-offs](/pt-br/concepts/architecture/quality-attributes.md)
- [Acoplamento e coesão](/pt-br/concepts/architecture/coupling-cohesion.md)
- [Registro de decisão de arquitetura](/pt-br/concepts/backend-systems/adr.md)
- [Análise de trade-offs ATAM](/pt-br/concepts/backend-systems/atam.md)
- [Monólito modular](/pt-br/concepts/architecture/modular-monolith.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 8 Is This Architecture? (Defining Software Architecture; Architectural Decisions): Compara as definições de Garlan-Perry, ISO/IEC 42010 e TOGAF e propõe um teste prático: um documento de arquitetura precisa conter decisões não triviais e sua justificativa.

## Fontes

1. ISO/IEC/IEEE (2022). *ISO/IEC/IEEE 42010 - Architecture description*. <https://www.iso.org/standard/74393.html> — Definição padronizada de arquitetura e descrição de arquitetura (pago; citado).
2. Bass, L., Clements, P. and Kazman, R. (2021). *Software Architecture in Practice*, 4th ed. Addison-Wesley. <https://www.oreilly.com/library/view/software-architecture-in/9780136885979/> — Estruturas, atributos de qualidade e avaliação de arquitetura.
3. Perry, D. E. and Wolf, A. L. (1992). *Foundations for the Study of Software Architecture*. ACM SIGSOFT Software Engineering Notes 17(4). <https://doi.org/10.1145/141874.141884> — Formulação inicial de arquitetura de software como elementos, forma e justificativa.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 8 Is This Architecture? (Defining Software Architecture; Architectural Decisions). Compara as definições de Garlan-Perry, ISO/IEC 42010 e TOGAF e propõe um teste prático: um documento de arquitetura precisa conter decisões não triviais e sua justificativa.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
