---
type: Concept
title: Engenharia do caos
description: A prática de rodar experimentos controlados que injetam falhas reais num sistema (muitas vezes em produção) para testar uma hipótese sobre como ele continua funcionando, para que fraquezas sejam encontradas antes de causar quedas.
tags:
- backend-systems
- reliability
domain: backend-systems
subdomain: reliability
also_in:
- software-engineering/testing
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Principles of Chaos Engineering (2019)
  resource: 'https://principlesofchaos.org/'
  kind: practitioner
- title: 'Basiri, A. et al. (2016). Chaos Engineering. IEEE Software 33(3), arXiv:1702.05843'
  resource: 'https://arxiv.org/abs/1702.05843'
  kind: paper
- title: AWS. What is AWS Fault Injection Service?
  resource: 'https://docs.aws.amazon.com/fis/latest/userguide/what-is.html'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/chaos-engineering.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Engenharia do caos

**Definição.** Chaos engineering é "a disciplina de experimentar num sistema para criar confiança na sua capacidade de resistir a condições turbulentas em produção" (Principles of Chaos). É um experimento, não quebra aleatória: você define o comportamento normal, prevê que ele se mantém sob uma falha, injeta a falha com raio de impacto limitado e verifica se estava certo.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `reliability`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Passos: (1) definir um estado estável com métricas de negócio (pedidos por minuto, inícios de reprodução), não só CPU; (2) supor que ele se mantém tanto no grupo de controle quanto no experimental; (3) injetar eventos realistas (perda de instância ou zona, latência, erros de dependência, desvio de relógio); (4) procurar diferença. Princípios avançados: rodar em produção, automatizar continuamente, minimizar o raio de impacto. A Netflix começou com o Chaos Monkey (encerrando instâncias aleatoriamente, 2011) e depois criou o ChAP para experimentos direcionados. Ferramentas: AWS Fault Injection Service, Gremlin, LitmusChaos, Chaos Mesh. GameDays são a forma manual e agendada.

## Por que importa e trade-offs

Encontra modos de falha que testes e revisões não pegam, especialmente em sistemas distribuídos com comportamento emergente, e treina times para responder. Exige boa observabilidade, uma cultura que aceita risco controlado e a capacidade de abortar experimentos; sem isso, é só causar incidentes.

## Exemplo

Hipótese: "se o serviço de recomendações adicionar 500 ms de latência para 1% dos usuários, o sucesso do checkout fica acima de 99,5%". Injete, observe a métrica e pare automaticamente se ela cair.

## Fontes

1. Principles of Chaos Engineering (2019). <https://principlesofchaos.org/> — Definição e os princípios de estado estável, hipótese, eventos reais, produção e raio de impacto.
2. Basiri, A. et al. (2016). *Chaos Engineering*. IEEE Software 33(3), arXiv:1702.05843. <https://arxiv.org/abs/1702.05843> — Descrição revisada por pares da disciplina e dos experimentos da Netflix.
3. AWS. *What is AWS Fault Injection Service?*. <https://docs.aws.amazon.com/fis/latest/userguide/what-is.html> — Ferramenta gerenciada independente com condições de parada e controle de raio de impacto.

## Relações
- (nenhuma ainda)

## Notas
- [Resilience testing your agents on Amazon Bedrock AgentCore](/pt-br/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md)
