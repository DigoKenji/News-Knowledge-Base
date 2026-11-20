---
type: Paper Summary
title: 'SoK: Avaliando a evolução da capacidade dos LLMs na pesquisa de vulnerabilidades de software'
description: 'Agentes de LLM agora encontram, validam e corrigem vulnerabilidades reais sem supervisão humana, mas os prazos de correção e de ataque ainda não mudaram.'
resource: https://arxiv.org/abs/2611.26601
tags:
- security
- llm-as-a-judge
- multi-agent-orchestration
- zero-trust-architecture
- agent-harness
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26601
  title: 'SoK: Assessing the Evolution of LLM Capability in Software Vulnerability Research'
  author: arXiv
status: stable
source_url: https://arxiv.org/abs/2611.26601
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: security
concepts:
- llm-as-a-judge
- multi-agent-orchestration
- zero-trust-architecture
- agent-harness
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81d79667c82fd2cada38
lang: pt-br
translation_of: /notes/security/2026-11-20-sok-assessing-the-evolution-of-llm-capability-in-software-vu.md
---

## 🎯 Em uma frase
Agentes de LLM deixaram de seguir passos prescritos e passaram a descobrir, validar e corrigir software real sem supervisão, atingindo precisão de especialista em descoberta e validação já em agosto de 2025. Enquanto isso, os registros CVE subiram de 39,638 para 89,749 entre janeiro e outubro de 2025 e de 2026, e o tempo para corrigir e o tempo para atacar mal se moveram.

## ❓ O problema
Encontrar uma vulnerabilidade de software é só o primeiro de três trabalhos. A descoberta precisa ser validada, normalmente por uma prova de vulnerabilidade (PoV, de *proof of vulnerability*), ou seja, uma entrada ou procedimento que mostra que a falha é real, como um exploit funcional. Depois, a correção precisa remover a causa raiz sem quebrar o software. Fuzzers e analisadores estáticos produzem muitos candidatos, a maioria falsos, então a triagem, a prova e o reparo continuam nas mãos de especialistas humanos. Desde 2024, agentes baseados em LLM têm relatado vulnerabilidades reais, mas as evidências estão espalhadas por artigos, relatórios de fornecedores e divulgações. A maior parte vem das empresas que construíram os sistemas, e as alegações raramente são comparáveis. Existem registros públicos, como CVEs, avisos de segurança e listagens de exploits, mas eles não tinham sido ligados ao que os sistemas realmente conseguem fazer.

## 💡 A ideia
Os autores tratam a questão como um problema de medição. Eles tomam emprestados os níveis SAE de automação de direção, em que cada nível é definido pela parte da tarefa de direção que continua com o humano. Aqui, cada nível é definido pelo que um sistema comprovadamente faz em alvos reais, separadamente para cada tarefa. Um sistema só recebe um nível se a maioria de seus resultados se sustentar. Cada alegação também é marcada com sua correção, seu tier de acesso e a força da evidência. O mapa é então comparado com uma década de registros públicos, para que as expectativas sobre as consequências possam ser checadas contra o que de fato aconteceu.

## ⚙️ Como funciona
### Arcabouço de avaliação: autonomia, correção, acesso, evidência
Cada resultado é um par de nível de autonomia e categoria de correção, por exemplo A4·C2, datado a partir da primeira fonte que mostra os dois valores para o mesmo sistema. A0 significa nenhuma contribuição; A2 significa executar passos que um humano especifica; A3 significa planejar e executar a tarefa inteira, com um humano ainda preparando os alvos; A4 significa concluir a tarefa em um alvo dado, sem intervenção humana; A5 também escolhe os próprios alvos. Correção é a parcela dos resultados entregues que se sustenta (precisão): C1 significa que a maioria se sustenta, C2 significa tanto quanto um especialista, e C3 significa precisão de especialista em código que especialistas e fuzzers examinaram por anos. Os tiers de acesso vão de T1 (somente laboratório) a T4 (pesos abertos), e os graus de evidência vão de E1 (relatado pelo próprio construtor) a E3 (confirmado por uma parte independente).

```text
step = (autonomy level, correctness category), dated by the first source
cell = (capability, access tier)        e.g. Patch, T2
```
Uma célula é uma capacidade em um tier de acesso, e cada célula contém uma escada datada de passos. As quatro capacidades são descobrir, validar vulnerabilidade, corrigir e validar correção. Valores de benchmark (marcados com cruz dupla) e valores inferidos (marcados com cruz simples) são mantidos separados da evidência direta.

