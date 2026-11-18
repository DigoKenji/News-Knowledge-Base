---
type: Paper Summary
title: Memória de autoaperfeiçoamento permite autoaperfeiçoamento recursivo contínuo
description: Registrar por que cada edição do harness foi feita, e o que ela previa e alcançou, permite que a otimização contínua supere a simples transferência do harness.
resource: https://arxiv.org/abs/2611.18856
tags:
- ai-ml
- mlops
- llm-as-a-judge
- model-context-protocol
- agent-harness
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.18856
  title: Self-Improvement Memory Enables Continual Recursive Self-Improvement
  author: (see paper)
status: stable
source_url: https://arxiv.org/abs/2611.18856
source: arXiv (forwarded via LinkedIn)
source_type: arxiv
published: 2026-11
domain: ai-ml
subdomain: mlops
concepts:
- llm-as-a-judge
- model-context-protocol
- agent-harness
selected_by: forwarded
content: handoff
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d816e920dc492fef2d65e
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-self-improvement-memory-enables-continual-recursive-self-imp.md
---

## 🎯 Em uma frase
Manter um registro estruturado do motivo de cada edição do harness, do que ela previa e do que alcançou permite que um otimizador contínuo do harness supere a simples continuação do harness em seis configurações entre datasets e quatro configurações Opus-para-Gemini, e acrescenta 2,36 pontos em média em dez execuções de codificação (p pareado = 0,003).

## ❓ O problema
Um agente de LLM depende de duas coisas: o modelo de política, que é o LLM que executa as tarefas, e o harness, que é o código em torno dele e gerencia contexto, ferramentas e fluxo de execução. Mudar o harness pode elevar as pontuações sem tocar nos pesos do modelo. Trabalhos recentes automatizam a melhoria do harness com autoaperfeiçoamento recursivo (RSI), um loop que propõe edições no harness, as executa em um benchmark (um conjunto fixo de tarefas pontuadas) e mantém as que obtêm boas pontuações. Esses loops costumam rodar para um único modelo fixo e um único benchmark. Quando um dos dois muda, um harness evoluído pode deixar de ajudar: um harness evoluído com o Opus obtém 68,41 nas tarefas de workspace do Gemini, abaixo dos 73,89 do harness semente (o ponto de partida não evoluído). O otimizador então precisa decidir quais edições herdadas ainda se aplicam, e o harness sozinho não registra por que cada edição foi feita nem como ela se saiu.

## 💡 A ideia
Guarde a experiência do otimizador junto ao harness, e não dentro do modelo que resolve as tarefas. Os autores chamam isso de Self-Improvement Memory (SIM). A SIM agrupa edições passadas pelo problema de harness que elas abordam. Para cada edição, ela mantém a justificativa, os resultados previstos antes do teste, os resultados observados depois e o contexto de avaliação (domínio, benchmark, modelo e rodada). Quando a otimização continua em um novo benchmark ou modelo, o proponente e o selecionador leem esse histórico antes de propor ou aceitar uma edição. Assim, podem manter uma correção herdada cuja razão ainda vale, revisá-la, substituí-la ou removê-la.

## ⚙️ Como funciona
### Componente 1: o registro SIM
Cada problema recorrente de harness c_i, como a conclusão prematura, é uma entrada m_i que guarda suas tentativas:

```text
m_i = (c_i, [(a_ij, o_ij, p_ij)]_j)
```

Aqui, j indexa as tentativas em ordem. a_ij é a edição e sua justificativa, o_ij são os resultados de tarefas previstos e observados, e p_ij é a proveniência (domínio, benchmark, política e rodada). O resultado previsto, ou seja, quais tarefas a edição deve corrigir, é escrito antes da avaliação e verificado depois. Os arquivos de execução do artigo guardam cada entrada como JSON com campos como predicted, fixed, missed, passed e total.

### Componente 2: o proponente e o selecionador leem o registro inteiro
A cada rodada, o proponente P recebe a SIM completa em seu system prompt. O selecionador S lê o mesmo registro ao triar e escolher entre os candidatos. Ambos leem todas as entradas, sem etapa de recuperação e sem truncamento. O loop de atualização fica assim:

