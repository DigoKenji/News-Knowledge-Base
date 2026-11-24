---
type: Concept
title: Tríade CIA (confidencialidade, integridade, disponibilidade)
description: Os três objetivos centrais da segurança da informação - confidencialidade (só partes autorizadas leem a informação), integridade (a informação não é alterada indevidamente) e disponibilidade (informação e sistemas podem ser usados quando necessário).
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
- title: ISO/IEC (2018). ISO/IEC 27000 - Information security management systems - Overview and vocabulary
  resource: 'https://www.iso.org/standard/73906.html'
  kind: standard
- title: NIST (2004). FIPS 199 - Standards for Security Categorization of Federal Information and Information Systems
  resource: 'https://csrc.nist.gov/pubs/fips/199/final'
  kind: standard
- title: NIST (2017). SP 800-12 Rev. 1 - An Introduction to Information Security
  resource: 'https://csrc.nist.gov/pubs/sp/800/12/r1/final'
  kind: standard
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/cia-triad.md
---

# Tríade CIA (confidencialidade, integridade, disponibilidade)

**Definição.** A ISO/IEC 27000 define segurança da informação como a preservação da confidencialidade, integridade e disponibilidade da informação. O NIST FIPS 199 usa os mesmos três objetivos para classificar a gravidade de uma perda para cada sistema de informação (impacto baixo, moderado, alto). Alguns frameworks acrescentam propriedades como autenticidade, responsabilização e não repúdio.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Todo controle de segurança protege pelo menos um dos três: criptografia e controle de acesso protegem a confidencialidade; hashes, assinaturas e controle de mudanças protegem a integridade; redundância, backups e proteção contra DDoS protegem a disponibilidade. Podem entrar em conflito: controles de confidencialidade mais rígidos podem prejudicar a disponibilidade. A modelagem de ameaças pergunta, para cada parte de um sistema, qual dos três um atacante poderia quebrar.

## Exemplo

Um banco de clientes vazado quebra a confidencialidade, um valor de fatura adulterado quebra a integridade, e um ransomware que tranca os arquivos quebra a disponibilidade.

## Conceitos mais específicos

- [Modelagem de ameaças](/pt-br/concepts/security/threat-modeling.md)
- [Arquitetura zero-trust](/pt-br/concepts/security/zero-trust-architecture.md)
- [Controle de fluxo de informação](/pt-br/concepts/security/information-flow-control.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. ISO/IEC (2018). *ISO/IEC 27000 - Information security management systems - Overview and vocabulary*. <https://www.iso.org/standard/73906.html> — Segurança da informação como preservação de confidencialidade, integridade e disponibilidade.
2. NIST (2004). *FIPS 199 - Standards for Security Categorization of Federal Information and Information Systems*. <https://csrc.nist.gov/pubs/fips/199/final> — Os três objetivos de segurança e níveis de impacto.
3. NIST (2017). *SP 800-12 Rev. 1 - An Introduction to Information Security*. <https://csrc.nist.gov/pubs/sp/800/12/r1/final> — Explicação introdutória dos objetivos e controles de segurança.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
