---
type: Concept
title: Gestão de identidades e acessos (IAM)
description: Os processos e sistemas que gerenciam identidades digitais (pessoas, serviços, dispositivos) ao longo do seu ciclo de vida e controlam o que cada identidade pode acessar - cobrindo provisionamento, autenticação, autorização, federação e revisões de acesso.
tags:
- security
- core
domain: security
subdomain: core
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
- title: OpenID Foundation (2014). OpenID Connect Core 1.0
  resource: 'https://openid.net/specs/openid-connect-core-1_0.html'
  kind: standard
- title: AWS. What is IAM? (AWS Identity and Access Management User Guide)
  resource: 'https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html'
  kind: docs
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§4.5 RBAC p. 127; §4.6 ABAC p. 133; §4.7 Identity, Credential, and Access Management p. 139'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 4 Identity and Access Management, pp. 49-76'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 11 Identity and Access Management Security'
lang: pt-br
translation_of: /concepts/security/identity-access-management.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Gestão de identidades e acessos (IAM)

**Definição.** IAM responde "quem é este, e o que pode fazer?" em toda uma organização. Inclui o ciclo de vida da identidade (entrada, mudança, saída), um provedor de identidade que autentica usuários, single sign-on e federação com padrões como SAML e OpenID Connect, modelos de permissão (papéis, políticas) e governança (revisões periódicas de acesso, logs de auditoria). Provedores de nuvem têm seus próprios serviços de IAM para controlar o acesso a recursos.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

A identidade virou o principal perímetro de segurança: com nuvem e trabalho remoto, credenciais roubadas e permissões amplas demais causam muitas violações. Identidades não humanas (contas de serviço, chaves de API, agentes de IA) já superam as pessoas em muitas organizações e precisam da mesma disciplina de ciclo de vida.

## Exemplo

Quando um funcionário sai, desativar uma única conta no provedor de identidade remove de uma vez o acesso ao e-mail, ao GitHub e ao console da nuvem, porque todos usam single sign-on.

## Conceitos mais específicos

- [OAuth 2.0](/pt-br/concepts/security/oauth2.md)
- [OpenID Connect](/pt-br/concepts/security/oidc.md)
- [Arquitetura zero-trust](/pt-br/concepts/security/zero-trust-architecture.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §4.5 RBAC p. 127; §4.6 ABAC p. 133; §4.7 Identity, Credential, and Access Management p. 139: Modelos de controle de acesso e o ciclo de vida ICAM.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 4 Identity and Access Management, pp. 49-76: IAM na nuvem: identidades, federação, MFA, segredos e revisões de acesso.
- **Chapple, M. and Seidl, D. (2017)**, *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*, ch. 11 Identity and Access Management Security: Ameaças e controles de IAM para analistas.

## Fontes

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Comprovação de identidade, autenticação e federação.
2. OpenID Foundation (2014). *OpenID Connect Core 1.0*. <https://openid.net/specs/openid-connect-core-1_0.html> — Padrão de identidade federada usado para single sign-on.
3. AWS. *What is IAM?* (AWS Identity and Access Management User Guide). <https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html> — IAM na nuvem - identidades, políticas, permissões de privilégio mínimo.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §4.5 RBAC p. 127; §4.6 ABAC p. 133; §4.7 Identity, Credential, and Access Management p. 139. Modelos de controle de acesso e o ciclo de vida ICAM.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 4 Identity and Access Management, pp. 49-76. IAM na nuvem: identidades, federação, MFA, segredos e revisões de acesso.
6. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 11 Identity and Access Management Security. Ameaças e controles de IAM para analistas.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