```text
C_t = P(H_t, F_t, D_t; M_t)
H_{t+1} = S(H_t, F_t, C_t, F_t^C; M_t)
```

H_t é o harness na rodada t. F_t é o feedback do conjunto de evolução (as tarefas que o otimizador pode ver). D_t é o histórico da execução, com feedbacks e diffs de código anteriores. C_t é o conjunto de k = 5 harnesses candidatos propostos por rodada. F_t^C é o feedback dos candidatos que passaram na triagem e foram executados. M_t é a memória. Tarefas de held-out nunca alimentam nenhuma dessas etapas.

### Componente 3: transferência contínua e atualização da memória
A otimização de origem começa com memória vazia e produz um harness H_S e uma memória M_S. A otimização de destino começa a partir de H_S e M_S, seja em um novo benchmark com a política fixa (entre datasets), seja com uma nova política no mesmo benchmark (entre modelos). Após cada rodada, apenas o candidato escolhido e_t é registrado, incluindo seu resultado medido mesmo quando a edição regride. A atualização M_{t+1} = U(M_t, e_t) acrescenta o registro à entrada que ele cita ou cria uma nova entrada. Se nada for aceito, a memória permanece igual. Entradas com texto de causa raiz idêntico são mescladas sem perder seus históricos. O artigo compara quatro condições a partir do mesmo ponto de partida: Cold Start (harness semente, sem memória), Harness Transfer (H_S, sem memória), Problem-Only Memory (H_S com apenas as causas raiz) e Full SIM (H_S com o registro completo).

## 🧪 Evidências
| Cenário | Baseline | Este artigo | Métrica |
|---|---|---|---|
| Codificação, semente AHE, TB2.1 para TB4 (entre datasets) | Harness Transfer 20,61 | Full SIM 24,85 | pass@1 após RSI no alvo |
| Codificação, semente Terminus-2, TB2.1 para TB4 (entre datasets) | Harness Transfer 19,39 | Full SIM 21,82 | pass@1 após RSI no alvo |
| Classificação de texto, FewShotAll, suíte de origem para 8 datasets alvo | Harness Transfer 74,12 | Full SIM 77,92 | acurácia após RSI no alvo |
| Workspace, ReAct, Harvey LAB para alvo mesclado (entre datasets) | Harness Transfer 85,07 | Full SIM 87,89 | média de três benchmarks de workspace |
| Engenharia, ReAct, EngDesign para Frontier-Eng (entre datasets) | Harness Transfer 42,61 | Full SIM 46,96 | pontuação de medalhas |
| Workspace, Opus 5.5 para Gemini 3.8 Flash (entre modelos) | Harness Transfer 73,42 | Full SIM 84,75 | média de três benchmarks de workspace |
| Engenharia, Opus 5.5 para Gemini 3.8 Flash (entre modelos) | Harness Transfer 42,61 | Full SIM 50,43 | pontuação de medalhas |
| Codificação pareada, dez execuções independentes de origem com Terminus-2, alvo TB4 | Sem SIM 19,33 | SIM 21,70 | pass@1, pontuação final média |

O Full SIM terminou em primeiro em todas as seis configurações entre datasets e em todos os quatro domínios entre modelos. Sua vantagem sobre o Harness Transfer foi de 2,43 a 4,35 pontos no conjunto entre datasets e de 3,60 a 11,33 pontos no conjunto entre modelos. A Problem-Only Memory teve efeitos mistos, de 1,82 ponto abaixo do Harness Transfer a 2,40 acima dele. No estudo de codificação pareada, o SIM terminou acima em nove dos dez pares, e o único par perdedor favoreceu o sem SIM por 0,61 ponto. As evidências não cobrem todos os domínios com execuções repetidas: o estudo pareado é o único com dez execuções independentes de origem, as demais condições são execuções únicas, e os conjuntos de held-out vão de 23 a 500 itens. Trocar o modelo otimizador pelo Claude Opus 5.5 mudou as pontuações de held-out da origem em apenas 1,06 ponto (semente AHE) e 0,05 ponto (semente Terminus-2).

