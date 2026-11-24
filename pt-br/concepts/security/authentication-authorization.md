---
type: Concept
title: Autenticação vs autorização
description: Autenticação verifica quem alguém (ou algo) é; autorização decide o que essa identidade verificada pode fazer - duas etapas separadas que muitas vezes são confundidas.
tags:
- security
- core
domain: security
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2017). SP 800-63-3 - Digital Identity Guidelines
  resource: 'https://csrc.nist.gov/pubs/sp/800/63/3/final'
  kind: standard
- title: Hardt, D. (ed.) (2012). RFC 6749 - The OAuth 2.0 Authorization Framework. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc6749'
  kind: standard
- title: OWASP. Authentication Cheat Sheet and Authorization Cheat Sheet
  resource: 'https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html'
  kind: practitioner
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/authentication-authorization.md
---

# Autenticação vs autorização

**Definição.** Autenticação (authn) prova uma alegação de identidade usando fatores: algo que você sabe (senha), tem (chave de segurança, celular) ou é (biometria); autenticação multifator combina dois ou mais. Autorização (authz) aplica uma política à identidade autenticada para conceder ou negar acesso a um recurso ou ação, usando modelos como controle de acesso baseado em papéis (RBAC) ou em atributos (ABAC).

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Em sistemas web, o OpenID Connect cuida da autenticação (quem entrou) e o OAuth 2.0 cuida da autorização delegada (o que um app pode fazer em nome do usuário); confundir os dois é uma fonte comum de vulnerabilidades. O próprio HTTP reflete a divisão: 401 significa "não autenticado", 403 "autenticado mas sem permissão". Quebra de controle de acesso, uma falha de autorização, é o primeiro item do OWASP Top 10 (2021).

## Exemplo

Entrar no GitHub com senha e chave de segurança é autenticação; o GitHub permitir que você faça push no seu repositório mas não no de outra pessoa é autorização.

## Conceitos mais específicos

- [OAuth 2.0](/pt-br/concepts/security/oauth2.md)
- [OpenID Connect](/pt-br/concepts/security/oidc.md)
- [Gestão de identidades e acessos (IAM)](/pt-br/concepts/security/identity-access-management.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Autenticação, autenticadores, níveis de garantia.
2. Hardt, D. (ed.) (2012). *RFC 6749 - The OAuth 2.0 Authorization Framework*. IETF. <https://www.rfc-editor.org/rfc/rfc6749> — Autorização delegada como algo distinto de autenticação.
3. OWASP. *Authentication Cheat Sheet* and *Authorization Cheat Sheet*. <https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html> — Orientação prática e erros comuns nos dois.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
