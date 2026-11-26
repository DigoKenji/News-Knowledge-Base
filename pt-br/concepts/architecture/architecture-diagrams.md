---
type: Concept
title: 'Diagramas de arquitetura e design guiado por diagramas'
description: 'Usar diagramas não só para comunicar uma arquitetura, mas para testá-la - um bom diagrama mostra os componentes, as relações (linhas) entre eles e as decisões por trás deles, em um nível de detalhe escolhido para o público.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 21 Emphasis Over Completeness; ch. 22 Diagram-Driven Design; ch. 23 Drawing the Line; ch. 24 Sketching Bank Robbers'
- title: 'Brown, S. The C4 model for visualising software architecture'
  resource: 'https://c4model.com/'
  kind: docs
- title: 'ISO/IEC/IEEE 42010 - Architecture description (overview site)'
  resource: 'http://www.iso-architecture.org/42010/'
  kind: standard
lang: pt-br
translation_of: /concepts/architecture/architecture-diagrams.md
---

# Diagramas de arquitetura e design guiado por diagramas

**Definição.** O "design guiado por diagramas" de Hohpe parte da ideia de que é mais difícil trapacear em uma imagem do que em palavras: se não dá para desenhar um diagrama claro do sistema, o design provavelmente também não está claro. Ele insiste que uma arquitetura sem linhas provavelmente não é uma arquitetura, porque são as relações entre os componentes, e não a lista ou a posição deles, que explicam como o sistema se comporta. Diagramas devem priorizar ênfase em vez de completude, usar um vocabulário visual consistente e se adequar ao público, por isso ele orienta engenheiros a desenhar a história do sistema, e não um inventário de caixas.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `architecture-practice`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

É o lado prático do que a ISO/IEC/IEEE 42010 formaliza como visões e pontos de vista para diferentes stakeholders, e de notações como o modelo C4 de Simon Brown (contexto, contêineres, componentes, código), que dão a cada nível de zoom um conjunto fixo de elementos e exigem que toda linha tenha rótulo.

## Exemplo

Um slide mostra seis caixas com nomes de produtos. Redesenhado com setas rotuladas ("publica eventos de pedido", "lê perfil do cliente, HTTPS síncrono"), revela um ciclo de dependência síncrona que ninguém tinha notado.

## Conceitos mais amplos

- [Arquitetura de software](/pt-br/concepts/architecture/software-architecture.md)

## Conceitos relacionados

- [O elevador do arquiteto (o papel do arquiteto)](/pt-br/concepts/architecture/architect-elevator.md)
- [Registro de decisão de arquitetura](/pt-br/concepts/backend-systems/adr.md)

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 21 Emphasis Over Completeness; ch. 22 Diagram-Driven Design; ch. 23 Drawing the Line; ch. 24 Sketching Bank Robbers. Diagramas como ferramenta de design, por que as linhas carregam a arquitetura e como ajudar engenheiros a desenhar imagens úteis.
2. Brown, S. The C4 model for visualising software architecture. <https://c4model.com/> — O modelo C4 para visualizar arquitetura de software em quatro níveis de zoom.
3. ISO/IEC/IEEE 42010 - Architecture description (overview site). <http://www.iso-architecture.org/42010/> — Visão geral das descrições de arquitetura da ISO/IEC/IEEE 42010: stakeholders, interesses, visões e pontos de vista.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