## ⚠️ Limitações
- Os autores afirmam que as repetições independentes de RSI se concentram em codificação; os demais domínios são avaliados com uma execução por condição.
- Os autores afirmam que estudam configurações com interfaces de harness compatíveis e mantêm fixos os procedimentos de planejamento e busca do otimizador.
- Os autores citam como trabalhos futuros mais execuções repetidas, sequências de transição mais longas e adaptação do planejamento, da alocação de tokens e das interfaces de ferramentas.
- A declaração de ética dos autores alerta que um harness auto-modificável poderia satisfazer o avaliador sem resolver a tarefa pretendida, e que as pontuações de workspace podem refletir vieses do juiz LLM.
- 🔎 Contexto adicional: as margens do estudo pareado são grosseiras. O conjunto de held-out do TB4 tem 165 rollouts, então um rollout vale cerca de 0,61 ponto (100/165). O único par perdedor (-0,61) corresponde aproximadamente a um único rollout.
- 🔎 Contexto adicional: a SIM é passada inteira, sem recuperação, então o prompt cresce a cada tentativa. O artigo não traz comparação de tokens nem de custo entre as condições; diz apenas que o uso de tokens e o tempo de execução podem variar.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. Se você executa otimização automatizada de prompts, ferramentas ou fluxos de agentes, a ideia da SIM corresponde a um registro de decisões indexado por problema, escrito pelo otimizador e lido em cada iteração, com os resultados previstos registrados antes de cada execução de teste. Ele viveria ao lado do harness de avaliação e do código que edita, e cresce a cada mudança aceita, então seu armazenamento é barato, mas seu custo em prompt não é. Para confiar nele, você precisaria de avaliação de held-out que o otimizador nunca veja, verificadores que não possam ser burlados e uma checagem de que as previsões estão de fato sendo registradas e resolvidas, inclusive as que erram.

## 🔑 Principais conclusões
- Registrar por que cada edição foi feita, e o que ela previa e alcançou, dá ao otimizador razões para manter, revisar ou remover mudanças herdadas.
- Guardar só o diagnóstico da causa raiz não basta: a Problem-Only Memory teve efeitos mistos, de 1,82 ponto abaixo do Harness Transfer a 2,40 acima dele.
- Um harness evoluído para um modelo ou benchmark pode começar abaixo da semente: 68,41 contra 73,89 nas tarefas de workspace do Gemini, e 72,92 contra 75,72 na classificação de texto.
- No estudo de codificação pareada, a SIM venceu em nove de dez pares, com vantagem média de 2,36 pontos (IC 95% [1,05; 3,68]; p pareado = 0,003).
- Frente a um RSI novo a partir da semente, a continuação teve resultados mistos: melhorou 3,5 pontos no Gemini 3.5 Flash e 1,6 ponto no Gemini 3.8 Flash, mas terminou 0,8 ponto abaixo no Gemini 4 Argon.
- A evidência mais forte é para codificação. Os demais domínios se apoiam em execuções únicas por condição, então suas margens são descritivas.

## 📚 Plano de leitura
### A partir do artigo
- [Meta-Harness](https://openreview.net/forum?id=tmbOUyFx3R) — o otimizador de harness executável sobre o qual este trabalho se apoia, e a origem do ambiente de classificação de texto.
- [Agentic Harness Engineering (AHE)](https://arxiv.org/abs/2604.25850) — torna as edições de harness observáveis e compara as previsões em nível de edição com os resultados; o parente mais próximo dos registros de previsto versus observado da SIM.
- [RRSI](https://arxiv.org/abs/2609.24972) — origem dos ambientes de workspace e de engenharia e das interfaces de pontuação usadas aqui.
- [Terminal-Bench](https://openreview.net/forum?id=a7Qa4CcHak) — a família de benchmarks de codificação (TB2.1 e TB4) por trás do estudo pareado.

### Para ir além
- "From Harness Optimization to Recursive Self-Improvement: A Survey of Language Model Agents" (Yu et al., 2026) — um mapa dos fluxos de otimização de harness nos quais o artigo se apoia.
- "Causal Improvement Graph for Agentic Harness Optimization" (Zhang et al., 2026) — outra forma de guardar evidências, hipóteses, intervenções e resultados; compare o seu registro com o da SIM.
- "Agentic Context Engineering" (Zhang et al., 2026) — memória de playbook para agentes que resolvem tarefas, o caso de contraste que o artigo discute no Apêndice A.3.
