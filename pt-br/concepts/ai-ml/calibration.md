---
type: Concept
title: Calibração
description: Quão bem a confiança declarada de um modelo corresponde à sua precisão real - de todas as respostas dadas com 80% de confiança, um modelo calibrado acerta cerca de 80% das vezes.
tags:
- ai-ml
- evaluation
domain: ai-ml
subdomain: evaluation
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Guo, C. et al. (2017). On Calibration of Modern Neural Networks. ICML 2017, arXiv:1706.04599'
  resource: 'https://arxiv.org/abs/1706.04599'
  kind: paper
- title: 'Kadavath, S. et al. (2022). Language Models (Mostly) Know What They Know. arXiv:2207.05221'
  resource: 'https://arxiv.org/abs/2207.05221'
  kind: paper
- title: scikit-learn. Probability calibration (user guide 1.16)
  resource: 'https://scikit-learn.org/stable/modules/calibration.html'
  kind: docs
lang: pt-br
translation_of: /concepts/ai-ml/calibration.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Calibração

**Definição.** Um modelo é calibrado quando suas probabilidades previstas podem ser lidas como frequências reais. Calibração é diferente de precisão: um modelo pode ser muito preciso e ainda assim confiante demais, ou fraco mas honesto sobre sua incerteza.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `evaluation`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Mede-se agrupando as previsões em faixas de confiança e comparando a confiança média com a precisão observada em cada faixa, num diagrama de confiabilidade; o Expected Calibration Error (ECE) é a diferença média ponderada. Guo et al. (2017) mostraram que redes profundas modernas são sistematicamente confiantes demais e que o temperature scaling, um parâmetro ajustado em dados separados, corrige boa parte disso. Outros métodos são Platt scaling e regressão isotônica. Em LLMs, a calibração pode ser medida nas probabilidades dos tokens ou na confiança verbalizada ("tenho 70% de certeza"); Kadavath et al. encontraram modelos grandes razoavelmente calibrados em perguntas de múltipla escolha, enquanto o ajuste por RLHF tende a piorá-la.

## Por que importa e trade-offs

Qualquer decisão que usa uma pontuação como limiar (aprovar automaticamente acima de 0,9, escalar para humano abaixo) precisa de pontuações calibradas, senão o limiar não significa nada. A calibração é medida sobre uma distribuição, então pode quebrar com data drift, e um modelo calibrado ainda pode ser inútil se for incerto sobre tudo.

## Exemplo

Um meteorologista que diz "70% de chance de chuva" em 100 dias é calibrado se chover em cerca de 70 deles.

## Fontes

1. Guo, C. et al. (2017). *On Calibration of Modern Neural Networks*. ICML 2017, arXiv:1706.04599. <https://arxiv.org/abs/1706.04599> — Diagramas de confiabilidade, ECE, excesso de confiança de redes profundas, temperature scaling.
2. Kadavath, S. et al. (2022). *Language Models (Mostly) Know What They Know*. arXiv:2207.05221. <https://arxiv.org/abs/2207.05221> — Calibração de grandes modelos de linguagem e autoavaliação das respostas.
3. scikit-learn. *Probability calibration* (user guide 1.16). <https://scikit-learn.org/stable/modules/calibration.html> — Curvas de calibração, calibração de Platt (sigmoide) e isotônica na prática.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
