---
type: Concept
title: Drift de conceito
description: 'Uma mudança ao longo do tempo na relação entre as entradas de um modelo e a saída correta, de modo que um modelo treinado com dados passados erra mesmo que as entradas pareçam iguais.'
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
- title: 'Gama, J. et al. (2014). A Survey on Concept Drift Adaptation. ACM Computing Surveys 46(4)'
  resource: 'https://doi.org/10.1145/2523813'
  kind: paper
- title: 'Lu, J. et al. (2019). Learning under Concept Drift: A Review. IEEE TKDE 31(12)'
  resource: 'https://doi.org/10.1109/TKDE.2018.2876857'
  kind: paper
relations:
- type: alternative-to
  target: /concepts/ai-ml/data-drift.md
lang: pt-br
translation_of: /concepts/ai-ml/concept-drift.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Drift de conceito

**Definição.** Concept drift acontece quando a relação entre entradas e resposta correta muda depois que o modelo foi treinado: P(y|x) se desloca, então a mesma entrada deveria receber outra previsão. Padrões de fraude que se adaptam a um detector, ou preferências de clientes que mudam após um reajuste de preço, são casos típicos. As revisões distinguem drift súbito, gradual, incremental e recorrente, e a detecção costuma depender de acompanhar o erro do modelo quando os rótulos verdadeiros chegam.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `ml-governance`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

É o irmão mais difícil do data drift: as estatísticas das entradas podem parecer iguais enquanto a acurácia cai. As respostas usuais são monitorar o erro em amostras rotuladas, retreinar de forma agendada ou disparada, e usar dados de treino em janelas ou ponderados que favorecem exemplos recentes.

## Exemplo

Um modelo de churn treinado antes de um concorrente lançar um plano mais barato continua recebendo as mesmas características de clientes, mas clientes com essas características agora saem com mais frequência; a precisão cai até ele ser retreinado com resultados recentes.

## Fontes

1. Iusztin, P. and Labonne, M. (2024). LLM Engineer's Handbook. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 11 MLOps and LLMOps: Drifts, p. 469. Tipos de drift e por que o monitoramento precisa detectá-los em produção.
2. Gama, J. et al. (2014). A Survey on Concept Drift Adaptation. ACM Computing Surveys 46(4). <https://doi.org/10.1145/2523813> — Revisão de referência: tipos de drift e estratégias de adaptação.
3. Lu, J. et al. (2019). Learning under Concept Drift: A Review. IEEE TKDE 31(12). <https://doi.org/10.1109/TKDE.2018.2876857> — Arcabouço de detecção, entendimento e adaptação.

## Relações
- **alternativa-a** [Drift de dados](/pt-br/concepts/ai-ml/data-drift.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
