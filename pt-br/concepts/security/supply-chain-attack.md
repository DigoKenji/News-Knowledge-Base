---
type: Concept
title: Ataque à cadeia de suprimentos
description: Um ataque que compromete algo de que o alvo depende - uma biblioteca de terceiros, um registro de pacotes, um sistema de build, um canal de atualização, um fornecedor ou uma ferramenta de desenvolvimento - para que o código malicioso ou o acesso chegue ao alvo por um caminho confiável em vez de um ataque direto.
tags:
- security
domain: security
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: MITRE ATT&CK. T1195 Supply Chain Compromise
  resource: 'https://attack.mitre.org/techniques/T1195/'
  kind: standard
- title: NIST (2022). SP 800-161 Rev. 1 - Cybersecurity Supply Chain Risk Management Practices
  resource: 'https://csrc.nist.gov/pubs/sp/800/161/r1/final'
  kind: standard
- title: SLSA. Supply-chain Levels for Software Artifacts v1.0 - Threats
  resource: 'https://slsa.dev/spec/v1.0/threats'
  kind: standard
- title: 'Ohm, M. et al. (2020). Backstabber''s Knife Collection - A Review of Open Source Software Supply Chain Attacks. DIMVA 2020, arXiv:2005.09535'
  resource: 'https://arxiv.org/abs/2005.09535'
  kind: paper
lang: pt-br
translation_of: /concepts/security/supply-chain-attack.md
---

# Ataque à cadeia de suprimentos

**Definição.** Um ataque à cadeia de suprimentos de software mira o processo que produz e entrega software, e não o software em execução. Um único comprometimento na origem pode atingir milhares de organizações adiante, porque elas instalam e executam o que seus fornecedores entregam. O MITRE ATT&CK o classifica como técnica T1195, Supply Chain Compromise.

Domínio: [security](/pt-br/domains/security.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Vetores comuns: pacotes maliciosos (typosquatting como "reqeusts", dependency confusion em que um pacote público sobrepõe um nome interno), tomada da conta de um mantenedor ou uma longa campanha de engenharia social (backdoor do xz-utils, 2024), sistemas de build comprometidos que injetam código em releases assinados (SolarWinds Orion, 2020), servidores de atualização envenenados e actions de CI ou ferramentas de desenvolvimento comprometidas (incluindo ferramentas de código com IA e servidores MCP). Ohm et al. catalogaram centenas de pacotes maliciosos reais no npm, PyPI e RubyGems. Defesas: fixar e verificar dependências (lockfiles, hashes), SBOMs, proveniência e assinatura (níveis SLSA, Sigstore), builds isolados e reproduzíveis, tokens de CI com privilégio mínimo e gestão de risco de fornecedores (NIST SP 800-161).

## Por que importa e trade-offs

Aplicações modernas são em grande parte código de terceiros, então a cadeia de suprimentos é uma superfície de ataque grande e compartilhada, com enorme alavancagem para atacantes. As defesas acrescentam atrito para atualizar dependências e fazer builds, e verificar toda a origem é impossível, então as organizações priorizam pela criticidade.

## Exemplo

Em 2020 atacantes inseriram uma backdoor no processo de build do SolarWinds Orion; cerca de 18.000 clientes instalaram a atualização assinada, incluindo agências do governo dos EUA.

## Notas de revisão para Kenji

O rascunho limitava isto a "um release de dependência"; agora cobre também sistemas de build, canais de atualização e fornecedores, como no NIST e no MITRE.

## Fontes

1. MITRE ATT&CK. *T1195 Supply Chain Compromise*. <https://attack.mitre.org/techniques/T1195/> — Definição da técnica e subtécnicas (dependências, software, hardware).
2. NIST (2022). *SP 800-161 Rev. 1 - Cybersecurity Supply Chain Risk Management Practices*. <https://csrc.nist.gov/pubs/sp/800/161/r1/final> — Gestão de risco da cadeia de suprimentos para organizações.
3. SLSA. *Supply-chain Levels for Software Artifacts v1.0 - Threats*. <https://slsa.dev/spec/v1.0/threats> — Modelo de ameaças do código-fonte ao build e à distribuição; níveis de proveniência.
4. Ohm, M. et al. (2020). *Backstabber's Knife Collection - A Review of Open Source Software Supply Chain Attacks*. DIMVA 2020, arXiv:2005.09535. <https://arxiv.org/abs/2005.09535> — Base de dados e taxonomia de pacotes maliciosos reais.

## Relações
- (nenhuma ainda)

## Notas
- [Preflight: Verificação agêntica do último trecho para cadeias de suprimento de software com IA](/pt-br/notes/security/2026-11-24-preflight-agentic-last-mile-verification-for-ai-software-sup.md)
