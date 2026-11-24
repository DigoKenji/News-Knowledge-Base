---
type: Concept
title: Privilégio mínimo e defesa em profundidade
description: Dois princípios básicos de projeto - dar a cada usuário, serviço e programa só o acesso de que precisa para sua tarefa (privilégio mínimo), e proteger ativos com várias camadas independentes de controles para que uma falha não exponha tudo (defesa em profundidade).
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
- title: Saltzer, J. H. and Schroeder, M. D. (1975). The Protection of Information in Computer Systems. Proceedings of the IEEE 63(9)
  resource: 'https://web.mit.edu/Saltzer/www/publications/protection/'
  kind: paper
- title: NIST (2020). SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations (AC-6 Least Privilege)
  resource: 'https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final'
  kind: standard
- title: NIST CSRC. Glossary - defense-in-depth
  resource: 'https://csrc.nist.gov/glossary/term/defense_in_depth'
  kind: standard
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/least-privilege.md
---

# Privilégio mínimo e defesa em profundidade

**Definição.** Saltzer e Schroeder (1975) incluíram o privilégio mínimo entre seus princípios de projeto para proteção: todo programa e usuário deve operar com o menor conjunto de privilégios necessário para fazer o trabalho. O NIST SP 800-53 o transforma no controle AC-6. Defesa em profundidade, termo da estratégia militar, significa sobrepor controles de pessoas, tecnologia e operações para que um atacante precise vencer vários deles.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Os dois princípios limitam o raio de impacto: quando (não se) algo for comprometido, o dano fica pequeno. São a base da arquitetura zero trust e são especialmente importantes para agentes de IA, que devem receber só as ferramentas e permissões que uma tarefa exige. O custo é atrito: permissões granulares dão trabalho para definir e manter, e camadas demais atrasam as pessoas.

## Exemplo

A tarefa da execução diária só pode fazer push no próprio branch, não no main (privilégio mínimo); e mesmo que fosse enganada, o validador, a revisão do pull request e a aprovação de Kenji são camadas adicionais (defesa em profundidade).

## Conceitos mais específicos

- [Arquitetura zero-trust](/pt-br/concepts/security/zero-trust-architecture.md)
- [Controle de fluxo de informação](/pt-br/concepts/security/information-flow-control.md)
- [Modelagem de ameaças](/pt-br/concepts/security/threat-modeling.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. Saltzer, J. H. and Schroeder, M. D. (1975). *The Protection of Information in Computer Systems*. Proceedings of the IEEE 63(9). <https://web.mit.edu/Saltzer/www/publications/protection/> — Privilégio mínimo e os outros princípios clássicos de projeto seguro.
2. NIST (2020). *SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations* (AC-6 Least Privilege). <https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final> — Privilégio mínimo como controle formal.
3. NIST CSRC. *Glossary - defense-in-depth*. <https://csrc.nist.gov/glossary/term/defense_in_depth> — Definição de defesa em profundidade.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