### Pipeline de evidências
Os autores construíram um pipeline de múltiplas etapas. Uma busca sistemática com 67 consultas e relatórios de 105 organizações produziu um corpus de 53,057 fontes. Dois agentes independentes marcaram 9,859 como relevantes, e 6,604 desses eram de 2024 em diante e tratavam de software real. Agentes então leram 1,100 delas na íntegra e citaram uma frase para cada valor. Um script confirmou que 99.6% dessas citações aparecem palavra por palavra nas fontes, o que descarta citações inventadas, mas não descarta apoio fraco. Cada célula foi pontuada duas vezes: uma a partir das leituras e outra buscando no corpus e na web. Agentes de decisão separados argumentaram a favor de níveis mais altos e mais baixos, e um verificador aceitou 34 propostas e rejeitou 30. O mapa final tem 128 passos, e dois autores revisaram cada um deles em relação às suas fontes.

### Medição com registros públicos
A segunda metade compara 503 declarações verificadas de laboratórios, mantenedores, agências e pesquisadores com cinco grandezas medidas: volume e severidade de CVEs, a idade do código vulnerável, a parcela de registros que nomeiam um sistema de IA como descobridor, o tempo entre o relato e a correção, e o tempo entre o registro e a exploração. As declarações foram transformadas em sete consequências comumente esperadas, como "as descobertas vão disparar" e "exploits vão surgir em horas após as correções". Cada uma delas é então confrontada com o que foi demonstrado e com o que os registros mostram.

## 🧪 Evidências
| Cenário | Referência | Este artigo | Métrica |
|---|---|---|---|
| Descoberta e validação, todos os tiers | Capaz (A4 C1) | Especialista (A4 C2), atingido em agosto de 2025 | Nível de autonomia e categoria de correção |
| Tier de parceiros, Claude Mythos Preview, código auditado por anos | Especialista em código geral (A4 C2) | A4 C3, maio de 2026; 90.6% de 1,752 descobertas avaliadas confirmadas por firmas independentes | Parcela de descobertas confirmadas |
| Tier público, Aikido Attack | Nenhum relatado | Especialista (A4 C2); firma de segurança confirmou 96% das descobertas | Parcela de descobertas confirmadas |
| GLM 5.3 de pesos abertos vs. Mythos Preview, exploits de ponta a ponta | Mythos Preview | 12% de 410 tentativas vs. 14% | Parcela de tentativas bem-sucedidas (comparação da Anthropic) |
| Correção, tier de parceiros (Defense Factory) | Nenhuma relatada | Especialista (A4 C2), setembro de 2026; 0.53% das correções revertidas | Parcela de correções revertidas |
| Validação de correções, juiz LLM em um benchmark | Leitura humana cuidadosa | Concordância em 85% dos casos | Taxa de concordância |
| Sistemas públicos depois do primeiro sistema A4, descobrir, capaz | Primeiro sistema (mês 0) | 5.0 meses [2.8, 9.9] | Meses, com intervalo (Tabela 4) |
| Sistemas de pesos abertos depois do primeiro A4, descobrir, especialista | Primeiro sistema (mês 0) | 13.7 meses [pelo menos 5.0] | Meses, com intervalo (Tabela 4) |
| Registros CVE, janeiro a outubro | 39,638 (2025) | 89,749 (2026) | Registros publicados |
| Parcela classificada como Critical ou High | 46% (2025) | 56% (julho a outubro de 2026) | Parcela dos registros pontuados |
| Bugs com 10 anos ou mais, Critical ou High | Taxa mensal de 2025 | 6 a 11 vezes mais frequentes (julho a outubro de 2026) | Múltiplo da taxa mensal, quando a idade é conhecida |
| Do relato à correção, Zero Day Initiative | 96 a 119 dias, todos os anos de 2016 a 2026 | 148 dias no último trimestre (28 casos) | Mediana de dias |
| Relatos em massa a mantenedores externos | Relatos curados e OSS-Fuzz: 58 a 94% corrigidos | 5 a 26% corrigidos | Parcela de relatos corrigidos |

Os resultados mostram que agentes de LLM atingiram precisão de especialista em software real em cerca de um ano, e que sistemas públicos e de pesos abertos seguiram em questão de meses. Duas coisas que eles não mostram são uma taxa estabelecida de exploração e a parcela dos registros devida a LLMs. As ablações importam aqui. Quando só se mantêm evidências E2 e E3, a descoberta e a validação ainda chegam a A4, mas em agosto de 2025 em vez de junho, e a validação de correções cai para A3 no tier público. A base de evidências é grande, mas desigual: a validação de correções se apoia sobretudo nos próprios relatos dos desenvolvedores.

