---
type: Concept
title: Mistura de especialistas
description: Uma camada de rede neural feita de muitas sub-redes especialistas mais um roteador que ativa só alguns especialistas por token, de modo que o total de parâmetros cresce sem um crescimento equivalente do cálculo por token.
tags:
- ai-ml
domain: ai-ml
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Jacobs, R. A. et al. (1991). Adaptive Mixtures of Local Experts. Neural Computation 3(1)
  resource: 'https://doi.org/10.1162/neco.1991.3.1.79'
  kind: paper
- title: 'Shazeer, N. et al. (2017). Outrageously Large Neural Networks - The Sparsely-Gated Mixture-of-Experts Layer. ICLR 2017, arXiv:1701.06538'
  resource: 'https://arxiv.org/abs/1701.06538'
  kind: paper
- title: 'Fedus, W., Zoph, B. and Shazeer, N. (2022). Switch Transformers. JMLR, arXiv:2101.03961'
  resource: 'https://arxiv.org/abs/2101.03961'
  kind: paper
- title: 'Jiang, A. Q. et al. (2024). Mixtral of Experts. arXiv:2401.04088'
  resource: 'https://arxiv.org/abs/2401.04088'
  kind: paper
lang: pt-br
translation_of: /concepts/mixture-of-experts.md
---

# Mistura de especialistas

**Definição.** Uma camada de mistura de especialistas (MoE) substitui um grande bloco feed-forward por N "especialistas" menores e uma rede de roteamento aprendida. Para cada token o roteador escolhe os top-k especialistas (muitas vezes 1 ou 2 de 8 a 256) e combina suas saídas. O modelo tem muitos parâmetros no total mas usa só uma fração deles para cada token, o que se chama ativação esparsa.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md). Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

A ideia vem de Jacobs et al. (1991). Shazeer et al. (2017) a fizeram funcionar em escala no deep learning com uma camada de roteamento esparso e uma perda auxiliar que equilibra a carga entre os especialistas, para que o roteador não mande tudo para alguns favoritos. O Switch Transformer simplificou o roteamento para top-1. O Mixtral 8x7B tem cerca de 47B de parâmetros mas usa cerca de 13B por token. Os especialistas não se especializam em temas humanos como "direito" ou "matemática"; tendem a se especializar em padrões no nível dos tokens. Os especialistas costumam ser espalhados por GPUs (paralelismo de especialistas), o que acrescenta comunicação all-to-all.

## Por que importa e trade-offs

MoE dá mais capacidade de modelo para o mesmo cálculo de treino e inferência, por isso muitos modelos de fronteira e abertos (Mixtral, DeepSeek-V3, Qwen MoE) o usam. O custo é memória: todos os especialistas precisam estar carregados mesmo que poucos sejam usados, além de instabilidade no treino, trabalho de balanceamento de carga e serviço mais difícil.

## Exemplo

O DeepSeek-V3 tem 671B de parâmetros mas ativa cerca de 37B por token, então roda com custo por token próximo ao de um modelo denso de 37B.

## Fontes

1. Jacobs, R. A. et al. (1991). *Adaptive Mixtures of Local Experts*. Neural Computation 3(1). <https://doi.org/10.1162/neco.1991.3.1.79> — Ideia original de mistura de especialistas com rede de roteamento.
2. Shazeer, N. et al. (2017). *Outrageously Large Neural Networks - The Sparsely-Gated Mixture-of-Experts Layer*. ICLR 2017, arXiv:1701.06538. <https://arxiv.org/abs/1701.06538> — Roteamento esparso top-k e balanceamento de carga em escala.
3. Fedus, W., Zoph, B. and Shazeer, N. (2022). *Switch Transformers*. JMLR, arXiv:2101.03961. <https://arxiv.org/abs/2101.03961> — Roteamento top-1 e estabilidade de treino.
4. Jiang, A. Q. et al. (2024). *Mixtral of Experts*. arXiv:2401.04088. <https://arxiv.org/abs/2401.04088> — Parâmetros totais vs ativos num modelo aberto de produção; análise da especialização dos especialistas.

## Relações
- (nenhuma ainda)

## Notas
- [O ciclo de realimentação de modelos de pesos abertos EUA-China](/pt-br/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
