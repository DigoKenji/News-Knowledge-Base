---
type: Concept
title: Avaliação de modelos e métricas
description: Medir o quão bem um modelo se sai em dados que não viu, com métricas escolhidas para a tarefa - acurácia, precisão, recall, F1, AUC para classificação; medidas de erro para regressão; benchmarks e juízes humanos ou modelos para IA generativa.
tags:
- ai-ml
- evaluation
- core
domain: ai-ml
subdomain: evaluation
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
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 3 Evaluation Methodology, pp. 113-157 (perplexity p. 121; exact evaluation p. 125; AI as a judge p. 136); ch. 4 Evaluate AI Systems, pp. 159-209'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 6 Model Evaluation and Hyperparameter Tuning, pp. 171-204 (k-fold CV p. 175; metrics p. 193)'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 7 Evaluating LLMs, pp. 261-288 (RAG evaluation and Ragas p. 271)'
lang: pt-br
translation_of: /concepts/ai-ml/model-evaluation.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Avaliação de modelos e métricas

**Definição.** A avaliação estima o desempenho no mundo real testando em dados separados e resumindo os resultados com métricas que refletem o que importa no caso de uso. Precisão é a parcela dos positivos previstos que estão corretos; recall é a parcela dos positivos reais encontrados; F1 equilibra os dois. Regressão usa MAE ou RMSE. A ISO/IEC TS 4213 padroniza como avaliar desempenho de classificação.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `evaluation`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Escolher a métrica é uma decisão de negócio: na detecção de fraude, deixar passar fraude (recall baixo) e bloquear bons clientes (precisão baixa) têm custos diferentes, e a acurácia engana quando as classes são desbalanceadas. Modelos generativos são mais difíceis: não há uma única resposta certa, então os times combinam benchmarks (MMLU, HELM), conjuntos de teste da tarefa, avaliações humanas e LLM-as-a-judge, e vigiam a contaminação de benchmarks.

## Exemplo

Um modelo de fraude marca 100 transações, 80 são de fato fraude (precisão de 80%), e ele pegou 80 das 200 fraudes que aconteceram (recall de 40%), mesmo com acurácia geral de 99,9%.

## Conceitos mais específicos

- [LLM como juiz](/pt-br/concepts/ai-ml/llm-as-a-judge.md)
- [Viés de posição](/pt-br/concepts/ai-ml/position-bias.md)
- [Calibração](/pt-br/concepts/ai-ml/calibration.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 3 Evaluation Methodology, pp. 113-157 (perplexity p. 121; exact evaluation p. 125; AI as a judge p. 136); ch. 4 Evaluate AI Systems, pp. 159-209: Avaliação de saídas generativas abertas e como desenhar um pipeline de avaliação.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 6 Model Evaluation and Hyperparameter Tuning, pp. 171-204 (k-fold CV p. 175; metrics p. 193): Avaliação clássica: validação cruzada, matriz de confusão, precisão/recall, ROC.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 7 Evaluating LLMs, pp. 261-288 (RAG evaluation and Ragas p. 271): Avaliações gerais, de domínio e de tarefa para LLMs, além da avaliação de RAG.

## Fontes

1. scikit-learn. *Metrics and scoring - quantifying the quality of predictions*. <https://scikit-learn.org/stable/modules/model_evaluation.html> — Definições e uso de métricas de classificação e regressão.
2. ISO/IEC (2022). *ISO/IEC TS 4213 - Assessment of machine learning classification performance*. <https://www.iso.org/standard/79799.html> — Avaliação padronizada de desempenho de classificação (pago; citado, não transcrito).
3. Liang, P. et al. (2023). *Holistic Evaluation of Language Models (HELM)*. TMLR, arXiv:2211.09110. <https://arxiv.org/abs/2211.09110> — Avaliação de modelos de linguagem com múltiplas métricas além da acurácia.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 3 Evaluation Methodology, pp. 113-157 (perplexity p. 121; exact evaluation p. 125; AI as a judge p. 136); ch. 4 Evaluate AI Systems, pp. 159-209. Avaliação de saídas generativas abertas e como desenhar um pipeline de avaliação.
5. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 6 Model Evaluation and Hyperparameter Tuning, pp. 171-204 (k-fold CV p. 175; metrics p. 193). Avaliação clássica: validação cruzada, matriz de confusão, precisão/recall, ROC.
6. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 7 Evaluating LLMs, pp. 261-288 (RAG evaluation and Ragas p. 271). Avaliações gerais, de domínio e de tarefa para LLMs, além da avaliação de RAG.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
