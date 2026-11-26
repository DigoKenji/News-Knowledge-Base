---
type: Concept
title: Privilégio mínimo e defesa em profundidade
description: Dois princípios básicos de projeto - dar a cada usuário, serviço e programa só o acesso de que precisa para sua tarefa (privilégio mínimo), e proteger ativos com várias camadas independentes de controles para que uma falha não exponha tudo (defesa em profundidade).
tags:
- security
- security-foundations
- core
domain: security
subdomain: security-foundations
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
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§1.4 Fundamental Security Design Principles, pp. 27-31'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 1 Least Privilege and Defense in Depth, pp. 1-6'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 10 Defense-in-Depth Security Architectures'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/least-privilege.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Privilégio mínimo e defesa em profundidade

**Definição.** Saltzer e Schroeder (1975) incluíram o privilégio mínimo entre seus princípios de projeto para proteção: todo programa e usuário deve operar com o menor conjunto de privilégios necessário para fazer o trabalho. O NIST SP 800-53 o transforma no controle AC-6. Defesa em profundidade, termo da estratégia militar, significa sobrepor controles de pessoas, tecnologia e operações para que um atacante precise vencer vários deles.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `security-foundations`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

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

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §1.4 Fundamental Security Design Principles, pp. 27-31: Menor privilégio, separação de privilégio, padrões seguros em falha, mediação completa, camadas.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 1 Least Privilege and Defense in Depth, pp. 1-6: Os dois princípios aplicados a ambientes de nuvem.
- **Chapple, M. and Seidl, D. (2017)**, *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*, ch. 10 Defense-in-Depth Security Architectures: Arquitetura de segurança em camadas na visão do analista.

## Fontes

1. Saltzer, J. H. and Schroeder, M. D. (1975). *The Protection of Information in Computer Systems*. Proceedings of the IEEE 63(9). <https://web.mit.edu/Saltzer/www/publications/protection/> — Privilégio mínimo e os outros princípios clássicos de projeto seguro.
2. NIST (2020). *SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations* (AC-6 Least Privilege). <https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final> — Privilégio mínimo como controle formal.
3. NIST CSRC. *Glossary - defense-in-depth*. <https://csrc.nist.gov/glossary/term/defense_in_depth> — Definição de defesa em profundidade.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §1.4 Fundamental Security Design Principles, pp. 27-31. Menor privilégio, separação de privilégio, padrões seguros em falha, mediação completa, camadas.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 1 Least Privilege and Defense in Depth, pp. 1-6. Os dois princípios aplicados a ambientes de nuvem.
6. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 10 Defense-in-Depth Security Architectures. Arquitetura de segurança em camadas na visão do analista.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
