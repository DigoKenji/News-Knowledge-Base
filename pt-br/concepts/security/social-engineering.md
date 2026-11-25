---
type: Concept
title: 'Engenharia social e phishing'
description: 'Ataques que manipulam pessoas em vez de sistemas - phishing, spear phishing, pretexto, tailgating - explorando confiança, autoridade, urgência ou disposição para ajudar, combatidos com treinamento de conscientização, procedimentos de verificação e controles técnicos como MFA.'
tags:
- security
- security
domain: security
subdomain: security
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Beaver, K. (2018). Hacking For Dummies, 6th edition. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9781119485476'
  kind: book
  locator: 'ch. 6 Social Engineering, pp. 67-84 (countermeasures p. 80)'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 6 Social Engineering pp. 278-294 (phishing p. 281, spear phishing p. 284, why it works p. 292)'
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§6.5 Propagation - Social Engineering, p. 218; §6.8 Phishing, p. 224; §17.1 Security Awareness, Training, and Education, p. 557'
- title: 'MITRE ATT&CK. Initial Access tactic (TA0001), incl. Phishing'
  resource: 'https://attack.mitre.org/tactics/TA0001/'
  kind: docs
lang: pt-br
translation_of: /concepts/security/social-engineering.md
---

# Engenharia social e phishing

**Definição.** Beaver chama a engenharia social de "hackear pessoas": explorar maliciosamente a natureza confiante das pessoas para obter informações ou acesso (p. 67). O phishing envia mensagens enganosas que levam pessoas a revelar credenciais, abrir malware ou aprovar pagamentos; o spear phishing mira uma pessoa específica com detalhes pesquisados; o whaling mira executivos; o pretexto inventa um cenário (uma falsa ligação do suporte de TI); variantes físicas incluem o tailgating (entrar atrás de alguém) em prédios. Gibson explica por que funciona por princípios como autoridade, intimidação, consenso, escassez, urgência, familiaridade e confiança. Stallings e Brown a tratam como via de propagação de malware (trojans, spam) e como carga de roubo de informação (phishing), e respondem com conscientização, treinamento e educação em segurança.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `security`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

O phishing está entre as técnicas de acesso inicial mais comuns (MITRE ATT&CK TA0001), por isso é muitas vezes onde um incidente começa. As defesas combinam pessoas, processos e tecnologia: treinamento e simulações de phishing, verificação por retorno de ligação para mudanças de pagamento ou senha, filtragem e autenticação de e-mail (SPF, DKIM, DMARC) e MFA resistente a phishing, para que senhas roubadas não bastem. A injeção de prompt é uma prima próxima para agentes de IA: manipular o modelo em vez da pessoa.

## Exemplo

O financeiro recebe um e-mail urgente que parece ser do CEO pedindo uma transferência no mesmo dia para um novo fornecedor. O procedimento exige ligar para o CEO em um número conhecido; a ligação mostra que o e-mail é falso.

## Conceitos mais amplos

- [Ameaça, vulnerabilidade e risco](/pt-br/concepts/security/threat-vulnerability-risk.md)

## Conceitos relacionados

- [Autenticação vs autorização](/pt-br/concepts/security/authentication-authorization.md)
- [Prompt injection indireta](/pt-br/concepts/security/indirect-prompt-injection.md)

## Fontes

1. Beaver, K. (2018). *Hacking For Dummies, 6th edition*. Wiley. <https://books.google.com/books?vid=ISBN9781119485476> — ch. 6 Social Engineering, pp. 67-84 (countermeasures p. 80). Por que atacantes a usam, como os testes são feitos e as contramedidas.
2. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 6 Social Engineering pp. 278-294 (phishing p. 281, spear phishing p. 284, why it works p. 292). Tipos de ataque e os princípios de persuasão por trás deles (SY0-501, 2017).
3. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §6.5 Propagation - Social Engineering, p. 218; §6.8 Phishing, p. 224; §17.1 Security Awareness, Training, and Education, p. 557. Engenharia social como vetor de malware e o programa de conscientização que a combate.
4. MITRE ATT&CK. Initial Access tactic (TA0001), incl. Phishing. <https://attack.mitre.org/tactics/TA0001/> — O ATT&CK classifica o phishing como técnica de acesso inicial.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
