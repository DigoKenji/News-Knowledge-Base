---
type: Paper Summary
title: 'Inteligência artificial na empresa: gargalos na produção de software'
description: Agentes de programação com IA aumentaram a produção de código, mas a entrega de software cresceu apenas 13,7%, com o code review como gargalo.
resource: https://fion.ac/jellyfish.pdf
tags:
- software-engineering
- engineering-leadership
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://fion.ac/jellyfish.pdf
  title: 'Artificial Intelligence in the Firm: Bottlenecks in Software Production'
  author: Fiona Chen, James Stratton
status: stable
source_url: https://fion.ac/jellyfish.pdf
source: Harvard job market paper (forwarded)
source_type: arxiv
published: '2026-11-14'
domain: software-engineering
subdomain: engineering-leadership
concepts:
- llm-as-a-judge
selected_by: forwarded
content: handoff
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81548bd3f03f417d3791
lang: pt-br
translation_of: /notes/software-engineering/2026-11-17-artificial-intelligence-in-the-firm-bottlenecks-in-software.md
---

# Inteligência artificial na empresa: gargalos na produção de software

**Fonte:** https://fion.ac/jellyfish.pdf
**Autores:** Fiona Chen, James Stratton
**Data:** 2026-11-14
**Conceitos:** [LLM-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md)

---

## 🎯 Em uma frase
Depois que as empresas adotaram agentes de programação com IA, os commits subiram 36,0% e os pull requests abertos subiram 35,7%, mas as issues resolvidas no Jira subiram apenas 13,7% (estatisticamente marginal, sem significância), porque o code review virou o gargalo: o tempo entre a submissão para revisão e a aprovação subiu 56,1%.

## ❓ O problema
As equipes de software hoje usam assistentes de programação com IA, que sugerem código enquanto o desenvolvedor digita, e agentes de programação com IA, que executam tarefas de código mais longas e com várias etapas. Essas ferramentas claramente aumentam a quantidade de código escrito, mas entregar uma funcionalidade exige mais etapas do que escrever: planejamento, revisão humana, testes e implantação. Uma etapa que não consegue acelerar, ou que fica mais difícil quando chega a produção da IA, limita a produção de toda a linha, e o artigo chama essa etapa de "gargalo". Encontrar onde está o gargalo é difícil, porque exige dados muito granulares sobre o processo interno de uma empresa, que a maioria dos conjuntos de dados em nível de empresa não tem. Neste artigo, um pull request (PR) é um conjunto de alterações de código enviado e aguardando revisão, "merged" significa aprovado e incorporado à base de código compartilhada, e uma issue do Jira só conta como resolvida quando seu código é implantado em produção.

## 💡 A ideia
A produção de software é limitada pela mais lenta de duas etapas complementares: escrever código e revisá-lo. Os autores modelam essas etapas como usadas em proporção fixa, o que os economistas chamam de produção de Leontief, de modo que um ganho de velocidade na codificação não acrescenta nada à produção entregue quando a revisão está saturada. Codificar mais rápido forma uma fila de código não revisado, e a espera cresce com a fila, enquanto a própria revisão leva o mesmo tempo por alteração. A revisão também não pode ser dimensionada livremente, porque os revisores trabalham principalmente em código que já conhecem, e a aprovação de um sênior costuma ser exigida em alterações complexas ou escritas por IA. A evidência central do artigo é a fila que aparece depois da adoção de agentes, enquanto o ganho de produção continua pequeno.

## ⚙️ Como funciona

### Dados e momento do tratamento
Os dados vêm do Jellyfish, uma plataforma de analytics que as empresas usam para monitorar equipes de engenharia. Ela registra 445 milhões de eventos de trabalho do GitHub (commits e PRs), do Jira (issues e epics), do Google Calendar (para um subconjunto de 192 empresas) e do uso de ferramentas de IA, cobrindo 720.542 trabalhadores em 655 empresas. A adoção de assistentes é a primeira ativação de licença nas APIs do GitHub Copilot ou do Cursor. A adoção de agentes é o primeiro sinal da API do Claude Code ou de contas de bot do GitHub e de assinaturas de commits ou PRs. O momento da adoção varia entre as empresas porque as etapas de compra (avaliação de fornecedores, orçamento, jurídico, segurança, pilotos) levam tempos diferentes, e os autores argumentam que essa variação provavelmente não está relacionada ao crescimento da empresa.

### Diferenças em diferenças escalonadas
O desenho compara empresas que já adotaram com empresas que ainda não adotaram, com efeitos fixos de empresa e de mês e controles para o tamanho inicial e para os meses desde a integração:

```text
Activity_{f,m} = γ_f + δ_m + Σ_{k≠−1} β_k · AI^k_{f,m} + θ_m · X_{f,m} + ε_{f,m}
```

Aqui, f é uma empresa e m é um mês. Activity é um resultado por trabalhador, como linhas de código, commits, PRs ou issues do Jira. γ_f é um efeito fixo de empresa que absorve características estáveis da empresa, e δ_m é um efeito fixo de mês que absorve choques comuns a todas as empresas. AI^k_{f,m} vale 1 quando o mês m está k meses depois do mês de adoção da empresa, então β_k é o efeito k meses após a adoção, e o artigo relata k = 12. Os erros padrão são agrupados por empresa.

