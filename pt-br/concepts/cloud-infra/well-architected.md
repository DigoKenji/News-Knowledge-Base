---
type: Concept
title: Well-Architected Framework
description: 'Frameworks dos provedores de nuvem (AWS, Azure, Google Cloud) com pilares e perguntas de revisão - confiabilidade, segurança, custo, desempenho, operação, sustentabilidade - para avaliar cargas de trabalho.'
tags:
- cloud-infra
- cloud-platforms
domain: cloud-infra
subdomain: cloud-platforms
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'AWS. AWS Well-Architected Framework'
  resource: 'https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html'
  kind: docs
- title: 'Microsoft. Azure Well-Architected Framework'
  resource: 'https://learn.microsoft.com/en-us/azure/well-architected/'
  kind: docs
- title: 'Google Cloud. Google Cloud Well-Architected Framework'
  resource: 'https://cloud.google.com/architecture/framework'
  kind: docs
lang: pt-br
translation_of: /concepts/cloud-infra/well-architected.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Well-Architected Framework

**Definição.** Um framework well-architected é o conjunto de princípios de design, boas práticas e perguntas de revisão de um provedor de nuvem para avaliar uma carga de trabalho. A AWS introduziu a ideia e a organiza em seis pilares: excelência operacional, segurança, confiabilidade, eficiência de desempenho, otimização de custos e sustentabilidade. Microsoft Azure e Google Cloud publicam frameworks equivalentes com pilares parecidos, além de ferramentas de revisão que pontuam a carga e listam melhorias.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `cloud-platforms`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Os pilares são atributos de qualidade concretizados para a nuvem, então os frameworks funcionam como uma revisão de arquitetura em formato de checklist, entre métodos mais profundos como o ATAM. São específicos do provedor nas recomendações de serviços, mas em boa parte portáveis nas perguntas.

## Exemplo

Uma revisão de um banco em uma única zona de disponibilidade aponta um risco de confiabilidade (sem failover) e um achado de custo (instâncias superdimensionadas), cada um com a correção recomendada pelo provedor.

## Fontes

1. AWS. AWS Well-Architected Framework. <https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html> — Os seis pilares e o processo de revisão.
2. Microsoft. Azure Well-Architected Framework. <https://learn.microsoft.com/en-us/azure/well-architected/> — Os pilares e a avaliação do Azure.
3. Google Cloud. Google Cloud Well-Architected Framework. <https://cloud.google.com/architecture/framework> — Os pilares e princípios de design do Google Cloud.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
