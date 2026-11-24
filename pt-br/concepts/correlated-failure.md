---
type: Concept
title: Falha correlacionada
description: Vários componentes que deveriam falhar de forma independente falham juntos porque compartilham uma causa oculta - uma dependência, uma mudança de configuração, um bug de software, um domínio de energia ou rede - o que derrota a redundância projetada supondo independência.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Ford, D. et al. (2010). Availability in Globally Distributed Storage Systems. OSDI 2010
  resource: 'https://www.usenix.org/conference/osdi10/availability-globally-distributed-storage-systems'
  kind: paper
- title: AWS (2022). AWS Fault Isolation Boundaries. Whitepaper
  resource: 'https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/abstract-and-introduction.html'
  kind: practitioner
- title: AWS (2021). Summary of the AWS Service Event in the Northern Virginia (US-EAST-1) Region
  resource: 'https://aws.amazon.com/message/12721/'
  kind: practitioner
lang: pt-br
translation_of: /concepts/correlated-failure.md
---

# Falha correlacionada

**Definição.** Falhas são correlacionadas quando a falha de um componente torna mais provável a falha de outro. A matemática da redundância (duas réplicas de 99,9% dão 99,9999%) supõe independência; falhas correlacionadas quebram essa suposição, então a disponibilidade real fica muito abaixo do que a conta diz.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

Causas compartilhadas típicas: a mesma versão de software ou configuração ruim aplicada em todo lugar de uma vez; uma dependência comum (DNS, identidade, um plano de controle, um serviço de uma única região do qual recursos "globais" dependem); domínios físicos comuns (energia do rack, switch, zona de disponibilidade); e carga compartilhada, em que o tráfego de failover sobrecarrega os sobreviventes. Ford et al. viram no Google que rajadas de falhas em muitos nós, muitas vezes num mesmo rack, dominam a indisponibilidade de dados, e que modelos que supõem independência subestimam a indisponibilidade em ordens de grandeza. Defesas: espalhar réplicas por domínios de falha (racks, zonas, regiões, até provedores), escalonar deploys (células, ondas, canários), remover dependências ocultas entre regiões e testar o failover de verdade.

## Por que importa e trade-offs

Falha correlacionada é o motivo pelo qual arquiteturas multirregião ainda caem juntas. Reduzi-la custa dinheiro (mais domínios, mais réplicas), desacelera os deploys (ondas escalonadas) e exige um mapa de dependências que facilmente fica desatualizado.

## Exemplo

Duas regiões estão "ativas", mas ambas leem feature flags de um serviço hospedado só em us-east-1; quando essa região tem um incidente, as duas falham ao mesmo tempo.

## Fontes

1. Ford, D. et al. (2010). *Availability in Globally Distributed Storage Systems*. OSDI 2010. <https://www.usenix.org/conference/osdi10/availability-globally-distributed-storage-systems> — Rajadas de falhas correlacionadas medidas no Google e seu impacto em modelos de disponibilidade.
2. AWS (2022). *AWS Fault Isolation Boundaries*. Whitepaper. <https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/abstract-and-introduction.html> — Zonas, regiões e células como domínios de falha independentes; dependências globais ocultas.
3. AWS (2021). *Summary of the AWS Service Event in the Northern Virginia (US-EAST-1) Region*. <https://aws.amazon.com/message/12721/> — Um incidente real em que um problema de rede interna numa região afetou muitos serviços dependentes ao mesmo tempo.

## Relações
- (nenhuma ainda)

## Notas
- [Construindo confiabilidade na nuvem: a observabilidade como plano de controle para o failover multirregional](/pt-br/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md)
