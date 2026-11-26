---
type: Concept
title: gRPC
description: 'Um framework de RPC de código aberto que usa HTTP/2 e Protocol Buffers para chamadas tipadas e eficientes entre serviços, incluindo streaming.'
tags:
- backend-systems
- api-design
domain: backend-systems
subdomain: api-design
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 4 Encoding and Evolution: Dataflow Through Services: REST and RPC (Current directions for RPC; Data encoding and evolution for RPC)'
- title: 'gRPC Authors. Introduction to gRPC'
  resource: 'https://grpc.io/docs/what-is-grpc/introduction/'
  kind: docs
- title: 'Google. Protocol Buffers overview'
  resource: 'https://protobuf.dev/overview/'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/grpc.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# gRPC

**Definição.** O gRPC é um framework de chamada de procedimento remoto de código aberto, originalmente do Google, em que os serviços são definidos em arquivos .proto do Protocol Buffers e o código de cliente e servidor é gerado para várias linguagens. As chamadas rodam sobre HTTP/2 com mensagens binárias Protobuf, e o gRPC suporta chamadas unárias e streaming do cliente, do servidor e bidirecional, com deadlines e cancelamento nativos.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `api-design`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

É a escolha comum para APIs internas entre serviços, quando contratos tipados e eficiência importam mais do que acesso por navegador; APIs públicas usam mais REST/JSON. Suas regras de evolução vêm da numeração de campos do Protobuf, que mantém clientes antigos e novos compatíveis.

## Exemplo

Um serviço de pedidos chama inventory.Reserve(items) com deadline de 200 ms; se o estoque estiver lento, a chamada é cancelada e o serviço de pedidos usa um plano B em vez de esperar.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 4 Encoding and Evolution: Dataflow Through Services: REST and RPC (Current directions for RPC; Data encoding and evolution for RPC): Por que RPC não é como uma chamada local, e como gRPC/Protocol Buffers lidam com a evolução.

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 4 Encoding and Evolution: Dataflow Through Services: REST and RPC (Current directions for RPC; Data encoding and evolution for RPC). Por que RPC não é como uma chamada local, e como gRPC/Protocol Buffers lidam com a evolução.
2. gRPC Authors. Introduction to gRPC. <https://grpc.io/docs/what-is-grpc/introduction/> — Definição de serviços, geração de código e tipos de chamada.
3. Google. Protocol Buffers overview. <https://protobuf.dev/overview/> — A linguagem de definição de interfaces e o formato binário usados pelo gRPC.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
