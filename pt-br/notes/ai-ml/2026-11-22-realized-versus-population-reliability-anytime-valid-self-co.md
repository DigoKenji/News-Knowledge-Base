---
type: Paper Summary
title: 'Confiabilidade Realizada versus Populacional: Certificação de Autoconsistência Válida a Qualquer Momento sob Raciocínio Correlacionado'
description: Com rollouts correlacionados, certificados de autoconsistência válidos a qualquer momento certificam
  uma afirmação por prompt, e não a afirmação de confiabilidade populacional.
resource: https://arxiv.org/abs/2611.26592
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26592
  title: 'Realized versus Population Reliability: Anytime-Valid Self-Consistency Certification under Correlated Reasoning'
  author: Terence D. Agbeyegbe, Toneukarin G. Agbeyegbe
status: stable
source_url: https://arxiv.org/abs/2611.26592
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: ai-ml
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d8166ba06e8c67ad1a181
lang: pt-br
translation_of: /notes/ai-ml/2026-11-22-realized-versus-population-reliability-anytime-valid-self-co.md
---

## 🎯 Em uma frase
Certificados de autoconsistência válidos a qualquer momento são exatos para a afirmação por prompt de que o raciocínio sorteado favorece uma dada resposta, mas, quando os rollouts são correlacionados, eles superestimam a afirmação populacional: no AIME, a regra de unidade única certifica 24.0% dos prompts, mesmo com a margem populacional estimada θ̂ = −0.15.

## ❓ O problema
Autoconsistência significa amostrar um modelo de linguagem várias vezes sobre a mesma pergunta e adotar a resposta majoritária; cada execução amostrada completa é um rollout. Trabalhos recentes (Cordero-Encinar and Duncan, 2025) transformam "quantas amostras são suficientes" em uma regra de parada: continue amostrando enquanto um e-process (um produto acumulado de fatores de evidência) acumula evidência, e pare quando ele cruza um limiar. A garantia é controle de erro do Tipo I válido a qualquer momento: a chance de um veredito falso de "certificado" permanece em ou abaixo de ϵ = 0.05 sempre que você parar, pela desigualdade de Ville. Essa garantia assume que cada rollout é uma extração independente. Fluxos de raciocínio reais não são independentes: os rollouts compartilham um prefixo de raciocínio amostrado, e as afirmações agregadas reúnem prompts de dificuldades muito diferentes, de modo que os rollouts são positivamente correlacionados. O artigo pergunta qual afirmação tal certificado ainda tem direito a fazer sob essa correlação.

## 💡 A ideia
A correlação divide a hipótese nula única em duas afirmações que coincidem apenas quando a correlação é zero. A afirmação realizada é sobre o único prompt e prefixo à sua frente: a resposta é favorecida por este raciocínio sorteado? A afirmação populacional é sobre o comportamento médio do modelo ao longo de sua aleatoriedade e da distribuição de tarefas: a resposta é favorecida em média? O processo de apostas padrão continua exatamente válido para a afirmação realizada, mas uma equipe que o lê como afirmação populacional está certificando algo que nunca testou. Os autores enquadram a lacuna como o efeito de desenho (design effect) de amostragem por conglomerados da estatística de pesquisas: muitas extrações de um mesmo conglomerado carregam muito menos informação do que sua contagem sugere.

## ⚙️ Como funciona

### Modelo de evidência dependente
Cada unidade de certificação (um prompt, ou um prefixo compartilhado) sorteia um estado latente Z uma vez. Dado Z, os rollouts são condicionalmente i.i.d. com média condicional µ(Z). O escore de cada rollout held-out é Y_t = 2·1{X_t = c} − 1, em que c é a resposta escolhida em uma amostra piloto separada e X_t é a resposta held-out. Marginalmente, os escores são intercambiáveis com correlação par ρ, a parcela da variância que vem da unidade.

```text
Y_t    = 2*1{X_t = c} - 1                   values in {-1, +1}
mu(Z)  = E[Y_t | Z]                          realized margin of this unit
theta  = E[mu(Z)]                            population drift
pi     = P(mu(Z) > 0)                        share of units with positive realized margin
rho    = sigma_B^2 / (sigma_B^2 + sigma_W^2)
```

