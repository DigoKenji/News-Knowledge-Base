---
type: Concept
title: Arquitetura zero-trust
description: Um modelo de segurança que não concede confiança implícita com base na localização de rede - toda requisição a um recurso é autenticada, autorizada e avaliada contra uma política (identidade, saúde do dispositivo, contexto) no momento em que é feita, com acesso de privilégio mínimo.
tags:
- security
- appsec
domain: security
subdomain: appsec
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Rose, S. et al. (2020). NIST SP 800-207 - Zero Trust Architecture
  resource: 'https://csrc.nist.gov/pubs/sp/800/207/final'
  kind: standard
- title: Ward, R. and Beyer, B. (2014). BeyondCorp - A New Approach to Enterprise Security. USENIX login 39(6)
  resource: 'https://research.google/pubs/beyondcorp-a-new-approach-to-enterprise-security/'
  kind: paper
- title: CISA (2023). Zero Trust Maturity Model v2.0
  resource: 'https://www.cisa.gov/zero-trust-maturity-model'
  kind: standard
lang: pt-br
translation_of: /concepts/security/zero-trust-architecture.md
---

# Arquitetura zero-trust

**Definição.** O NIST SP 800-207 define zero trust como um conjunto de princípios que "assume que não há confiança implícita concedida a ativos ou contas de usuário baseada apenas em sua localização física ou de rede". Uma arquitetura zero trust (ZTA) é o plano de uma empresa que aplica esses princípios: proteger recursos e não segmentos de rede, e decidir o acesso por sessão.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `appsec`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Componentes lógicos do NIST: um policy engine decide, um policy administrator abre ou encerra a sessão, e policy enforcement points ficam na frente de cada recurso. As decisões usam identidade (MFA forte, muitas vezes resistente a phishing), postura do dispositivo, contexto da requisição e sinais de comportamento, e são reavaliadas continuamente. O BeyondCorp do Google, iniciado depois dos ataques Aurora de 2009, tirou os funcionários da VPN: todo app interno é acessado por um proxy de acesso que verifica usuário e dispositivo. O modelo de maturidade da CISA organiza a adoção em cinco pilares: identidade, dispositivos, redes, aplicações e workloads, dados.

## Por que importa e trade-offs

O modelo antigo de perímetro falha quando um atacante está dentro (phishing, credenciais de VPN roubadas) e não combina com nuvem e trabalho remoto. Zero trust limita a movimentação lateral e o raio de impacto. É um programa de vários anos, não um produto; precisa de bons inventários de ativos e identidades, acrescenta latência e atrito, e fornecedores muitas vezes vendem ferramentas parciais como "zero trust".

## Exemplo

Uma engenheira abre o painel administrativo interno de um café sem VPN; o proxy de acesso verifica sua identidade, o MFA e se o notebook é gerenciado e atualizado, e então libera só aquele app.

## Fontes

1. Rose, S. et al. (2020). *NIST SP 800-207 - Zero Trust Architecture*. <https://csrc.nist.gov/pubs/sp/800/207/final> — Definição, princípios, componentes lógicos, modelos de implantação.
2. Ward, R. and Beyer, B. (2014). *BeyondCorp - A New Approach to Enterprise Security*. USENIX login 39(6). <https://research.google/pubs/beyondcorp-a-new-approach-to-enterprise-security/> — Implementação de produção em larga escala sem rede privilegiada.
3. CISA (2023). *Zero Trust Maturity Model v2.0*. <https://www.cisa.gov/zero-trust-maturity-model> — Cinco pilares e estágios de maturidade.

## Relações
- (nenhuma ainda)

## Notas
- [Agent Trust Infrastructure (ATI): identidade federada e atestação para agentes de IA entre organizações](/pt-br/notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md)
