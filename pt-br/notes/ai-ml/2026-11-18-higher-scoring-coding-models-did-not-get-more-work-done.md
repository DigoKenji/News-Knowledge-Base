---
type: Paper Summary
title: Modelos de código com maior pontuação não entregaram mais trabalho
description: Uma troca forçada de 19 dias para longe do Fable 5 e cinco atualizações posteriores deixaram as taxas de merge estáveis, apesar dos ganhos em benchmarks.
resource: https://arxiv.org/abs/2611.23216
tags:
- ai-ml
- mlops
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.23216
  title: Higher-Scoring Coding Models Did Not Get More Work Done
  author: Yining Hua
status: stable
source_url: https://arxiv.org/abs/2611.23216
source: arXiv
source_type: arxiv
published: '2026-11-17'
domain: ai-ml
subdomain: mlops
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d810d8db5f294db6841c7
lang: pt-br
translation_of: /notes/ai-ml/2026-11-18-higher-scoring-coding-models-did-not-get-more-work-done.md
---

## 🎯 Em uma frase
Ficar sem o Claude Fable 5 por 19 dias, e depois cinco atualizações comuns de modelo, deixou a taxa de merge em 7 dias dos mesmos desenvolvedores estável: o acesso ao Fable 5 a alterou em +1,4 ponto percentual (IC de 95% de −1,4 a +4,3), embora o Fable 5 estivesse 10,8 pontos à frente do Opus 4.8 no SWE-Bench Pro.

## ❓ O problema
Novos modelos de programação são vendidos com base em ganhos em benchmarks. O SWE-Bench Pro é um benchmark cujas tarefas vêm de correções reais de bugs em repositórios públicos, e uma tarefa conta como resolvida quando a mudança de código passa nos testes da correção original. O artigo pergunta se uma pontuação maior significa mais trabalho concluído. O trabalho real é diferente: os pedidos são vagos, o código precisa seguir o estilo do projeto e alguém precisa aceitar a mudança. No GitHub, essa aceitação é um merge de um pull request (PR), que é uma mudança de código proposta. Comparar desenvolvedores que usam modelos diferentes é difícil, porque os adotantes iniciais podem ser mais experientes, podem dar suas tarefas mais difíceis ao modelo mais forte e cada repositório tem seus próprios hábitos de merge.

## 💡 A ideia
Em vez de comparar modelos entre pessoas, os autores usam um evento externo. Em 12 de junho de 2026, uma diretiva de controle de exportação dos EUA removeu o acesso ao Fable 5 por 19 dias, de modo que seus usuários foram forçados a modelos de nível inferior, e alguns voltaram quando o acesso retornou. As pessoas e os projetos permaneceram os mesmos e apenas o modelo mudou, o que os autores chamam de experimento natural. Eles comparam as taxas de merge dos mesmos desenvolvedores entre as fases com um grupo de controle que usou apenas o Opus 4.8, e depois verificam o resultado contra cinco atualizações de modelo em que os desenvolvedores escolheram trocar.

## ⚙️ Como funciona
### Dados e desfecho
Os dados são PRs públicos do GitHub que o Claude Code ajudou a criar. O Claude Code adiciona aos commits uma linha de coautor que nomeia o modelo, e os autores leem o modelo na descrição do PR. Um PR só é mantido se sua descrição nomear exatamente um modelo. A taxa de merge em 7 dias é a parcela de PRs mesclados até 7 dias após a criação. Em uma verificação de 200 PRs, o modelo nomeado na descrição coincidiu com os commits em 96% a 99% dos casos.

### Experimento natural (análise de suspensão)
O grupo tratado é formado por 297 desenvolvedores que abriram pelo menos um PR nomeando o Fable 5 durante os três dias em que ele esteve disponível. O grupo de controle são 8.908 desenvolvedores que usaram o Opus 4.8 e nunca nomearam o Fable 5. Cada PR é uma observação em um modelo de probabilidade linear:

```text
Y_it = α_i + γ_t + β0·P0_it + β1·P1_it + β3·P3_it + ε_it
```