Aqui sigma_B^2 é a variância de µ(Z) entre unidades (variância entre unidades) e sigma_W^2 é a variância intraunidade média. Quando ρ = 0, o estado latente não carrega informação e o modelo se reduz ao caso independente.

### Qual hipótese nula o certificado padrão controla
O certificado padrão multiplica fatores por rollout, M_n = produto sobre t ≤ n de (1 + λ·Y_t), com a fração de aposta λ entre 0 e 1/B (aqui B = 1), e mistura λ sobre uma grade. Sob correlação, ele ainda é um teste exato da hipótese nula realizada µ(Z) ≤ 0, porque, condicionado em Z, os rollouts são i.i.d. com média não positiva (Teorema 4.2). Não é um teste válido da hipótese nula populacional θ ≤ 0 sobre o histórico observado, pois os dados revelam gradualmente a crença realizada. No limite, sua probabilidade de cruzamento sob a hipótese nula populacional é de pelo menos π (Proposição 4.5), o que pode ficar bem acima do ϵ nominal.

```text
M_n = prod over t <= n of (1 + lambda * Y_t),   0 <= lambda <= 1/B
P( sup_n M_n >= 1/eps )  <=  eps     under the realized null mu(Z) <= 0
```

Aqui lambda é a fração de aposta, eps é o nível de erro nominal (0.05 nos experimentos), e M_n é o produto de evidência acumulado após n rollouts.

### Saturação de informação
Extrair mais rollouts de um prompt não resolve o problema. Dentro de uma unidade, a média amostral converge para a crença realizada µ(Z), não para θ, e sua variância para de cair em sigma_B^2. O tamanho efetivo de amostra da média intraunidade é n_eff = n / (1 + (n − 1)·ρ), que tende a 1/ρ. Com as correlações medidas próximas de 0.45, um prompt vale cerca de dois rollouts independentes para a questão populacional, não importa quantos rollouts você extraia (Teorema 5.1). Os autores destacam que este é um cálculo de efeito de desenho para esse estimador, não um limite universal de informação para todo estimador.

```text
Var(Ybar_n) = sigma_B^2 + sigma_W^2 / n = sigma^2 * (1 + (n - 1) * rho) / n
n_eff       = n / (1 + (n - 1) * rho)     tends to 1/rho as n grows
```

Ybar_n é a média dos n rollouts intraunidade, e sigma^2 = sigma_B^2 + sigma_W^2 é a variância marginal de um único escore.

### Remédio no nível do prompt e alocação
O remédio move a unidade de acumulação de rollouts para prompts. Para cada prompt q, a média intraunidade U_q é um resumo quase independente, e o processo é M_Q = produto sobre q ≤ Q de (1 + λ·U_q) através de Q prompts. Como os U_q são independentes com média θ, isso é um supermartingale não negativo sob θ ≤ 0, então a desigualdade de Ville dá controle populacional exato válido a qualquer momento (Teorema 6.2). Para um orçamento total fixo N = Q·n, a variância do estimador agrupado cresce com n, então, sob um orçamento puro de rollouts, a melhor escolha é um rollout por prompt (Proposição 6.3). Rollouts extras por prompt compensam apenas para amortizar um custo real por prompt, e então apenas até um ponto que diminui conforme a correlação cresce.

```text
U_q    = within-unit mean of the n_q rollouts for prompt q
M_Q    = prod over q <= Q of (1 + lambda * U_q)
n_star = sqrt( (c_p / c_r) * (1 - rho) / rho )    two-cost model, per the paper's display
```

Aqui c_p é o custo fixo por prompt (prefill ou codificação), c_r é o custo por rollout, e n_star é o número contínuo ótimo de rollouts por prompt.

### ρ desconhecido e dependência serial
A validade sob a hipótese realizada não exige conhecimento de ρ, e a validade populacional vale para qualquer λ fixo, qualquer que seja ρ. O que ρ afeta é a eficiência, porque a melhor fração de aposta depende da dispersão das médias das unidades. Os autores recomendam apostas adaptativas à variância no nível da unidade, como apostas empírico-Bernstein (Waudby-Smith and Ramdas, 2024) ou estimadores do tipo Catoni para resumos de cauda pesada (Wang and Ramdas, 2023). O argumento de condicionamento exige um estado latente por unidade com independência condicional, então dependência serial arbitrária dentro de uma unidade não é coberta. Os autores a chamam de principal direção em aberto e apontam para construções de mistura ou por blocos.

