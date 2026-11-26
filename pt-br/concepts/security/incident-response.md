---
type: Concept
title: 'Resposta a incidentes'
description: 'O processo preparado e repetível para lidar com um incidente de segurança - preparação, detecção e análise, contenção, erradicação e recuperação, e lições pós-incidente - conduzido por uma equipe definida, com runbooks, tratamento de evidências e planos de comunicação.'
tags:
- security
- security-operations
domain: security
subdomain: security-operations
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 5 Building an Incident Response Program (phases of incident response); ch. 6 Analyzing Symptoms; ch. 7 Performing Forensic Analysis; ch. 8 Recovery and Post-Incident Response'
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§17.4 Computer Security Incident Response Teams, pp. 567-574 (triage function p. 570)'
- title: 'Amazon Web Services (2019). AWS Security Incident Response Guide (whitepaper, June 2019)'
  resource: 'https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf'
  kind: docs
  locator: 'Foundation of Incident Response (educate, prepare, simulate, iterate), pp. 2-5; Prepare - People and Technology, pp. 10-25; Runbooks and Automation, pp. 29-32'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 7 Detecting, Responding to, and Recovering from Security Incidents, pp. 139-170'
- title: 'NIST (2012). SP 800-61 Rev. 2 - Computer Security Incident Handling Guide'
  resource: 'https://csrc.nist.gov/pubs/sp/800/61/r2/final'
  kind: standard
- title: 'NIST (2025). SP 800-61 Rev. 3 - Incident Response Recommendations and Considerations for Cybersecurity Risk Management'
  resource: 'https://csrc.nist.gov/pubs/sp/800/61/r3/final'
  kind: standard
lang: pt-br
translation_of: /concepts/security/incident-response.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Resposta a incidentes

**Definição.** Um incidente de segurança é um evento que compromete, de fato ou potencialmente, a confidencialidade, integridade ou disponibilidade de um sistema ou de suas informações. O ciclo clássico do NIST SP 800-61 Rev. 2, usado por Chapple e Seidl, tem quatro fases: preparação; detecção e análise; contenção, erradicação e recuperação; e atividades pós-incidente. Stallings e Brown descrevem a equipe de resposta a incidentes (CSIRT) e suas funções: detectar incidentes, fazer a triagem por um ponto único de contato, responder e documentar. O NIST SP 800-61 Rev. 3 (2025) passou a mapear a resposta a incidentes nas funções do Cybersecurity Framework 2.0, em vez de um ciclo separado. Na nuvem, o guia da AWS organiza o trabalho em educar, preparar (pessoas e tecnologia), simular e iterar, e destaca que as responsabilidades seguem o modelo de responsabilidade compartilhada.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `security-operations`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

A resposta a incidentes é onde os outros controles são testados de verdade: logs e observabilidade decidem se é possível detectar e reconstruir o que aconteceu, o menor privilégio limita o raio de impacto, e os backups decidem a velocidade da recuperação. A etapa de lições aprendidas alimenta a gestão de riscos e a modelagem de ameaças. Ensaiar importa tanto quanto o plano: o guia da AWS e Dotson (cap. 7) defendem simulações e runbooks automatizados, porque incidentes na nuvem se desenrolam mais rápido que processos manuais.

## Exemplo

Um alerta mostra uma chave de acesso usada de um país desconhecido. A equipe segue o runbook: desativa a chave (contenção), encontra e remove os recursos criados pelo atacante (erradicação), troca as credenciais relacionadas e restaura o serviço (recuperação), e depois cria uma regra que bloqueia chaves de longa duração (lições aprendidas).

## Conceitos mais amplos

- [Controles de segurança](/pt-br/concepts/security/security-controls.md)

## Conceitos relacionados

- [Observabilidade (logs, métricas, traces)](/pt-br/concepts/cloud-infra/observability.md)
- [Modelo de responsabilidade compartilhada](/pt-br/concepts/security/shared-responsibility-model.md)

## Fontes

1. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 5 Building an Incident Response Program (phases of incident response); ch. 6 Analyzing Symptoms; ch. 7 Performing Forensic Analysis; ch. 8 Recovery and Post-Incident Response. Quatro capítulos inteiros sobre montar e operar o programa de resposta, seguindo o NIST SP 800-61.
2. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §17.4 Computer Security Incident Response Teams, pp. 567-574 (triage function p. 570). Funções da CSIRT e os passos para tratar um incidente.
3. Amazon Web Services (2019). *AWS Security Incident Response Guide (whitepaper, June 2019)*. <https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf> — Foundation of Incident Response (educate, prepare, simulate, iterate), pp. 2-5; Prepare - People and Technology, pp. 10-25; Runbooks and Automation, pp. 29-32. Resposta a incidentes específica de nuvem: preparar acessos, runbooks e automação na AWS.
4. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 7 Detecting, Responding to, and Recovering from Security Incidents, pp. 139-170. O que muda na detecção e na resposta em ambientes de nuvem.
5. NIST (2012). *SP 800-61 Rev*. 2 - Computer Security Incident Handling Guide. <https://csrc.nist.gov/pubs/sp/800/61/r2/final> — O ciclo clássico de tratamento de incidentes em quatro fases.
6. NIST (2025). *SP 800-61 Rev*. 3 - Incident Response Recommendations and Considerations for Cybersecurity Risk Management. <https://csrc.nist.gov/pubs/sp/800/61/r3/final> — Orientação atual do NIST (2025), alinhada ao CSF 2.0.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