Y_it é 1 se o PR p do desenvolvedor i, aberto no dia t, foi mesclado em até 7 dias, e 0 caso contrário. α_i é o efeito fixo do desenvolvedor, que absorve diferenças estáveis entre desenvolvedores. γ_t é o efeito fixo do dia do calendário, que absorve mudanças que atingem todos no mesmo dia. P0, P1 e P3 marcam os PRs do grupo tratado nas fases antes do lançamento, disponível e restaurada, tendo a suspensão P2 como referência. A estimativa principal é (β1 + β3)/2, a diferença média entre as duas fases com o Fable 5 disponível e a suspensão; β0 é um placebo.

### Análise de atualizações
As cinco atualizações agrupadas são Opus 4.6 para 4.7, Opus 4.7 para 4.8, Opus 4.8 para 5, Opus 5 para 5.5 e Sonnet 5 para 5.5. Os desenvolvedores que trocaram tiveram PRs com o modelo antigo na semana anterior e com o novo modelo na semana seguinte. Para cada atualização, Y_ip = α_i + δ·New_ip + ε_ip, em que New_ip é 1 se o PR p foi aberto com o modelo novo, e δ é a mudança na taxa de merge. As cinco estimativas são agrupadas por ponderação pelo inverso da variância. Os desenvolvedores escolheram seus modelos por conta própria, então esta parte é uma associação, e não um efeito randomizado.

```text
Y_ip = α_i + δ · New_ip + ε_ip
```

### Contexto de benchmarks
As pontuações relatadas pela Anthropic no SWE-Bench Pro são 80,0% para o Fable 5 e 69,2% para o Opus 4.8, uma diferença de 10,8 pontos. Para seis das atualizações, as duas pontuações foram relatadas, e o modelo mais novo marcou de 5 a 18 pontos a mais. O artigo usa esses ganhos como o lado de benchmark da comparação.

## 🧪 Evidências
| Cenário | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Fable 5 disponível vs. suspenso, 297 desenvolvedores tratados | Período de suspensão no Opus 4.8 | +1,4 ponto percentual (IC de 95% de −1,4 a +4,3) | Taxa de merge em 7 dias |
| Cinco atualizações de modelo agrupadas, 1.208 desenvolvedores que trocaram | Mesmos desenvolvedores no modelo antigo | −0,9 ponto percentual (IC de 95% de −1,9 a +0,2) | Taxa de merge em 7 dias |
| Seis atualizações, contra desenvolvedores que não trocaram | Desenvolvedores que não trocaram | +2,4% (IC de 95% de −4,9% a +10,2%) | PRs mesclados por semana |
| Cinco atualizações agrupadas, PRs que rodaram verificações | Modelo antigo | +0,3 ponto percentual (IC de 95% de −1,4 a +2,0) | Verificação automatizada que falhou |
| Opus 4.7 para 4.8, atualização única | Opus 4.7 | +5,4 (IC de 95% de +1,1 a +9,7) | Verificação automatizada que falhou |
| Diferença no benchmark | Opus 4.8: 69,2% | Fable 5: 80,0%, vantagem de 10,8 pontos | Pontuação no SWE-Bench Pro |

A estimativa da suspensão tem intervalos amplos porque a janela durou apenas 19 dias. O artigo descarta ganhos acima de cerca de 4 pontos com o acesso ao Fable 5. As cinco atualizações dão intervalos mais estreitos e a mesma direção: todas as cinco estimativas pontuais foram negativas e cada intervalo incluiu o 0. Quase todos os PRs já estavam mesclados antes das atualizações (94,5% em até 7 dias na semana anterior), então o desfecho tinha pouco espaço para subir.

