---
type: Concept
title: Autenticação vs autorização
description: Autenticação verifica quem alguém (ou algo) é; autorização decide o que essa identidade verificada pode fazer - duas etapas separadas que muitas vezes são confundidas.
tags:
- security
- iam
- core
domain: security
subdomain: iam
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 3 User Authentication, pp. 72-112; ch. 4 Access Control, pp. 113-154'
- title: 'Stuttard, D. and Pinto, M. (2008). The Web Application Hacker''s Handbook. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9780470170779'
  kind: book
  locator: 'ch. 6 Attacking Authentication p. 133; ch. 8 Attacking Access Controls p. 217'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/authentication-authorization.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Autenticação vs autorização

**Definição.** Autenticação (authn) prova uma alegação de identidade usando fatores: algo que você sabe (senha), tem (chave de segurança, celular) ou é (biometria); autenticação multifator combina dois ou mais. Autorização (authz) aplica uma política à identidade autenticada para conceder ou negar acesso a um recurso ou ação, usando modelos como controle de acesso baseado em papéis (RBAC) ou em atributos (ABAC).

Domínio: [security](/pt-br/domains/security.md) · subdomínio `iam`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

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

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, ch. 3 User Authentication, pp. 72-112; ch. 4 Access Control, pp. 113-154: Senhas, tokens, biometria e autenticação remota; DAC, RBAC e ABAC.
- **Stuttard, D. and Pinto, M. (2008)**, *The Web Application Hacker's Handbook*, ch. 6 Attacking Authentication p. 133; ch. 8 Attacking Access Controls p. 217: Como autenticação e autorização falham em aplicações web, lido como lista de erros de design a evitar.

## Fontes

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Autenticação, autenticadores, níveis de garantia.
2. Hardt, D. (ed.) (2012). *RFC 6749 - The OAuth 2.0 Authorization Framework*. IETF. <https://www.rfc-editor.org/rfc/rfc6749> — Autorização delegada como algo distinto de autenticação.
3. OWASP. *Authentication Cheat Sheet* and *Authorization Cheat Sheet*. <https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html> — Orientação prática e erros comuns nos dois.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 3 User Authentication, pp. 72-112; ch. 4 Access Control, pp. 113-154. Senhas, tokens, biometria e autenticação remota; DAC, RBAC e ABAC.
5. Stuttard, D. and Pinto, M. (2008). *The Web Application Hacker's Handbook*. Wiley. <https://books.google.com/books?vid=ISBN9780470170779> — ch. 6 Attacking Authentication p. 133; ch. 8 Attacking Access Controls p. 217. Como autenticação e autorização falham em aplicações web, lido como lista de erros de design a evitar.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