## ⚠️ Limitações
- A maior parte da evidência vem das organizações que constroem os sistemas. A validação de correções se apoia sobretudo nos próprios relatos delas (E1), e a remediação é a tarefa menos medida.
- Valores de benchmark (‡) e valores inferidos (†) são relatados separadamente. A correção em pesos abertos é inferida, e os atrasos dos pesos abertos são limites superiores, porque datam o primeiro uso documentado.
- A maioria dos registros CVE não nomeia nenhuma ferramenta, então as contagens de vulnerabilidades encontradas por IA são limites inferiores, e a parcela da alta causada por LLMs ainda não é conhecida.
- Nenhuma fonte testa uma mitigação contra um atacante baseado em LLM, então as notas de mitigação descrevem efetividade geral, não efetividade contra esses sistemas.
- Correção é precisão. Revocação raramente é relatada, então as vulnerabilidades que um sistema deixa passar são desconhecidas.
- 🔎 Contexto adicionado: a classificação de cada passo foi feita por agentes de LLM, em sua maioria modelos Claude, que também estão entre os sistemas avaliados. A verificação dos autores cobriu 16 células, e o artigo não analisa se isso produz um viés sistemático.
- 🔎 Contexto adicionado: a alegação de que restringir modelos de fronteira "atrasa a disseminação apenas em meses" se apoia em poucas comparações, uma delas a comparação do próprio laboratório entre GLM 5.3 e Mythos Preview. Os intervalos da Tabela 4 são amplos (por exemplo, 5.0 [2.8, 9.9]).

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. Agentes de descoberta se conectariam a CI e à varredura de segurança, e agentes de validação de correções, ao gate de revisão antes de uma correção ser integrada. O artigo sugere que o gargalo agora é a correção e sua implantação, então o primeiro investimento está em automação de dependências, cadência de release, rollback e medição do tempo até a implantação. O custo de executar agentes não é quantificado no texto, apenas representado na Figura 5. Para confiar em um pipeline desses, você precisaria de uma prova funcional para cada relato, apenas relatos confirmados e suas próprias taxas de rollback e de confirmação, no espírito da figura de 0.53% da Defense Factory. O artigo também trata isso como motivo para presumir que atacantes podem alcançar a mesma capacidade, o que aponta para compartimentalização e privilégio mínimo como defesa.

## 🔑 Principais conclusões
- Agentes não supervisionados atingiram precisão de especialista (A4 C2) para descoberta e validação em sistemas públicos cerca de nove meses depois do primeiro sistema (Tabela 4).
- Código auditado por anos foi encontrado em A4 C3 por um sistema do tier de parceiros em maio de 2026, com 90.6% de 1,752 descobertas avaliadas confirmadas.
- A correção atingiu A4 C1 em outubro de 2025, mas a correção de especialista (C2) só veio em setembro de 2026, e a validação de correções é a tarefa menos medida.
- Os registros CVE mais que dobraram em 2026, e a parcela classificada como Critical ou High subiu de 46% para 56%.
- O tempo entre o relato e a correção (96 a 119 dias) e entre o registro e o ataque listado (2 a 5 dias) não mudou de forma perceptível, e a maioria dos relatos em massa a mantenedores externos segue sem correção.
- As mitigações mais bem sustentadas são práticas consolidadas: linguagens com segurança de memória, superfície de ataque menor, compartimentalização e atualizações automáticas.

## 📚 Plano de leitura
### Do artigo
- [Anthropic, Assessing Claude Mythos Preview's cybersecurity capabilities](https://www.anthropic.com/research/mythos-preview) — o sistema do tier de parceiros por trás do resultado A4 C3 e de suas 1,752 descobertas avaliadas.
- [Anthropic, Measuring LLMs' impact on N-day exploits](https://www.anthropic.com/research/n-days) — a evidência de que exploits podem ser construídos a partir de correções públicas em poucas horas.
- [Levels of autonomy for AI agents (arXiv:2506.12469)](https://arxiv.org/abs/2506.12469) — a taxonomia de autonomia na qual o arcabouço se baseia.
- [SAE J3016, Taxonomy and definitions for driving automation](https://saemobilus.sae.org/content/j3016_202104) — os níveis de automação de direção nos quais a escala de autonomia é modelada.
- [CVE List v5 records (CVEProject)](https://github.com/CVEProject/cvelistV5) — a fonte pública de registros para as medições de volume, severidade e idade.
- [PatchEval (ByteDance)](https://github.com/bytedance/PatchEval) — o benchmark de correções reais de CVE usado para os valores de validação de correções.
### Para aprofundar
- CyberGym: avaliando as capacidades de segurança cibernética reais de agentes de IA em escala — o benchmark que o artigo usa como evidência complementar.
- Divulgação coordenada de vulnerabilidades e a janela de correção de 90 dias — a política contra a qual os resultados de tempo até a correção são medidos.
- Milk or wine: does software security improve with age? — o estudo clássico sobre quanto tempo vulnerabilidades vivem no código, relevante para a descoberta de bugs antigos.
