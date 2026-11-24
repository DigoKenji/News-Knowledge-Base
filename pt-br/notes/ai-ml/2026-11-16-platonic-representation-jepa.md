---
type: Post Summary
title: 'Dois JEPAs, um modelo de mundo: a Hipótese da Representação Platônica'
description: 'Um post sobre o resultado de LeCun e Balestriero: JEPAs de imagem e de texto treinados separadamente convergem para a mesma representação, com 42x menos amostras do que a predição do próximo token.'
resource: https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB
tags:
- ai-ml
- deep-learning
- jepa
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB
  title: 'Two JEPAs, one world model: the Platonic Representation Hypothesis'
  author: alphaXiv
status: stable
source_url: https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB
source: LinkedIn (forwarded)
source_type: linkedin
published: '2026-11-01'
domain: ai-ml
subdomain: deep-learning
concepts:
- jepa
selected_by: forwarded
content: handoff
notion_url: https://app.notion.com/p/41abaf74172d81d6b296dc0cc88b2882
lang: pt-br
translation_of: /notes/ai-ml/2026-11-16-platonic-representation-jepa.md
---

# LeCun e Balestriero sobre a Hipótese da Representação Platônica

**Fonte:** https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB (post no LinkedIn)
**Autor:** alphaXiv (post; o post cita Yann LeCun e Randall Balestriero como os pesquisadores)
**Data:** 2026-11-01
**Tags:** #representation-learning #jepa #multimodal #format-linkedin-post

---

## Sobre o que é

A Hipótese da Representação Platônica diz que modelos treinados em modalidades diferentes, como imagens e texto, acabam aprendendo representações semelhantes do mundo. O ponto do post é que a forma como um modelo é treinado importa mais do que simplesmente aumentá-lo. Ele afirma que os pesquisadores mostram matematicamente que, sob certas condições, dois [JEPA](/pt-br/concepts/ai-ml/jepa.md)s treinados completamente de forma separada aprendem essencialmente a mesma representação subjacente. Um aprende a partir de imagens diferentes da mesma cena, o outro a partir de descrições diferentes dessa cena, e nenhum dos dois é treinado com pares imagem-texto.

## Resultados citados no post

- O mesmo alinhamento imagem-texto da predição do próximo token, com 42x menos amostras de treinamento.
- Um JEPA de texto com 109M de parâmetros que supera LLMs de até 72B de parâmetros em benchmarks de alinhamento entre modalidades.

Esses números vêm do post, e não do próprio artigo.

> A ideia-chave: o objetivo de treinamento e o pareamento dos dados importam mais do que a escala. Dois JEPAs treinados separadamente podem convergir para a mesma representação sem nunca ver pares imagem-texto.

## Padrões e técnicas identificados

- **Predição em embedding conjunto:** JEPAs predizem representações de uma visão a partir de outra, em vez de predizer tokens ou pixels brutos.
- **Alinhamento entre modalidades sem dados pareados:** cada modelo vê uma visão diferente do mesmo conteúdo (imagens de uma cena, ou descrições dela), e o alinhamento surge sem pares imagem-texto.

## Principais conclusões

- A tese central do post é que o método de treinamento importa mais do que a escala para aprender representações compartilhadas.
- O alinhamento entre modalidades pode vir de combinar visões diferentes da mesma cena, e não de dados imagem-texto pareados.
- A diferença de 42x na eficiência de amostras em relação à predição do próximo token é o argumento prático para o treinamento no estilo JEPA.

## Leituras de aprofundamento

- 📄 Artigo citado no post — https://lnkd.in/gcenbTDQ → O artigo completo que o post resume. Leia-o para verificar as condições em que o resultado vale.
- 📚 Assran et al., 2023, *Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture* (I-JEPA, CVPR 2023), arXiv:2301.08243 → O JEPA de imagem sobre o qual essa linha de trabalho se apoia.
- 📚 Huh et al., 2024, *The Platonic Representation Hypothesis* (ICML 2024), arXiv:2405.07987 → A hipótese original à qual o resultado deste post está ligado.

---
*Resumo gerado: 2026-11-16*