## 🧪 Evidências
| Cenário | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| Sintético: recuperação de ρ (ρ planejado = 0.09) | Planejado 0.091 | Recuperado 0.091 | ICC por ANOVA de um fator |
| Sintético: hipótese nula realizada (θ ≤ 0, π = 0.37) | ϵ nominal = 0.05 | Certificado de unidade única 0.005 | Erro do Tipo I na hipótese realizada |
| Sintético: hipótese nula populacional, certificado de unidade única | ϵ nominal = 0.05 | 0.32, subindo em direção a π = 0.37 | Taxa de certificação na hipótese populacional |
| Sintético: hipótese nula populacional, remédio no nível do prompt | ϵ nominal = 0.05 | 0.033 com θ = 0 | Taxa de certificação na hipótese populacional |
| Sintético: saturação | Nenhuma | Limite de variância 0.090 = sigma_B^2; n_eff ≈ 11 ≈ 1/ρ | Limite de variância e tamanho efetivo de amostra |
| GSM8K, 300 prompts × 30 rollouts | Rollouts independentes (ρ = 0) | ρ-hat 0.435 [0.208, 0.619]; θ-hat +0.96; π-hat 0.99; 1/ρ-hat 2.3 | ICC com IC 95% por bootstrap de conglomerados |
| AIME, 150 prompts × 30 rollouts | Rollouts independentes (ρ = 0) | ρ-hat 0.480 [0.405, 0.544]; θ-hat -0.15; π-hat 0.33; 1/ρ-hat 2.1 | ICC com IC 95% por bootstrap de conglomerados |
| AIME: certificado de unidade única | ϵ nominal = 0.05 | 0.240 por prompt (24.0%) | Taxa de certificação na hipótese populacional |
| AIME: remédio no nível do prompt | ϵ nominal = 0.05 | 0.000 | Taxa de certificação por bootstrap |
| AIME por faixa de concordância do piloto | ϵ nominal = 0.05 | Taxa de unidade única 0.00, 0.03, 0.06, 0.73, 1.00 nas faixas; θ-hat de -0.70 a +0.97 | Taxa de certificação por faixa |

O estudo sintético confirma o mecanismo: o certificado de unidade única mantém o erro da hipótese realizada muito abaixo de ϵ, mas certifica a hipótese populacional em 0.32, enquanto o remédio no nível do prompt se mantém em 0.033 quando θ = 0. Em rollouts reais do Claude Haiku 4.5 com temperatura 1.0, a correlação está presente nos dois benchmarks, mas apenas o AIME tem deriva populacional negativa. Ali, os certificados de unidade única vêm inteiramente de prompts de alta concordância: nenhum dos 101 prompts com médias held-out não positivas disparou, enquanto 49 prompts com médias positivas dispararam, com taxa de 0.74. O artigo não relata ablação sobre a grade de apostas nem sobre o tamanho do piloto, e as evidências se apoiam em um modelo, 150 a 300 prompts e uma semente; os autores afirmam que o 0.000 do remédio no nível do prompt é consistente com validade, mas não a estabelece. O texto arredonda o ρ-hat do GSM8K para 0.44, enquanto a tabela reporta 0.435.

## ⚠️ Limitações
- A correlação intraprompt proveniente de um prefixo de raciocínio compartilhado não é medida; apenas o ρ entre prompts é.
- Foi executado apenas um modelo, Claude Haiku 4.5, com temperatura 1.0; os autores consideram uma replicação com modelo aberto a verificação natural e observam que ela poderia alterar as magnitudes e possivelmente o padrão.
- Dependência serial arbitrária dentro de uma unidade está fora da teoria; o modelo assume uma estrutura de crença latente intercambiável.
- O trabalho é um resultado de correção, não de eficiência, e o controle do Tipo I não é poder estatístico: perto de θ ≈ 0, certificar a hipótese populacional pode exigir muitas unidades.
- O π-hat é um proxy descritivo baseado no sinal das médias held-out, e a taxa de cruzamento por bootstrap de prompts não é uma estimativa da taxa de erro do Tipo I.
- O código-fonte e os rollouts salvos são suplementares e serão depositados em um repositório público apenas com a publicação, portanto ainda não existe link público de artefato.
- 🔎 Contexto adicionado: o número de destaque de 24.0% vem de uma semente, um modelo e uma regex de extração de respostas; o artigo não relata quão sensível ele é a essas escolhas.
- 🔎 Contexto adicionado: com 150 prompts do AIME, uma taxa de 0.000 tem pouco poder para mostrar qualquer coisa, então não consegue separar um procedimento válido, mas muito conservador, de um procedimento fraco.

