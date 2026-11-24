---
type: Concept
title: Gestão de identidades e acessos (IAM)
description: Os processos e sistemas que gerenciam identidades digitais (pessoas, serviços, dispositivos) ao longo do seu ciclo de vida e controlam o que cada identidade pode acessar - cobrindo provisionamento, autenticação, autorização, federação e revisões de acesso.
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
- title: OpenID Foundation (2014). OpenID Connect Core 1.0
  resource: 'https://openid.net/specs/openid-connect-core-1_0.html'
  kind: standard
- title: AWS. What is IAM? (AWS Identity and Access Management User Guide)
  resource: 'https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html'
  kind: docs
lang: pt-br
translation_of: /concepts/identity-access-management.md
---

# Gestão de identidades e acessos (IAM)

**Definição.** IAM responde "quem é este, e o que pode fazer?" em toda uma organização. Inclui o ciclo de vida da identidade (entrada, mudança, saída), um provedor de identidade que autentica usuários, single sign-on e federação com padrões como SAML e OpenID Connect, modelos de permissão (papéis, políticas) e governança (revisões periódicas de acesso, logs de auditoria). Provedores de nuvem têm seus próprios serviços de IAM para controlar o acesso a recursos.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A identidade virou o principal perímetro de segurança: com nuvem e trabalho remoto, credenciais roubadas e permissões amplas demais causam muitas violações. Identidades não humanas (contas de serviço, chaves de API, agentes de IA) já superam as pessoas em muitas organizações e precisam da mesma disciplina de ciclo de vida.

## Exemplo

Quando um funcionário sai, desativar uma única conta no provedor de identidade remove de uma vez o acesso ao e-mail, ao GitHub e ao console da nuvem, porque todos usam single sign-on.

## Conceitos mais específicos

- [OAuth 2.0](/pt-br/concepts/oauth2.md)
- [OpenID Connect](/pt-br/concepts/oidc.md)
- [Arquitetura zero-trust](/pt-br/concepts/zero-trust-architecture.md)

## Fontes

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Comprovação de identidade, autenticação e federação.
2. OpenID Foundation (2014). *OpenID Connect Core 1.0*. <https://openid.net/specs/openid-connect-core-1_0.html> — Padrão de identidade federada usado para single sign-on.
3. AWS. *What is IAM?* (AWS Identity and Access Management User Guide). <https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html> — IAM na nuvem - identidades, políticas, permissões de privilégio mínimo.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
