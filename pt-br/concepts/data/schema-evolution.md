---
type: Concept
title: 'Evolução de schema e compatibilidade de dados'
description: 'Mudar a estrutura dos dados ao longo do tempo sem quebrar leitores nem escritores - mantendo compatibilidade retroativa (código novo lê dados antigos) e compatibilidade futura (código antigo lê dados novos), geralmente com codificações baseadas em schema como Protocol Buffers, Thrift ou Avro.'
tags:
- data
- data-engineering
domain: data
subdomain: data-engineering
also_in:
- backend-systems/api-design
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 4 Encoding and Evolution: Formats for Encoding Data (Thrift and Protocol Buffers; Avro); Modes of Dataflow'
- title: 'Google. Protocol Buffers - Updating a Message Type (proto3 language guide)'
  resource: 'https://protobuf.dev/programming-guides/proto3/#updating'
  kind: docs
- title: 'Apache Software Foundation. Apache Avro Specification - Schema Resolution'
  resource: 'https://avro.apache.org/docs/1.11.1/specification/#schema-resolution'
  kind: docs
lang: pt-br
translation_of: /concepts/data/schema-evolution.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Evolução de schema e compatibilidade de dados

**Definição.** Kleppmann define compatibilidade retroativa como o código novo conseguir ler dados escritos pelo código antigo, e compatibilidade futura como o código antigo conseguir ler dados escritos pelo código novo. As duas importam porque, em um sistema em produção, versões antigas e novas convivem: implantações graduais, clientes móveis que não atualizam e dados gravados há anos. Formatos de texto como JSON e XML são flexíveis, mas vagos quanto a tipos; formatos binários com schema (Protocol Buffers, Thrift, Avro) tornam as regras explícitas: dar tags ou nomes aos campos, só adicionar campos opcionais ou com valor padrão, nunca reutilizar uma tag removida. As mesmas regras valem onde quer que os dados fluam: em bancos, em chamadas de serviço (REST/RPC) e em message brokers.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `data-engineering`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

A evolução de schema é o lado dos dados do versionamento de APIs e o motivo de sistemas orientados a eventos precisarem de um schema registry. Ela também se conecta a change data capture e event sourcing, em que eventos antigos precisam continuar legíveis para sempre. Quebrar a compatibilidade é uma das causas mais comuns de implantações com falha em sistemas distribuídos.

## Exemplo

Uma equipe adiciona um campo opcional "desconto" ao evento de pedido. Consumidores antigos ignoram o campo desconhecido (compatibilidade futura), e consumidores novos tratam a ausência de desconto como zero ao ler eventos antigos (compatibilidade retroativa), então produtores e consumidores podem ser implantados em qualquer ordem.

## Conceitos mais amplos

- [Modelo de dados (relacional, documento, grafo)](/pt-br/concepts/data/data-model.md)

## Conceitos relacionados

- [Versionamento de API](/pt-br/concepts/backend-systems/api-versioning.md)
- [gRPC](/pt-br/concepts/backend-systems/grpc.md)

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 4 Encoding and Evolution: Formats for Encoding Data (Thrift and Protocol Buffers; Avro); Modes of Dataflow. Definições de compatibilidade retroativa e futura e como cada codificação suporta a evolução de schema em bancos, serviços e mensagens.
2. Google. Protocol Buffers - Updating a Message Type (proto3 language guide). <https://protobuf.dev/programming-guides/proto3/#updating> — Guia oficial para atualizar tipos de mensagem com segurança.
3. Apache Software Foundation. Apache Avro Specification - Schema Resolution. <https://avro.apache.org/docs/1.11.1/specification/#schema-resolution> — Especificação do Avro, regras de resolução de schema.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
