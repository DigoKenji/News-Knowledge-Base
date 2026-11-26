---
type: Concept
title: 'Gestão de riscos de segurança e análise de impacto no negócio'
description: 'O ciclo contínuo de identificar ativos, ameaças e vulnerabilidades, avaliar probabilidade e impacto (incluindo uma análise de impacto no negócio dos processos críticos), escolher um tratamento - mitigar, transferir, evitar ou aceitar - e monitorar o resultado.'
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
  locator: 'ch. 14 IT Security Management and Risk Assessment, pp. 485-514 (security risk assessment p. 492; detailed risk analysis p. 495); ch. 15 Monitoring Risks, p. 526'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 8 Understanding Risk Management, pp. 344-356; ch. 9 Business Impact Analysis Concepts, pp. 406-412'
- title: 'Course handout (n.d.). Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)'
  resource: 'FIAP-MBA/Materiais/Referencias (local copy, no public URL)'
  kind: handout
  locator: 'pp. 1-23 (whole handout)'
- title: 'NIST (2012). SP 800-30 Rev. 1 - Guide for Conducting Risk Assessments'
  resource: 'https://csrc.nist.gov/pubs/sp/800/30/r1/final'
  kind: standard
- title: 'NIST (2010). SP 800-34 Rev. 1 - Contingency Planning Guide for Federal Information Systems'
  resource: 'https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final'
  kind: standard
lang: pt-br
translation_of: /concepts/security/security-risk-management.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Gestão de riscos de segurança e análise de impacto no negócio

**Definição.** Stallings e Brown apresentam a gestão de segurança de TI como um ciclo: definir o contexto organizacional e a política, avaliar riscos, escolher e implementar controles, e monitorar (caps. 14-15). A avaliação de riscos pode ser baseline, informal, detalhada ou combinada; uma análise detalhada identifica ativos, ameaças e vulnerabilidades, estima probabilidade e consequência, e registra o nível de risco resultante em um registro de riscos. Cada risco recebe então uma resposta: aceitar, evitar, mitigar (reduzir com controles) ou compartilhar/transferir (por exemplo, seguro). Uma análise de impacto no negócio (BIA) pergunta quanto a perda de cada processo prejudicaria a organização ao longo do tempo, e define metas de recuperação como RTO (em quanto tempo) e RPO (quantos dados podem ser perdidos) (Gibson, cap. 9). A apostila de riscos da FIAP acrescenta a visão de auditoria: controles devem ser eficazes e justificados em custo, lacunas são encontradas por análise de gap, e o SGSI melhora pelo ciclo PDCA (planejar-fazer-checar-agir).

Domínio: [security](/pt-br/domains/security.md) · subdomínio `governance-risk`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Este conceito é a camada de gestão acima de Ameaça, vulnerabilidade e risco: aqueles termos definem o risco, este é o que a organização faz a respeito. Ele se conecta ao trabalho de arquitetura do MBA: uma BIA usa os mesmos processos e capacidades de negócio que a arquitetura de negócio mapeia, e o apetite a risco é uma decisão de governança. O NIST SP 800-30 e a ISO/IEC 27005 são os métodos padrão.

## Exemplo

Uma BIA mostra que o processamento de pedidos perde R$ 200 mil por hora parado, enquanto a intranet quase nada perde. O processamento de pedidos recebe RTO de 1 hora e um banco replicado (mitigar); o risco da intranet é aceito; o risco de vazamento de dados de cartão é em parte transferido com seguro cibernético.

## Conceitos mais amplos

- [Ameaça, vulnerabilidade e risco](/pt-br/concepts/security/threat-vulnerability-risk.md)

## Conceitos mais específicos

- [Controles de segurança](/pt-br/concepts/security/security-controls.md)

## Conceitos relacionados

- [Modelagem de ameaças](/pt-br/concepts/security/threat-modeling.md)

## Fontes

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 14 IT Security Management and Risk Assessment, pp. 485-514 (security risk assessment p. 492; detailed risk analysis p. 495); ch. 15 Monitoring Risks, p. 526. Ciclo completo de gestão, abordagens de avaliação de risco, registro de riscos e as quatro respostas a risco.
2. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 8 Understanding Risk Management, pp. 344-356; ch. 9 Business Impact Analysis Concepts, pp. 406-412. Tipos de resposta a risco e termos de BIA (RTO, RPO, MTBF) em nível de prova (SY0-501, 2017).
3. Course handout (n.d.). *Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)*. — pp. 1-23 (whole handout). Perspectiva de auditoria: etapas da análise de risco, matriz de tratamento, análise de gap e PDCA.
4. NIST (2012). *SP 800-30 Rev*. 1 - Guide for Conducting Risk Assessments. <https://csrc.nist.gov/pubs/sp/800/30/r1/final> — Método de avaliação de riscos do NIST.
5. NIST (2010). *SP 800-34 Rev*. 1 - Contingency Planning Guide for Federal Information Systems. <https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final> — Planejamento de contingência e o processo de BIA.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
