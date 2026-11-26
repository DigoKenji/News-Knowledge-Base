---
type: Concept
title: OAuth 2.0
description: 'Um framework de autorização que permite a uma aplicação obter acesso limitado aos recursos de um usuário em outro serviço por meio de tokens de acesso, sem lidar com a senha do usuário.'
tags:
- security
- iam
domain: security
subdomain: iam
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'IETF (2012). RFC 6749 The OAuth 2.0 Authorization Framework'
  resource: 'https://www.rfc-editor.org/rfc/rfc6749'
  kind: standard
- title: 'IETF (2025). RFC 9700 Best Current Practice for OAuth 2.0 Security'
  resource: 'https://www.rfc-editor.org/rfc/rfc9700'
  kind: standard
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 4 Identity and Access Management (SSO, OIDC on OAuth 2.0), p. 62'
lang: pt-br
translation_of: /concepts/security/oauth2.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# OAuth 2.0

**Definição.** O OAuth 2.0 (RFC 6749) é um framework de autorização: permite que uma aplicação cliente obtenha um token de acesso para chamar uma API em nome de um dono de recurso (geralmente um usuário), com escopo e validade limitados, sem nunca ver a senha do usuário. Um servidor de autorização emite os tokens depois que o usuário consente; tipos de concessão como authorization code (com PKCE) e client credentials atendem a diferentes tipos de cliente.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `iam`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

O OAuth responde 'o que este app pode fazer?', e não 'quem é o usuário?' - a autenticação é acrescentada pelo OpenID Connect por cima. A melhor prática de segurança de 2025 (RFC 9700) aposenta os fluxos implicit e password e exige PKCE e correspondência exata de redirecionamento.

## Exemplo

Um app de agenda pede para ler seu Google Agenda; você consente na página do Google e o app recebe um token com escopo calendar.readonly, e não sua senha do Google.

## Fontes

1. IETF (2012). RFC 6749 The OAuth 2.0 Authorization Framework. <https://www.rfc-editor.org/rfc/rfc6749> — A especificação: papéis, concessões, tokens.
2. IETF (2025). RFC 9700 Best Current Practice for OAuth 2.0 Security. <https://www.rfc-editor.org/rfc/rfc9700> — Requisitos de segurança atuais (PKCE, sem fluxo implicit).
3. Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 4 Identity and Access Management (SSO, OIDC on OAuth 2.0), p. 62. OAuth e OIDC em identidade na nuvem e SSO.

## Relações
- [OpenID Connect](/pt-br/concepts/security/oidc.md) **estende** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
