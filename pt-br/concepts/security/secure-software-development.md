---
type: Concept
title: Desenvolvimento seguro de software (SSDLC)
description: Incorporar segurança em todas as fases do ciclo de vida de desenvolvimento de software - requisitos, projeto, codificação, testes, entrega e manutenção - em vez de testá-la só no final.
tags:
- security
- appsec
- core
domain: security
subdomain: appsec
also_in:
- software-engineering/process
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2022). SP 800-218 - Secure Software Development Framework (SSDF) Version 1.1
  resource: 'https://csrc.nist.gov/pubs/sp/800/218/final'
  kind: standard
- title: OWASP. Software Assurance Maturity Model (SAMM)
  resource: 'https://owaspsamm.org/'
  kind: standard
- title: Microsoft. Security Development Lifecycle (SDL)
  resource: 'https://www.microsoft.com/en-us/securityengineering/sdl'
  kind: practitioner
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 11 Software Security, pp. 375-415 (handling program input p. 380; writing safe code p. 392)'
- title: 'Stuttard, D. and Pinto, M. (2008). The Web Application Hacker''s Handbook. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9780470170779'
  kind: book
  locator: 'ch. 2 Core Defense Mechanisms, pp. 15-34'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 12 Software Development Security'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/secure-software-development.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Desenvolvimento seguro de software (SSDLC)

**Definição.** Um ciclo de vida de desenvolvimento seguro acrescenta práticas de segurança a cada etapa: requisitos de segurança, modelagem de ameaças no projeto, padrões de codificação segura, revisão de código, análise estática e dinâmica (SAST, DAST), varredura de dependências, build e entrega seguros, e resposta a vulnerabilidades após a entrega. O Secure Software Development Framework do NIST (SSDF, SP 800-218) agrupa isso em quatro áreas: preparar a organização, proteger o software, produzir software bem protegido e responder a vulnerabilidades.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `appsec`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Corrigir uma falha no projeto custa muito menos do que corrigi-la em produção, que é o argumento do "shift left". O SDL da Microsoft (2004) popularizou a abordagem; o OWASP SAMM é um modelo de maturidade aberto para ela. Com a IA escrevendo mais código, práticas de SSDLC como varredura automática e etapas obrigatórias de revisão importam mais, porque código gerado pode carregar vulnerabilidades em escala.

## Exemplo

Um time faz a modelagem de ameaças de uma nova funcionalidade de pagamento já no projeto, roda SAST e varredura de dependências em todo pull request e tem um processo documentado para corrigir e divulgar vulnerabilidades reportadas depois do lançamento.

## Conceitos mais específicos

- [Modelagem de ameaças](/pt-br/concepts/security/threat-modeling.md)
- [Ataque à cadeia de suprimentos](/pt-br/concepts/security/supply-chain-attack.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, ch. 11 Software Security, pp. 375-415 (handling program input p. 380; writing safe code p. 392): Programação defensiva: tratamento de entrada, código seguro, interação com o SO e tratamento de saída.
- **Stuttard, D. and Pinto, M. (2008)**, *The Web Application Hacker's Handbook*, ch. 2 Core Defense Mechanisms, pp. 15-34: Controle de acesso e entrada do usuário, tratamento de atacantes e gestão da aplicação.
- **Chapple, M. and Seidl, D. (2017)**, *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*, ch. 12 Software Development Security: Atividades e testes de segurança no ciclo de desenvolvimento.

## Fontes

1. NIST (2022). *SP 800-218 - Secure Software Development Framework (SSDF) Version 1.1*. <https://csrc.nist.gov/pubs/sp/800/218/final> — Os quatro grupos de práticas e as práticas individuais.
2. OWASP. *Software Assurance Maturity Model (SAMM)*. <https://owaspsamm.org/> — Modelo de maturidade aberto para desenvolvimento seguro.
3. Microsoft. *Security Development Lifecycle (SDL)*. <https://www.microsoft.com/en-us/securityengineering/sdl> — O processo da indústria que popularizou o SSDLC.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 11 Software Security, pp. 375-415 (handling program input p. 380; writing safe code p. 392). Programação defensiva: tratamento de entrada, código seguro, interação com o SO e tratamento de saída.
5. Stuttard, D. and Pinto, M. (2008). *The Web Application Hacker's Handbook*. Wiley. <https://books.google.com/books?vid=ISBN9780470170779> — ch. 2 Core Defense Mechanisms, pp. 15-34. Controle de acesso e entrada do usuário, tratamento de atacantes e gestão da aplicação.
6. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 12 Software Development Security. Atividades e testes de segurança no ciclo de desenvolvimento.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
