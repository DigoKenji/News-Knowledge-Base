---
type: Concept
title: 'Controles de segurança de rede (firewalls, segmentação, IDS/IPS)'
description: 'Controles que restringem e observam o tráfego entre redes e hosts - firewalls que aplicam uma política de acesso, segmentação e DMZs que limitam o que cada zona alcança, VPNs que protegem o tráfego em trânsito, e sistemas de detecção ou prevenção de intrusão que identificam ataques.'
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
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 8 Intrusion Detection, pp. 267-303; ch. 9 Firewalls and Intrusion Prevention Systems, pp. 304-335 (types of firewalls p. 308; DMZ and configurations p. 317; IPS p. 322)'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 6 Network Security, pp. 109-137 (firewalls and network segmentation p. 121)'
- title: 'Davies, G. (2019). Networking Fundamentals (Microsoft MTA 98-366). Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781838643508'
  kind: book
  locator: 'ch. 1 Firewalls p. 12, DMZ p. 26, VPNs p. 27; ch. 9 OSI Model p. 250; ch. 10 TCP/IP p. 268; ch. 14 Packet-filtering firewalls p. 407'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 10 Defense-in-Depth Security Architectures'
- title: 'NIST (2009). SP 800-41 Rev. 1 - Guidelines on Firewalls and Firewall Policy'
  resource: 'https://csrc.nist.gov/pubs/sp/800/41/r1/final'
  kind: standard
lang: pt-br
translation_of: /concepts/security/network-security.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Controles de segurança de rede (firewalls, segmentação, IDS/IPS)

**Definição.** Um firewall fica entre redes e aplica uma política de acesso ao tráfego que passa por ele. Stallings e Brown descrevem os tipos principais: filtragem de pacotes (regras sobre endereços, portas e protocolo), inspeção com estado (acompanha conexões), gateways de aplicação e de circuito (proxies), além de firewalls de host e pessoais; e onde posicioná-los, incluindo uma DMZ para servidores públicos entre um firewall externo e um interno. A segmentação divide a rede em zonas (VLANs, sub-redes, security groups na nuvem) para que o comprometimento de uma zona não alcance tudo. Um sistema de detecção de intrusão (IDS) observa a atividade do host ou da rede por assinatura (uso indevido) ou por anomalia e gera alertas; um sistema de prevenção de intrusão (IPS) fica em linha e pode bloquear. Os fundamentos de redes de Davies (camadas OSI e TCP/IP, VLANs, VPNs, DMZ) são o vocabulário em que esses controles são escritos.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `security-operations`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Controles de rede são uma camada da defesa em profundidade, não a defesa inteira: o modelo de perímetro (dentro confiável, fora não confiável) falha com nuvem, trabalho remoto e movimento lateral, e por isso a arquitetura zero trust leva a decisão para a identidade e para cada requisição. Na nuvem as mesmas ideias reaparecem como security groups, ACLs de rede e sub-redes privadas (Dotson, cap. 6). Os alertas de IDS alimentam o monitoramento de segurança e a resposta a incidentes.

## Exemplo

Uma loja virtual coloca seus servidores web em uma DMZ que só aceita HTTPS da internet, permite que só a camada web alcance a sub-rede do banco na porta do banco, e roda um IDS de rede que alerta sobre assinaturas de SQL injection.

## Conceitos mais amplos

- [Controles de segurança](/pt-br/concepts/security/security-controls.md)

## Conceitos relacionados

- [Arquitetura zero-trust](/pt-br/concepts/security/zero-trust-architecture.md)
- [Privilégio mínimo e defesa em profundidade](/pt-br/concepts/security/least-privilege.md)

## Fontes

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 8 Intrusion Detection, pp. 267-303; ch. 9 Firewalls and Intrusion Prevention Systems, pp. 304-335 (types of firewalls p. 308; DMZ and configurations p. 317; IPS p. 322). Tipos e posicionamento de firewalls, abordagens de análise de IDS, honeypots e IPS.
2. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 6 Network Security, pp. 109-137 (firewalls and network segmentation p. 121). Como os mesmos controles aparecem em redes de nuvem.
3. Davies, G. (2019). *Networking Fundamentals (Microsoft MTA 98-366)*. Packt. <https://books.google.com/books?vid=ISBN9781838643508> — ch. 1 Firewalls p. 12, DMZ p. 26, VPNs p. 27; ch. 9 OSI Model p. 250; ch. 10 TCP/IP p. 268; ch. 14 Packet-filtering firewalls p. 407. O vocabulário de redes por baixo dos controles (nível MTA 98-366).
4. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 10 Defense-in-Depth Security Architectures. Segmentação e desenho de rede em camadas para analistas.
5. NIST (2009). *SP 800-41 Rev*. 1 - Guidelines on Firewalls and Firewall Policy. <https://csrc.nist.gov/pubs/sp/800/41/r1/final> — Diretrizes do NIST sobre firewalls e políticas de firewall.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
