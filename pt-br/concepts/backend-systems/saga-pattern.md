---
type: Concept
title: Padrão Saga
description: 'Executar uma transação de negócio entre serviços como uma sequência de transações locais, cada uma com uma ação compensatória que a desfaz se um passo posterior falhar.'
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
relations:
- type: mitigates
  target: /concepts/backend-systems/cap-theorem.md
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 17 Your Coffee Shop Doesn''t Use Two-Phase Commit (Write-off; Retry; Compensating Action)'
- title: 'Garcia-Molina, H. and Salem, K. (1987). Sagas. SIGMOD 1987'
  resource: 'https://doi.org/10.1145/38713.38742'
  kind: paper
- title: 'Richardson, C. Pattern: Saga (microservices.io)'
  resource: 'https://microservices.io/patterns/data/saga.html'
  kind: practitioner
- title: 'Microsoft. Saga distributed transactions pattern (Azure Architecture Center)'
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/saga'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/saga-pattern.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Padrão Saga

**Definição.** Uma saga divide uma transação de negócio longa em uma sequência de transações locais, cada uma confirmada em seu próprio serviço ou banco; se um passo falha, transações compensatórias desfazem os passos já feitos. Garcia-Molina e Salem descreveram sagas em 1987 para transações de banco de longa duração; os microsserviços as adotaram porque o two-phase commit entre serviços é frágil e bloqueante. Sagas são coordenadas por coreografia (serviços reagem aos eventos uns dos outros) ou por orquestração (um coordenador diz a cada serviço o que fazer).

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `messaging-and-workflows`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Sagas abrem mão do isolamento: outras transações podem ver estados intermediários, então os desenhos acrescentam travas semânticas, atualizações comutativas ou reordenação. Cada passo precisa ser idempotente e cada compensação precisa ser possível, o que é fácil para estornos e difícil para e-mails já enviados.

## Exemplo

Reservar viagem: reservar voo → reservar hotel → cobrar cartão. A cobrança falha, então o orquestrador cancela o hotel e depois o voo.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action): Três alternativas ao two-phase commit quando um passo falha: absorver a perda, tentar de novo ou desfazer os passos concluídos com ações compensatórias.

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action). Três alternativas ao two-phase commit quando um passo falha: absorver a perda, tentar de novo ou desfazer os passos concluídos com ações compensatórias.
2. Garcia-Molina, H. and Salem, K. (1987). Sagas. SIGMOD 1987. <https://doi.org/10.1145/38713.38742> — Definição original com transações compensatórias.
3. Richardson, C. Pattern: Saga (microservices.io). <https://microservices.io/patterns/data/saga.html> — Coreografia versus orquestração em microsserviços.
4. Microsoft. Saga distributed transactions pattern (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/patterns/saga> — Anomalias de isolamento e contramedidas.

## Relações
- **mitiga** [Teorema CAP](/pt-br/concepts/backend-systems/cap-theorem.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