### Modelo estrutural da produção
O modelo tem duas etapas, codificação e revisão, com a produção definida pela mais escassa delas:

```text
Y = (1/n) · min{C, R}     output, with n approved changes per unit of software
C^P = A_C · L_C           code produced: coding productivity times coding labor
R^P = A_R · L_R           reviews completed
A_R = (1 − π) · Ã_R       effective review productivity, net of the bug rate π
W = B / R^P               wait for review: backlog B divided by review capacity
```

A_C é a produtividade de codificação, L_C e L_R são as participações de trabalho na codificação e na revisão, e π é a probabilidade de a revisão encontrar um bug e devolver o código. Uma restrição de só-sênior limita o trabalho de revisão a um teto, de modo que uma parcela das revisões só pode ser feita por engenheiros seniores. Como codificação e revisão são complementares, a alocação ótima equilibra as duas capacidades, e qualquer ganho de codificação além da capacidade de revisão forma um backlog em vez de produção.

### Estimação e simulação de longo prazo
Os autores estimam os cinco parâmetros θ = (dlnA_C, dlnA_R, dlnL_C, dlnL_R, dln n) pelo método clássico de distância mínima, escolhendo θ̂ para reproduzir da forma mais próxima possível seis momentos de estudo de evento, com uma matriz de pesos construída a partir do inverso dos erros padrão ao quadrado. Seis momentos e quatro parâmetros livres deixam duas restrições de sobreidentificação, testadas com uma estatística J de 2,91 (p = 0,233). Para a simulação, a demanda é linear, P = a − bY, e o artigo define a elasticidade da demanda ε no ótimo pré-adoção. O trabalho total da empresa só aumenta se ε superar 3,68. A produtividade de codificação é projetada como uma tendência exponencial ajustada pelas duas estimativas (1,35 para assistentes, 2,34 acumulado com agentes), seguindo a taxa de duplicação dos horizontes de tarefa dos modelos de fronteira de Kwa et al. (2025).

## 🧪 Evidências
| Cenário | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Assistentes de IA, 12 meses após a adoção | 3.445,3 | +1.045,9 (+30,4%), estatisticamente marginal | Linhas de código adicionadas por trabalhador-mês |
| Agentes de IA, 12 meses após a adoção | 4.806,1 | +2.113,6 (+44,0%), estatisticamente marginal | Linhas de código adicionadas por trabalhador-mês |
| Assistentes de IA | 18,55 | +3,11 (+16,8%), significativo | Commits por trabalhador-mês |
| Agentes de IA | 26,43 | +9,51 (+36,0%), significativo | Commits por trabalhador-mês |
| Assistentes de IA | 3,52 | +0,734 (+20,9%), significativo | Pull requests abertos por trabalhador-mês |
| Agentes de IA | 4,83 | +1,73 (+35,7%), significativo | Pull requests abertos por trabalhador-mês |
| Assistentes de IA | 1,80 | −0,027 (−1,5%), não significativo | Issues do Jira resolvidas por trabalhador-mês |
| Agentes de IA | não informado | +13,7%, estatisticamente marginal | Issues do Jira resolvidas |
| Assistentes de IA | 0,095 | +0,003 (+3,4%), não significativo | Epics do Jira resolvidos por trabalhador-mês |
| Agentes de IA | não informado | +18,0%, estatisticamente marginal | Epics do Jira resolvidos |
| Assistentes de IA | não informado | +3,0%, descarta mudanças abaixo de −4,7% | Engenheiros ativos (Jellyfish) |
| Agentes de IA | não informado | −2,6%, descarta mudanças abaixo de −15,6% | Engenheiros ativos (Jellyfish) |
| Agentes de IA | 3,56 dias | +1,99 dias (+56,1%) | Dias entre a submissão do PR e o merge |
| Agentes de IA | 2,78 dias | +1,67 dias (+59,9%) | Dias em backlog até o primeiro comentário |
| Agentes de IA | 3,98 dias | +2,33 dias (+58,5%) | Dias em revisão ativa |
| Agentes de IA | 35,17% | +4,61 pontos (+13,1%), estatisticamente marginal | Parcela de PRs com pelo menos um comentário |
| Agentes de IA | 1,63 | +0,629 (+38,6%) | Comentários de revisão por PR |
| Agentes de IA, estimativa estrutural | não se aplica | Produtividade de codificação +73,7%; produtividade de revisão −7,0%, não significativo | Modelo de distância mínima |

As estimativas vêm de 655 empresas, com efeitos de agentes sobre linhas de código variando de 38,3% a 77,6% nas variantes de robustez. Os efeitos sobre commits e PRs são significativos, mas os efeitos sobre linhas de código, issues e epics não são, e a introdução do artigo chama o efeito dos agentes sobre issues de "limítrofe em significância", enquanto a Seção 4.5 o chama de estatisticamente marginal. O artigo também informa, no texto, que 10,8% dos PRs recebem um comentário gerado por IA, mas a Figura 8 mostra 13,2% para a mesma medida. Entrevistas com 21 pessoas e uma pesquisa com 100 engenheiros ilustram o mecanismo, mas não o testam.