## ⚠️ Limitações
- A suspensão durou apenas 19 dias, então sua estimativa é ruidosa, e a análise de atualizações é observacional, porque os desenvolvedores escolheram suas próprias trocas.
- A estimativa principal usa apenas PRs cuja descrição nomeia um modelo, cerca de 1% dos PRs mesclados do Claude Code, e essa parcela caiu durante a suspensão. Um novo ajuste com todos os PRs deu a mesma direção (+0,5 ponto, −2,3 a +3,3).
- Um merge mostra que o desenvolvedor aceitou a mudança, não que ela está correta: 95% dos PRs amostrados foram mesclados pelo próprio autor, e 90% não tinham aprovação de outra conta.
- Foram estudados apenas o Claude Code e apenas PRs públicos do GitHub, e só o Fable 5 teve uma troca imposta de fora.
- A fase de restauração coincide com o lançamento do Sonnet 5 e com uma regra de uso que limitou o uso do Fable 5 até 7 de julho, então alguns desenvolvedores podem ter mantido o Fable 5 para certas tarefas.
- 🔎 Contexto adicional: os 297 desenvolvedores tratados eram adotantes iniciais que começaram a usar o Fable 5 nos seus três primeiros dias. A estimativa do artigo, com peso igual por desenvolvedor, foi de +3,7 (−5,5 a +12,9), então o resultado principal descreve mais quem abre muitos PRs do que um desenvolvedor típico.
- 🔎 Contexto adicional: a comparação é entre dois modelos de fronteira adjacentes, o Fable 5 e o Opus 4.8. O artigo diz pouco sobre ganhos em relação a modelos muito mais antigos.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. Se a sua equipe executa agentes de programação com IA em um pipeline de entrega, o artigo sugere que a pontuação de benchmark de um modelo é um sinal fraco de que a saída dele será aceita. Uma versão prática marcaria cada mudança produzida por agente com o modelo que a fez, como faz a linha de coautor do Claude Code, e depois compararia aceitação e retrabalho nos seus próprios repositórios antes e depois de uma troca de modelo, implantando por equipe para que uma equipe ainda não migrada sirva como grupo de controle natural. O custo são telemetria e várias semanas de dados por equipe. Você só confiaria no resultado se a composição das equipes e os hábitos de revisão se mantivessem estáveis durante a janela, e se também medisse a revisão e os defeitos pós-merge, o que este artigo não consegue medir.

## 🔑 Principais conclusões
- Uma troca forçada de 19 dias do Fable 5 para o Opus 4.8 alterou a taxa de merge em 7 dias dos mesmos desenvolvedores em +1,4 ponto percentual, o que não é estatisticamente significativo.
- Cinco atualizações de modelo agrupadas deram −0,9 ponto percentual (IC de 95% de −1,9 a +0,2), também sem significância.
- Quase todos os PRs já estavam mesclados antes das atualizações, então a taxa de merge tinha pouco espaço para se mover. Os PRs mesclados por semana mudaram +2,4%, sem significância.
- Uma vantagem de 10,8 pontos no SWE-Bench Pro não apareceu como mais trabalho mesclado nesta amostra.
- Merges medem aceitação, não qualidade. Os reverts não mudaram, e as verificações que falharam subiram apenas na troca do Opus 4.7 para o 4.8.
- Teste a afirmação de benchmark de um modelo contra os resultados do seu próprio trabalho antes de trocar de ferramentas.

## 📚 Plano de leitura
### A partir do artigo
- [Anthropic, Statement on the US government directive to suspend access to Fable 5 and Mythos 5](https://www.anthropic.com/news/fable-mythos-access) — o evento que cria o experimento natural.
- [Peralta et al., Why are agentic pull requests merged or rejected?](https://arxiv.org/abs/2605.22534) — o que faz PRs de agentes serem mesclados.
- [Pinna et al., Comparing AI coding agents: a task-stratified analysis of pull request acceptance](https://arxiv.org/abs/2602.08915) — taxas de merge entre agentes e tipos de tarefa.
- [METR, Many SWE-bench-passing PRs would not be merged into main](https://metr.org/notes/2026-03-10-many-swe-bench-passing-prs-would-not-be-merged-into-main/) — por que as aprovações em benchmarks superestimam o trabalho mesclável.
- [Robbes et al., Promises, perils, and (timely) heuristics for mining coding agent activity](https://arxiv.org/abs/2601.18345) — ressalvas para minerar PRs de agentes.
- [benchmarks-vs-merges](https://github.com/ningkko/benchmarks-vs-merges) — o conjunto de dados publicado e o código de reprodução.

### Para ir além
- Designing benchmarks for knowledge work (Hua, Na, Ayubcha, Lian, 2026) — por que as tarefas de benchmark diferem do trabalho real.
- The effects of generative AI on high-skilled work: evidence from three field experiments with software developers (Cui et al., Management Science, 2026) — comparações randomizadas do acesso à IA.
- Difference-in-differences with fixed effects and clustered standard errors — a estatística por trás da estimativa principal.
- Terminal-Bench (Merrill et al., ICLR 2026) — o benchmark de tarefas difíceis com o qual o artigo se compara.
