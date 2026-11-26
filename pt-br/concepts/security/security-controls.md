---
type: Concept
title: 'Controles de segurança'
description: 'Medidas que reduzem risco, classificadas pela forma de implementação (gerenciais/administrativas, operacionais, técnicas, físicas) e pelo que fazem (preventivas, detectivas, corretivas, dissuasivas, compensatórias).'
tags:
- security
- governance-risk
domain: security
subdomain: governance-risk
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 15 IT Security Controls, Plans, and Procedures, §15.2 Security Controls or Safeguards, pp. 516-524'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 1 Understanding Control Types, pp. 69-75 (deterrent, compensating, combining types and goals)'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 9 Policy and Compliance; ch. 10 Defense-in-Depth Security Architectures'
- title: 'NIST (2020). SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations'
  resource: 'https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final'
  kind: standard
lang: pt-br
translation_of: /concepts/security/security-controls.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Controles de segurança

**Definição.** Stallings e Brown definem controle como uma ação, dispositivo, procedimento ou outra medida que reduz risco eliminando ou prevenindo uma violação de segurança, minimizando o dano que ela pode causar, ou descobrindo-a e reportando-a para que seja corrigida (p. 517). Eles classificam os controles em gerenciais (políticas, planejamento, normas), operacionais (procedimentos executados principalmente por pessoas) e técnicos (recursos de hardware e software), e cada um pode ser de suporte, preventivo ou de detecção e recuperação. Os guias de certificação usam um esquema paralelo: técnicos, administrativos e físicos pela implementação, e preventivos, detectivos, corretivos, dissuasivos e compensatórios pelo objetivo; um controle compensatório é uma alternativa usada quando o controle principal não é viável (Gibson, cap. 1). O NIST SP 800-53 é o grande catálogo de controles concretos agrupados em famílias.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `governance-risk`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Controles são o "tratar" do tratamento de riscos: a avaliação de riscos diz o que precisa ser reduzido, e os controles são escolhidos, implementados e depois monitorados (Stallings, fig. 15.1). Combinar tipos diferentes é defesa em profundidade: uma regra de firewall (técnica, preventiva), a revisão de logs (operacional, detectiva), a restauração de backup (corretiva) e uma câmera visível (física, dissuasiva) cobrem as lacunas umas das outras. Auditorias verificam se os controles existem e se realmente funcionam.

## Exemplo

Um sistema legado não suporta MFA, então a equipe cria um controle compensatório: ele só é acessível por um jump host que exige MFA, e cada sessão é registrada.

## Conceitos mais amplos

- [Gestão de riscos de segurança e análise de impacto no negócio](/pt-br/concepts/security/security-risk-management.md)

## Conceitos mais específicos

- [Privilégio mínimo e defesa em profundidade](/pt-br/concepts/security/least-privilege.md)
- [Controles de segurança de rede (firewalls, segmentação, IDS/IPS)](/pt-br/concepts/security/network-security.md)
- [Resposta a incidentes](/pt-br/concepts/security/incident-response.md)

## Conceitos relacionados

- [Tríade CIA (confidencialidade, integridade, disponibilidade)](/pt-br/concepts/security/cia-triad.md)

## Fontes

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 15 IT Security Controls, Plans, and Procedures, §15.2 Security Controls or Safeguards, pp. 516-524. Definição de controle, as classes gerencial/operacional/técnica e os tipos de suporte/preventivo/detecção e recuperação.
2. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 1 Understanding Control Types, pp. 69-75 (deterrent, compensating, combining types and goals). A taxonomia padrão de prova para tipos e objetivos de controles (SY0-501, 2017).
3. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 9 Policy and Compliance; ch. 10 Defense-in-Depth Security Architectures. Controles em frameworks de políticas e arquiteturas em camadas.
4. NIST (2020). *SP 800-53 Rev*. 5 - Security and Privacy Controls for Information Systems and Organizations. <https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final> — Catálogo de controles de segurança e privacidade do NIST.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
