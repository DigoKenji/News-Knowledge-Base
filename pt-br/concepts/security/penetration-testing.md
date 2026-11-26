---
type: Concept
title: 'Teste de intrusão (pentest) e avaliação de vulnerabilidades'
description: 'Tentativas autorizadas e com escopo definido de encontrar e explorar com segurança fraquezas de um sistema como um atacante faria, para provar o risco real e orientar correções - diferente de uma varredura automática de vulnerabilidades, que só lista fraquezas conhecidas.'
tags:
- security
- appsec
domain: security
subdomain: appsec
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Easttom, W. (2018). Penetration Testing Fundamentals: A Hands-On Guide to Reliable Security Audits. Pearson IT Certification'
  resource: 'https://books.google.com/books?vid=ISBN9780789759375'
  kind: book
  locator: 'ch. 1 Introduction to Penetration Testing (vulnerability scans vs penetration tests); ch. 2 Standards (PCI DSS, NIST 800-115, PTES, CREST); ch. 19 A Sample Pen Test Project'
- title: 'Beaver, K. (2018). Hacking For Dummies, 6th edition. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9781119485476'
  kind: book
  locator: 'ch. 1 Introduction to Vulnerability and Penetration Testing, pp. 7-24 (testing versus auditing p. 10; the testing process p. 18)'
- title: 'Allen, L., Heriyanto, T. and Ali, S. (2014). Kali Linux - Assuring Security by Penetration Testing, 2nd edition. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781849519489'
  kind: book
  locator: 'ch. 2 Penetration Testing Methodology, pp. 51-72 (OSSTMM p. 56); ch. 3 Target Scoping p. 73; ch. 12 Documentation and Reporting, pp. 365-380'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 8 Vulnerability Scanning p. 356 and Penetration Testing p. 359'
- title: 'NIST (2008). SP 800-115 - Technical Guide to Information Security Testing and Assessment'
  resource: 'https://csrc.nist.gov/pubs/sp/800/115/final'
  kind: standard
- title: 'OWASP. Web Security Testing Guide'
  resource: 'https://owasp.org/www-project-web-security-testing-guide/'
  kind: docs
lang: pt-br
translation_of: /concepts/security/penetration-testing.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Teste de intrusão (pentest) e avaliação de vulnerabilidades

**Definição.** Easttom define teste de intrusão como usar técnicas reais de ataque para testar a segurança de um sistema-alvo, e destaca que pentest e hacking não são sinônimos: a diferença é a permissão por escrito, um escopo e regras de engajamento acordados, e um relatório. Uma varredura de vulnerabilidades detecta automaticamente vulnerabilidades conhecidas e deve rodar com regularidade; um pentest vai além, encadeando achados para mostrar o que um atacante realmente alcançaria. O NIST SP 800-115 descreve quatro fases (planejamento, descoberta, ataque, relatório); metodologias como PTES, OSSTMM e o OWASP Web Security Testing Guide detalham os passos. Os testes podem ser caixa-preta (sem conhecimento interno), caixa-cinza ou caixa-branca, e podem incluir engenharia social se o escopo permitir.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `appsec`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

O pentest é a verificação ofensiva de tudo o que é defensivo neste domínio: valida controles, alimenta a gestão de vulnerabilidades com achados priorizados e é exigido por alguns padrões (PCI DSS). Seu resultado vale o que vale o relatório: visões executiva, gerencial e técnica com evidências reproduzíveis e recomendações de correção (Allen et al., cap. 12). Os resultados são um retrato pontual, por isso varreduras contínuas e testes periódicos se complementam. Vários livros da biblioteca do MBA são manuais de ferramentas ofensivas; esta base guarda só os conceitos, não procedimentos de ataque.

## Exemplo

Antes de lançar um portal de clientes, uma empresa contrata testadores com escopo assinado cobrindo só a aplicação web e sua API. Eles encontram uma falha de controle de acesso que permite a um cliente ler as faturas de outro, demonstram com uma conta de teste, e o relatório a classifica como crítica, com a correção.

## Conceitos mais amplos

- [Ameaça, vulnerabilidade e risco](/pt-br/concepts/security/threat-vulnerability-risk.md)

## Conceitos relacionados

- [Gestão de vulnerabilidades](/pt-br/concepts/security/vulnerability-management.md)
- [Classes comuns de vulnerabilidade](/pt-br/concepts/security/common-vulnerability-classes.md)

## Fontes

1. Easttom, W. (2018). *Penetration Testing Fundamentals: A Hands-On Guide to Reliable Security Audits*. Pearson IT Certification. <https://books.google.com/books?vid=ISBN9780789759375> — ch. 1 Introduction to Penetration Testing (vulnerability scans vs penetration tests); ch. 2 Standards (PCI DSS, NIST 800-115, PTES, CREST); ch. 19 A Sample Pen Test Project. Definições, padrões e um teste completo de exemplo; o mais metódico dos livros de pentest.
2. Beaver, K. (2018). *Hacking For Dummies, 6th edition*. Wiley. <https://books.google.com/books?vid=ISBN9781119485476> — ch. 1 Introduction to Vulnerability and Penetration Testing, pp. 7-24 (testing versus auditing p. 10; the testing process p. 18). Visão em linguagem simples de testes éticos, permissão e processo.
3. Allen, L., Heriyanto, T. and Ali, S. (2014). *Kali Linux - Assuring Security by Penetration Testing, 2nd edition*. Packt. <https://books.google.com/books?vid=ISBN9781849519489> — ch. 2 Penetration Testing Methodology, pp. 51-72 (OSSTMM p. 56); ch. 3 Target Scoping p. 73; ch. 12 Documentation and Reporting, pp. 365-380. Metodologias, definição de escopo e como escrever os três tipos de relatório.
4. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 8 Vulnerability Scanning p. 356 and Penetration Testing p. 359. Contraste em nível de prova entre varreduras e pentests (SY0-501, 2017).
5. NIST (2008). *SP 800-115 - Technical Guide to Information Security Testing and Assessment*. <https://csrc.nist.gov/pubs/sp/800/115/final> — Guia de testes e avaliação do NIST (modelo de quatro fases).
6. OWASP. Web Security Testing Guide. <https://owasp.org/www-project-web-security-testing-guide/> — Metodologia OWASP para testes de aplicações web.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
