---
type: Concept
title: Avaliação de modelos e métricas
description: Medir o quão bem um modelo se sai em dados que não viu, com métricas escolhidas para a tarefa - acurácia, precisão, recall, F1, AUC para classificação; medidas de erro para regressão; benchmarks e juízes humanos ou modelos para IA generativa.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: scikit-learn. Metrics and scoring - quantifying the quality of predictions
  resource: 'https://scikit-learn.org/stable/modules/model_evaluation.html'
  kind: docs
- title: ISO/IEC (2022). ISO/IEC TS 4213 - Assessment of machine learning classification performance
  resource: 'https://www.iso.org/standard/79799.html'
  kind: standard
- title: 'Liang, P. et al. (2023). Holistic Evaluation of Language Models (HELM). TMLR, arXiv:2211.09110'
  resource: 'https://arxiv.org/abs/2211.09110'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/model-evaluation.md
---

# Avaliação de modelos e métricas

**Definição.** A avaliação estima o desempenho no mundo real testando em dados separados e resumindo os resultados com métricas que refletem o que importa no caso de uso. Precisão é a parcela dos positivos previstos que estão corretos; recall é a parcela dos positivos reais encontrados; F1 equilibra os dois. Regressão usa MAE ou RMSE. A ISO/IEC TS 4213 padroniza como avaliar desempenho de classificação.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Escolher a métrica é uma decisão de negócio: na detecção de fraude, deixar passar fraude (recall baixo) e bloquear bons clientes (precisão baixa) têm custos diferentes, e a acurácia engana quando as classes são desbalanceadas. Modelos generativos são mais difíceis: não há uma única resposta certa, então os times combinam benchmarks (MMLU, HELM), conjuntos de teste da tarefa, avaliações humanas e LLM-as-a-judge, e vigiam a contaminação de benchmarks.

## Exemplo

Um modelo de fraude marca 100 transações, 80 são de fato fraude (precisão de 80%), e ele pegou 80 das 200 fraudes que aconteceram (recall de 40%), mesmo com acurácia geral de 99,9%.

## Conceitos mais específicos

- [LLM como juiz](/pt-br/concepts/ai-ml/llm-as-a-judge.md)
- [Viés de posição](/pt-br/concepts/ai-ml/position-bias.md)
- [Calibração](/pt-br/concepts/ai-ml/calibration.md)

## Fontes

1. scikit-learn. *Metrics and scoring - quantifying the quality of predictions*. <https://scikit-learn.org/stable/modules/model_evaluation.html> — Definições e uso de métricas de classificação e regressão.
2. ISO/IEC (2022). *ISO/IEC TS 4213 - Assessment of machine learning classification performance*. <https://www.iso.org/standard/79799.html> — Avaliação padronizada de desempenho de classificação (pago; citado, não transcrito).
3. Liang, P. et al. (2023). *Holistic Evaluation of Language Models (HELM)*. TMLR, arXiv:2211.09110. <https://arxiv.org/abs/2211.09110> — Avaliação de modelos de linguagem com múltiplas métricas além da acurácia.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
