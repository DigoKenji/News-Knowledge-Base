---
type: Concept
title: 'Modelo de responsabilidade compartilhada'
description: 'A divisão das responsabilidades de segurança entre um provedor de nuvem e seu cliente - o provedor protege a segurança "da nuvem" (instalações, hardware, virtualização), o cliente protege a segurança "na nuvem" (dados, identidades, configuração) - com a linha subindo conforme os serviços vão de IaaS a SaaS.'
tags:
- security
- cloud-security
domain: security
subdomain: cloud-security
also_in:
- cloud-infra/cloud-platforms
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Amazon Web Services (2019). AWS Security Incident Response Guide (whitepaper, June 2019)'
  resource: 'https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf'
  kind: docs
  locator: 'Shared Responsibility, pp. 3-4 (figure 1)'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 1 The Cloud Shared Responsibility Model, pp. 6-9'
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§5.9 Cloud Security Risks and Countermeasures, p. 187; §5.10 Data Protection in the Cloud, p. 189'
- title: 'NIST (2011). SP 800-144 - Guidelines on Security and Privacy in Public Cloud Computing'
  resource: 'https://csrc.nist.gov/pubs/sp/800/144/final'
  kind: standard
- title: 'Cloud Security Alliance. Security Guidance for Critical Areas of Focus in Cloud Computing'
  resource: 'https://cloudsecurityalliance.org/research/guidance'
  kind: standard
lang: pt-br
translation_of: /concepts/security/shared-responsibility-model.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Modelo de responsabilidade compartilhada

**Definição.** A AWS resume assim: a AWS cuida da segurança da nuvem, o cliente é responsável pela segurança na nuvem. Em serviços de infraestrutura, como máquinas virtuais, o provedor opera tudo das instalações físicas até a camada de virtualização, e o cliente gerencia o sistema operacional convidado e seus patches, as aplicações e a configuração dos controles do provedor, como security groups, ACLs de rede e IAM. Em serviços abstraídos (armazenamento de objetos, bancos gerenciados) o provedor também opera o SO e a plataforma, e o cliente continua cuidando dos seus dados, das opções de criptografia, da classificação e das permissões (guia da AWS, pp. 3-4). Dotson observa que a divisão depende do modelo de serviço e que outras partes, como equipes internas ou parceiros de serviços gerenciados, podem ficar com partes dela.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `cloud-security`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

A maioria das violações na nuvem acontece do lado do cliente: buckets de armazenamento públicos, chaves vazadas, papéis de IAM amplos demais. O modelo, portanto, diz onde concentrar esforço: gestão de identidades e acessos, configuração e proteção de dados. Ele também molda a resposta a incidentes, pois logs e perícia abaixo da linha dependem do que o provedor expõe. É a face de segurança dos modelos de serviço em nuvem.

## Exemplo

Uma empresa roda seu banco em um serviço gerenciado. O provedor aplica os patches do motor do banco; a empresa ainda precisa mantê-lo fora da internet pública, exigir criptografia e limitar quais papéis podem ler a tabela de clientes.

## Conceitos mais amplos

- [Modelos de serviço (IaaS, PaaS, SaaS)](/pt-br/concepts/cloud-infra/cloud-service-models.md)

## Conceitos relacionados

- [Gestão de identidades e acessos (IAM)](/pt-br/concepts/security/identity-access-management.md)
- [Resposta a incidentes](/pt-br/concepts/security/incident-response.md)

## Fontes

1. Amazon Web Services (2019). *AWS Security Incident Response Guide (whitepaper, June 2019)*. <https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf> — Shared Responsibility, pp. 3-4 (figure 1). A própria formulação do provedor e como ela muda entre serviços de infraestrutura e abstraídos.
2. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 1 The Cloud Shared Responsibility Model, pp. 6-9. Explicação neutra de fornecedor para IaaS, PaaS e SaaS.
3. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §5.9 Cloud Security Risks and Countermeasures, p. 187; §5.10 Data Protection in the Cloud, p. 189. Riscos da nuvem e os deveres de proteção de dados do cliente.
4. NIST (2011). *SP 800-144 - Guidelines on Security and Privacy in Public Cloud Computing*. <https://csrc.nist.gov/pubs/sp/800/144/final> — Diretrizes do NIST sobre segurança e privacidade em nuvem pública.
5. Cloud Security Alliance. Security Guidance for Critical Areas of Focus in Cloud Computing. <https://cloudsecurityalliance.org/research/guidance> — Orientação da CSA, um segundo órgão independente.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