## ⚠️ Limitações
- Os autores medem os efeitos apenas nos primeiros 12 a 16 meses após a adoção, e o emprego pode responder devagar.
- O desenho identifica efeitos de equilíbrio parcial, então os ganhos de produção nas empresas que adotaram podem vir em parte de roubo de negócios e cair em outro lugar.
- A amostra é formada sobretudo por empresas médias e grandes e consolidadas (a empresa média tinha 1.676,6 trabalhadores no total em janeiro de 2023), então os efeitos podem ser diferentes para startups ou outros trabalhos de conhecimento.
- A adoção é medida a partir dos dados e é um limite inferior, e os efeitos por trabalhador com VI são um limite superior para a produtividade individual.
- O cenário em que a produtividade de revisão cresce junto com a produtividade de codificação é um limite superior, e o modelo deixa de fora a especificação, os testes e a coordenação com clientes, que poderiam ser outros gargalos.
- 🔎 Contexto adicionado: a estimativa de produtividade de revisão é de −7,0%, com erro padrão de 0,051 em pontos logarítmicos, e não é significativa, mas a divisão relatada do repasse (62,4% gargalo, 37,6% fragmentação) depende dela. O modelo também trata um PR que recebe comentário como um proxy para bug, o que é uma premissa, e não uma taxa de defeitos medida.
- 🔎 Contexto adicionado: o teste de ajuste do modelo tem dois graus de liberdade e p = 0,233, o que é um apoio fraco à estrutura, e não uma confirmação. Os caminhos de longo prazo extrapolam uma tendência exponencial a partir de duas estimativas, então os números de 2031 são sensíveis a essa escolha.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. Em um pipeline de entrega baseado em PRs, a fila de revisão é onde a produção dos agentes chega, então é ali que a capacidade importa: regras de atribuição de revisores, limites de tamanho de alterações e qualquer revisor de IA que faça uma triagem prévia das alterações antes de um humano ver. O gasto mediano de US$ 178 por trabalhador por mês com agentes (média de US$ 808) é pequeno diante dos custos de salário, então o custo maior é provavelmente o tempo dos revisores seniores, que eu esperaria que fosse o recurso escasso. Para confiar em uma implantação, você acompanharia, por equipe, o tempo até o merge, o tamanho da fila e os comentários por PR antes e depois da adoção, já que o modelo prevê uma fila crescente quando a capacidade de revisão é fixa. As entrevistas também sugerem que uma regra que exige aprovação sênior para código gerado por IA pode criar uma fila própria.

## 🔑 Principais conclusões
- O volume de código superestima a produção: depois da adoção de agentes, os commits subiram 36,0% e os PRs abertos subiram 35,7%, enquanto as issues do Jira resolvidas subiram 13,7%, estatisticamente marginal.
- A revisão é a restrição que cresceu: o tempo entre a submissão para revisão e a aprovação subiu 56,1%, com a espera em backlog subindo 59,9% e a revisão ativa subindo 58,5%.
- A capacidade de revisão está concentrada: 82,8% das revisões são de código em que o revisor já tinha trabalhado, e engenheiros seniores fazem 75,7% das revisões de código escrito por seniores.
- Ainda não há mudança mensurável no emprego: as estimativas são +3,0% para assistentes e −2,6% para agentes, com intervalos amplos.
- O resultado de longo prazo depende da produtividade de revisão: até 2031, se a revisão ficar fixa, a produção sobe 57,8% e o emprego cai 10,3%; se a revisão acompanhar o ritmo, a produção sobe 341,4% e o emprego cai 76,2%.
- Os resultados são de curto prazo e em nível de empresa, medidos ao longo de 12 a 16 meses em empresas em sua maioria grandes.

## 📚 Plano de leitura
### Do artigo
O artigo não oferece repositório de código nem de dados, e as obras citadas aparecem no texto apenas como autor e ano, então não há links a listar.

### Para aprofundar
- Kwa et al. (2025), sobre a medição do horizonte de tempo das tarefas de modelos de IA de fronteira, que os autores usam para extrapolar a produtividade de codificação.
- Demirer, Musolff, & Yang (2026), o estudo anterior mais próximo: em repositórios de código aberto, o Copilot aumentou muito mais as linhas e os commits do que os PRs e as releases.
- Little (1961), sobre a lei de Little, que o modelo usa para converter backlog em tempo de espera por revisão.
- So & Tang (1995) e Smith & Eppinger (1997), sobre como a produção de menor qualidade e o retrabalho desaceleram uma etapa de produção.
- Borusyak et al. (2024) e Callaway & Sant'Anna (2021), sobre estimadores de diferenças em diferenças escalonadas, que os autores usam como verificações de robustez.
- Brynjolfsson, Chandar, & Chen (2025), sobre a perda de empregos júnior causada pela IA, que os autores contrastam com seu resultado nulo para o emprego.
