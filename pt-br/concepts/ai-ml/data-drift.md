---
type: Concept
title: Drift de dados
description: 'Uma mudança ao longo do tempo na distribuição dos dados de entrada de um modelo em relação aos dados de treino, que pode degradar as previsões e é monitorada em produção.'
tags:
- ai-ml
- ml-governance
domain: ai-ml
subdomain: ml-governance
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 11 MLOps and LLMOps: Drifts, p. 469'
- title: 'Rabanser, S., Günnemann, S. and Lipton, Z. C. (2019). Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift. NeurIPS 2019'
  resource: 'https://arxiv.org/abs/1810.11953'
  kind: paper
- title: 'Quiñonero-Candela, J. et al. (eds.) (2009). Dataset Shift in Machine Learning. MIT Press'
  resource: 'https://books.google.com/books?vid=ISBN9780262170055'
  kind: book
lang: pt-br
translation_of: /concepts/ai-ml/data-drift.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Drift de dados

**Definição.** Data drift (também covariate shift) é uma mudança na distribuição das entradas de um modelo, P(x), em relação aos dados de treino - novos segmentos de usuários, um sensor trocado, outro formato vindo de um sistema anterior. O modelo pode continuar certo para as entradas que conhece, mas passa a ser consultado em regiões que viu pouco, então a qualidade pode cair. O drift é detectado sem rótulos, comparando estatísticas das features entre uma janela de referência e o tráfego ao vivo.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ml-governance`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Como não precisa de rótulos, é o primeiro alarme do monitoramento de ML, geralmente com testes estatísticos ou distâncias (teste KS, population stability index, maximum mean discrepancy) por feature ou sobre embeddings. É um gatilho para investigar, não prova de falha; o concept drift é o que de fato quebra o mapeamento.

## Exemplo

Depois de um redesenho do app, a proporção de sessões sem referenciador salta de 2% para 30%; o job de monitoramento sinaliza a feature antes que qualquer métrica de acurácia esteja disponível.

## Fontes

1. Iusztin, P. and Labonne, M. (2024). LLM Engineer's Handbook. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 11 MLOps and LLMOps: Drifts, p. 469. Data drift versus concept drift e como são monitorados.
2. Rabanser, S., Günnemann, S. and Lipton, Z. C. (2019). Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift. NeurIPS 2019. <https://arxiv.org/abs/1810.11953> — Compara métodos de detecção de mudança de distribuição em dados reais.
3. Quiñonero-Candela, J. et al. (eds.) (2009). Dataset Shift in Machine Learning. MIT Press. <https://books.google.com/books?vid=ISBN9780262170055> — Livro de referência sobre covariate shift e dataset shift.

## Relações
- [Drift de conceito](/pt-br/concepts/ai-ml/concept-drift.md) **alternativa-a** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
