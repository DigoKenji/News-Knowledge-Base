---
type: Concept
title: Atributos de qualidade e trade-offs
description: As propriedades mensuráveis que dizem quão bem um sistema funciona - desempenho, disponibilidade, segurança, modificabilidade, escalabilidade, custo e assim por diante - que guiam a arquitetura e geralmente competem entre si.
tags:
- architecture
- core
domain: architecture
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC (2023). ISO/IEC 25010 - Product quality model
  resource: 'https://www.iso.org/standard/78176.html'
  kind: standard
- title: Bass, L., Clements, P. and Kazman, R. (2021). Software Architecture in Practice, 4th ed. Addison-Wesley
  resource: 'https://www.oreilly.com/library/view/software-architecture-in/9780136885979/'
  kind: book
- title: AWS. AWS Well-Architected Framework
  resource: 'https://aws.amazon.com/architecture/well-architected/'
  kind: practitioner
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 9 Architecture Is Selling Options (An Architecture Option: Elasticity; Strike Prices)'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/architecture/quality-attributes.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Atributos de qualidade e trade-offs

**Definição.** A ISO/IEC 25010 define um modelo de qualidade de produto com características como adequação funcional, eficiência de desempenho, compatibilidade, capacidade de interação, confiabilidade, segurança, manutenibilidade, flexibilidade e proteção (safety). Bass, Clements e Kazman as tornam concretas com cenários de atributo de qualidade: estímulo, ambiente, resposta e uma medida de resposta mensurável.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Arquitetura é sobretudo a arte de equilibrar atributos de qualidade: cache melhora desempenho mas arrisca consistência; criptografia melhora segurança mas custa latência; mais redundância melhora disponibilidade mas custa dinheiro. Métodos como ATAM e frameworks como o AWS Well-Architected (organizado por pilares) existem para tornar esses trade-offs explícitos.

## Exemplo

Cenário - "quando 500 usuários buscam ao mesmo tempo em operação normal, 95% dos resultados voltam em menos de 1 segundo." Isso é testável, ao contrário de "o sistema deve ser rápido".

## Conceitos mais específicos

- [Análise de trade-offs ATAM](/pt-br/concepts/backend-systems/atam.md)
- [Well-Architected Framework](/pt-br/concepts/cloud-infra/well-architected.md)
- [Orçamento de erros de SLO](/pt-br/concepts/cloud-infra/slo-error-budget.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 9 Architecture Is Selling Options (An Architecture Option: Elasticity; Strike Prices): Atributos de qualidade como elasticidade vistos como opções com prêmio antecipado e preço de exercício, que valem mais quando a demanda é incerta.

## Fontes

1. ISO/IEC (2023). *ISO/IEC 25010 - Product quality model*. <https://www.iso.org/standard/78176.html> — Características de qualidade padronizadas (pago; citado).
2. Bass, L., Clements, P. and Kazman, R. (2021). *Software Architecture in Practice*, 4th ed. Addison-Wesley. <https://www.oreilly.com/library/view/software-architecture-in/9780136885979/> — Cenários e táticas de atributos de qualidade.
3. AWS. *AWS Well-Architected Framework*. <https://aws.amazon.com/architecture/well-architected/> — Atributos de qualidade organizados em pilares com orientação sobre trade-offs.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 9 Architecture Is Selling Options (An Architecture Option: Elasticity; Strike Prices). Atributos de qualidade como elasticidade vistos como opções com prêmio antecipado e preço de exercício, que valem mais quando a demanda é incerta.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