## 🏗️ O que isso significa para construir sistemas
Esta é minha interpretação, não a afirmação dos autores. Se você usa amostragem repetida como portão automatizado, como votação sobre respostas de modelo para decidir se aceita uma saída ou para de gastar tokens, o certificado que você obtém responde a uma pergunta por requisição, não a uma afirmação de confiabilidade no nível de implantação, que o artigo diz exigir evidências agregadas entre prompts. Implementar isso significaria um serviço de amostragem somado a um pipeline de avaliação executado sobre um conjunto de prompts representativo e independente, com os rollouts piloto e held-out mantidos estritamente separados. O custo está sobretudo no prompt: com tamanhos efetivos de amostra próximos de dois rollouts por prompt, rollouts extras por prompt principalmente aumentam o gasto. Para confiar nisso, você precisaria de uma nova amostra de prompts do tráfego de produção e de sua própria estimativa de ρ, já que os números do artigo vêm de um único modelo.

## 🔑 Principais conclusões
- A garantia se aplica à afirmação, não ao método: o certificado de apostas padrão é exato para "este raciocínio sorteado favorece a resposta" e não exige conhecimento de ρ.
- Sob correlação, o mesmo certificado pode cruzar a afirmação populacional muito mais do que ϵ = 0.05; no AIME, a regra de unidade única dispara em 24.0% dos prompts enquanto θ-hat = -0.15.
- A correlação limita a informação que um prompt carrega: ρ-hat de 0.435 no GSM8K e 0.480 no AIME significam que um prompt vale cerca de 2.3 e 2.1 rollouts independentes para a questão populacional.
- Sob um orçamento puro de rollouts, um rollout por prompt em muitos prompts é a escolha que minimiza a variância para a validade populacional (Proposição 6.3).
- O e-process no nível do prompt é o remédio: a taxa de certificação na hipótese populacional cai para 0.033 com θ = 0 em simulação e para 0.000 no AIME, com validade exata sob independência entre prompts.
- O regime decide se a lacuna importa: no GSM8K, θ-hat = +0.96 a deixa dormente; no AIME, com θ-hat = -0.15, ela é saliente.

## 📚 Plano de leitura
### Do artigo
- [arXiv:2611.26592, este artigo](https://arxiv.org/abs/2611.26592) — leia primeiro as Seções 4 e 8, depois a Seção 6 para o remédio.
### Para se aprofundar
- Certified self-consistency (Cordero-Encinar and Duncan, 2025), arXiv:2510.17472 — o Martingale Majority Certificate com rollouts independentes que este artigo corrige.
- CITE: anytime-valid statistical inference in LLM self-consistency (Ota et al., 2026), arXiv:2605.05873 — certificação da resposta-alvo quando o conjunto de respostas é desconhecido.
- Anytime-valid answer sufficiency certificates (Akter et al., 2025), arXiv:2510.06478 — e-processes empírico-Bernstein autonormalizados para geração de LLMs.
- Game-theoretic statistics and safe anytime-valid inference (Ramdas, Grünwald, Vovk and Shafer, 2023) — o framework de e-process por trás das provas.
- Estimating means of bounded random variables by betting (Waudby-Smith and Ramdas, 2024) — as apostas adaptativas à variância que os autores recomendam.
- Survey Sampling (Kish, 1965) — a ideia de efeito de desenho por trás do limite 1/ρ.
- Training verifiers to solve math word problems (Cobbe et al., 2021), arXiv:2110.14168 — o benchmark GSM8K.
