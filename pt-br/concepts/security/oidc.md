---
type: Concept
title: OpenID Connect
description: 'Uma camada de identidade sobre o OAuth 2.0 que permite às aplicações verificar quem é o usuário por meio de um ID token assinado por um provedor de identidade (single sign-on).'
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
- title: 'OpenID Foundation (2014). OpenID Connect Core 1.0'
  resource: 'https://openid.net/specs/openid-connect-core-1_0.html'
  kind: standard
- title: 'IETF (2015). RFC 7519 JSON Web Token (JWT)'
  resource: 'https://www.rfc-editor.org/rfc/rfc7519'
  kind: standard
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 4 Identity and Access Management (SSO with SAML and OIDC), p. 62'
relations:
- type: extends
  target: /concepts/security/oauth2.md
lang: pt-br
translation_of: /concepts/security/oidc.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# OpenID Connect

**Definição.** O OpenID Connect é uma camada de identidade sobre o OAuth 2.0, finalizada em 2014. Além do token de acesso, o provedor de identidade devolve um ID token - um JSON Web Token assinado que informa quem é o usuário, quem emitiu o token, para qual aplicação e quando - e oferece um endpoint UserInfo e metadados de descoberta. É o protocolo comum de single sign-on em apps web e móveis.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `iam`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Onde o OAuth cuida da autorização, o OIDC cuida da autenticação; juntos substituem boa parte do SAML em sistemas mais novos, usando JSON e JWTs em vez de XML. Dotson observa que, na prática, a aplicação nunca vê a senha do usuário e o usuário não precisa entrar de novo em cada app.

## Exemplo

'Entrar com a Microsoft' em um portal interno: o portal valida assinatura, emissor e público do ID token e então lê o e-mail e os grupos do usuário em suas claims.

## Fontes

1. OpenID Foundation (2014). OpenID Connect Core 1.0. <https://openid.net/specs/openid-connect-core-1_0.html> — ID token, fluxos e claims.
2. IETF (2015). RFC 7519 JSON Web Token (JWT). <https://www.rfc-editor.org/rfc/rfc7519> — O formato de token usado pelos ID tokens.
3. Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 4 Identity and Access Management (SSO with SAML and OIDC), p. 62. OIDC versus SAML para single sign-on.

## Relações
- **estende** [OAuth 2.0](/pt-br/concepts/security/oauth2.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
