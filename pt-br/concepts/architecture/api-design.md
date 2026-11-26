---
type: Concept
title: API e design de APIs
description: Uma API (interface de programação de aplicações) é o contrato pelo qual componentes ou sistemas de software conversam entre si; design de APIs é escolher seus recursos, operações, formatos de dados, erros e versionamento para que seja fácil de usar e segura de evoluir.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Fielding, R. T. (2000). Architectural Styles and the Design of Network-based Software Architectures, ch. 5 REST
  resource: 'https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm'
  kind: paper
- title: OpenAPI Initiative (2021). OpenAPI Specification v3.1.0
  resource: 'https://spec.openapis.org/oas/v3.1.0'
  kind: standard
- title: Google Cloud. API Design Guide
  resource: 'https://docs.cloud.google.com/apis/design'
  kind: practitioner
lang: pt-br
translation_of: /concepts/architecture/api-design.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# API e design de APIs

**Definição.** APIs web seguem principalmente o REST (Fielding, 2000): recursos identificados por URLs, manipulados com métodos HTTP padrão, requisições sem estado. Alternativas incluem estilos RPC como gRPC, GraphQL e APIs assíncronas por eventos. A OpenAPI Specification é a forma padrão de descrever APIs HTTP como contratos legíveis por máquina.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

APIs são as fronteiras entre serviços, equipes e empresas, então seu design tem consequências duradouras: depois de publicadas, mudanças incompatíveis prejudicam clientes, daí versionamento, compatibilidade retroativa e operações idempotentes para retentativas seguras. Boa prática: desenhar o contrato primeiro, nomes consistentes, modelo claro de erros, paginação, autenticação com OAuth.

## Exemplo

GET /articles?domain=security&since=2026-11-01 devolve uma página de artigos; POST /articles com um cabeçalho Idempotency-Key pode ser repetido sem criar duplicatas.

## Conceitos mais específicos

- [Versionamento de API](/pt-br/concepts/backend-systems/api-versioning.md)
- [gRPC](/pt-br/concepts/backend-systems/grpc.md)
- [Idempotência](/pt-br/concepts/backend-systems/idempotency.md)
- [OAuth 2.0](/pt-br/concepts/security/oauth2.md)

## Fontes

1. Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures*, ch. 5 REST. <https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm> — Definição do estilo arquitetural REST.
2. OpenAPI Initiative (2021). *OpenAPI Specification v3.1.0*. <https://spec.openapis.org/oas/v3.1.0> — Descrição padrão e legível por máquina de APIs HTTP.
3. Google Cloud. *API Design Guide*. <https://docs.cloud.google.com/apis/design> — Design orientado a recursos, nomes, erros, versionamento em escala.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
