---
type: Concept
title: Computação em nuvem
description: Um modelo de acesso sob demanda, via rede, a um conjunto compartilhado de recursos computacionais configuráveis (servidores, armazenamento, redes, aplicações) que podem ser provisionados e liberados rapidamente com mínimo esforço de gestão, pagando pelo que se usa.
tags:
- cloud-infra
- cloud-platforms
- core
domain: cloud-infra
subdomain: cloud-platforms
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Mell, P. and Grance, T. (2011). NIST SP 800-145 - The NIST Definition of Cloud Computing. National Institute of Standards and Technology
  resource: 'https://doi.org/10.6028/NIST.SP.800-145'
  kind: standard
- title: ISO/IEC (2023). ISO/IEC 22123-1 - Information technology - Cloud computing - Part 1 Vocabulary
  resource: 'https://www.iso.org/standard/82758.html'
  kind: standard
- title: Armbrust, M. et al. (2010). A View of Cloud Computing. Communications of the ACM 53(4)
  resource: 'https://doi.org/10.1145/1721654.1721672'
  kind: paper
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§5.8 Cloud Computing, pp. 180-187'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/cloud-infra/cloud-computing.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Computação em nuvem

**Definição.** A NIST SP 800-145 define computação em nuvem como um modelo que permite acesso ubíquo, conveniente e sob demanda, via rede, a um conjunto compartilhado de recursos computacionais configuráveis que podem ser rapidamente provisionados e liberados com mínimo esforço de gestão ou interação com o provedor. Ela nomeia cinco características essenciais (autoatendimento sob demanda, amplo acesso à rede, agrupamento de recursos, elasticidade rápida, serviço medido), três modelos de serviço e quatro modelos de implantação. A ISO/IEC 22123-1 fornece o vocabulário internacional.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `cloud-platforms`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A nuvem transformou a infraestrutura de compra de capital em despesa operacional e trouxe elasticidade, serviços gerenciados e alcance global a equipes pequenas. Armbrust et al. enquadraram a economia: a ilusão de capacidade infinita, nenhum compromisso antecipado e pagamento por uso. Os trade-offs são disciplina de custos (FinOps), responsabilidade compartilhada pela segurança e alguma dependência de um provedor.

## Exemplo

Uma startup lança num provedor de nuvem sem hardware: sobe duas pequenas máquinas virtuais e um banco gerenciado por API, escala para 40 máquinas no dia do lançamento e volta a duas na semana seguinte, pagando só pelas horas usadas.

## Conceitos mais específicos

- [Modelos de serviço (IaaS, PaaS, SaaS)](/pt-br/concepts/cloud-infra/cloud-service-models.md)
- [Modelos de implantação (pública, privada, híbrida)](/pt-br/concepts/cloud-infra/cloud-deployment-models.md)
- [Serverless (sem servidor)](/pt-br/concepts/cloud-infra/serverless.md)
- [Well-Architected Framework](/pt-br/concepts/cloud-infra/well-architected.md)
- [FinOps e custo de nuvem](/pt-br/concepts/cloud-infra/finops.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §5.8 Cloud Computing, pp. 180-187: Características, modelos de serviço e de implantação de nuvem do NIST em um livro de segurança.

## Fontes

1. Mell, P. and Grance, T. (2011). *NIST SP 800-145 - The NIST Definition of Cloud Computing*. National Institute of Standards and Technology. <https://doi.org/10.6028/NIST.SP.800-145> — Definição, cinco características essenciais, modelos de serviço e de implantação.
2. ISO/IEC (2023). *ISO/IEC 22123-1 - Information technology - Cloud computing - Part 1 Vocabulary*. <https://www.iso.org/standard/82758.html> — Vocabulário internacional de nuvem (pago; citado, não transcrito).
3. Armbrust, M. et al. (2010). *A View of Cloud Computing*. Communications of the ACM 53(4). <https://doi.org/10.1145/1721654.1721672> — Economia da computação em nuvem e seus obstáculos.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §5.8 Cloud Computing, pp. 180-187. Características, modelos de serviço e de implantação de nuvem do NIST em um livro de segurança.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
