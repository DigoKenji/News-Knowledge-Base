---
type: Paper Summary
title: 'Jetriever: deslocando a recuperação de memória de agentes da similaridade para a utilidade'
description: Ranquear memórias armazenadas pela pontuação de utilidade de um modelo de decisão em LLM encontra memórias indiretamente relevantes que a busca por similaridade deixa passar.
resource: https://arxiv.org/abs/2611.21478
tags:
- ai-ml
- ai-agents
- rag
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.21478
  title: 'Jetriever: Shifting Agent Memory Retrieval from Similarity to Utility'
  author: Ruizhe Li, Licheng Zhang, Mingxuan Du, Zhendong Mao
status: stable
source_url: https://arxiv.org/abs/2611.21478
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-16'
domain: ai-ml
subdomain: ai-agents
concepts:
- rag
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d810d9be8fab230cc53cb
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-jetriever-shifting-agent-memory-retrieval-from-similarity-to.md
---

# Jetriever: deslocando a recuperação de memória de agentes da similaridade para a utilidade

**Fonte:** https://arxiv.org/abs/2611.21478
**Autor:** Ruizhe Li, Licheng Zhang, Mingxuan Du, Zhendong Mao
**Data:** 2026-11-16
**Conceitos:** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/rag.md) · [Throughput/latência/custo de inferência](/pt-br/concepts/inference-tradeoffs.md)

---

## 🎯 Em uma frase
O Jetriever pontua cada memória armazenada com o modelo de decisão Jev quanto à sua utilidade para a pergunta atual e ranqueia pela pontuação, colocando a troca de referência em primeiro lugar em 84,8% das 125 tarefas do InMind, enquanto os controles BM25 e MiniLM pareados chegam a 1,6% e 0,0%.

## ❓ O problema
A maioria dos sistemas de memória de agentes recupera qualquer texto que se pareça com a pergunta, usando pontuação por palavras-chave como o BM25 (uma fórmula clássica que favorece termos da consulta presentes em um documento) ou similaridade de embeddings (distância cosseno entre vetores de um encoder de texto como o MiniLM). Isso funciona quando a memória útil compartilha palavras ou entidades com a consulta, mas falha quando o vínculo é conhecimento de mundo: uma alergia a nozes armazenada importa para um pedido de receita de macarons que nunca menciona nozes, porque macarons geralmente contêm farinha de amêndoas. O benchmark InMind mostra o custo: um backbone que recebe diretamente a memória decisiva responde 84,0% das consultas indiretas, enquanto os seis sistemas de memória testados chegam, no máximo, a 14,4% de ponta a ponta. Pedir a um LLM que adivinhe termos de busca antes não resolve, porque o espaço de associações ocultas é grande demais para enumerar, e o modelo que responde nunca vê uma memória que o recuperador não devolveu. A parte difícil é pontuar a utilidade de cada memória armazenada a um custo baixo o suficiente para cobrir todo o conjunto.

## 💡 A ideia
Em vez de perguntar qual memória soa como a pergunta, o Jetriever pergunta, para cada memória real, se ela ajudaria a responder a pergunta. Os autores afirmam que isso exige três coisas ao mesmo tempo: conhecimento de mundo amplo para conectar uma memória a uma consulta por meio de um fato não declarado; pontuações que separem evidência decisiva de evidência apenas plausível (um recuperador não consegue ranquear centenas de memórias se considera quase todas úteis); e uma pontuação barata o suficiente para rodar sobre todo o conjunto. O Jev, um modelo de decisão construído sobre LLMs pré-treinados e treinado para decisões calibradas com RLCD (um método que o artigo atribui à TypeSafe), deve atender às três exigências, e sua pontuação de utilidade é usada diretamente como pontuação de recuperação. O resumo de uma linha dos autores é "Encontre a memória que importa para a resposta, não apenas uma que soe parecida."

## ⚙️ Como funciona
### Decisão de utilidade por par (Jev)
Para uma consulta q e um candidato de memória m_i, o sistema envia apenas o par ao Jev e solicita um julgamento binário, que o artigo chama de julgamento "Noul", sem definir o termo além disso. Para o InMind, o critério pergunta se o trecho de memória contém informação pessoal necessária para uma resposta correta e personalizada, incluindo informação indiretamente relevante, e instrui o Jev a julgar apenas o que aquele trecho contribui. Para as tarefas MAB mais amplas, o critério pergunta se a memória contém informação necessária para responder corretamente, incluindo fatos declarados diretamente e a evidência de apoio necessária. O Jev devolve uma pontuação s(q, m_i) que é usada apenas como sinal de ranking, e o artigo observa que essas pontuações são arredondadas para duas casas decimais. O Jev produz decisões estruturadas e probabilidades, em vez de texto livre, o que ajuda a explicar por que as chamadas são baratas.

### Regra de ranking
```text
pi_q = argsort_i ( -s(q, m_i), id(m_i) )
M_k(q) = { m_pi_q(1), ..., m_pi_q(k) }
```
Aqui q é a consulta, m_i é a i-ésima memória candidata e s(q, m_i) é a pontuação do Jev para aquele par. O argsort_i devolve os índices dos candidatos ordenados pelo par de chaves: primeiro a pontuação negativa, para que pontuações maiores venham primeiro, depois o identificador da memória. pi_q é a ordenação resultante para a consulta q, e M_k(q) é o conjunto de suas k memórias principais. O identificador existe apenas para desempatar, o que acontece porque as pontuações são arredondadas. O respondedor vê as k memórias principais, mas não a pontuação do Jev nem nenhuma justificativa gerada.

### Execução sobre o conjunto completo
Cada candidato recebe sua própria chamada ao Jev, então N candidatos significam N decisões independentes, sem um índice aproximado de vizinhos mais próximos. O artigo trata o paralelismo como o que torna isso viável: a execução do InMind tem média de 518 ms por requisição de API por candidato, com até 50 requisições concorrentes. Uma execução separada do LongMemEval pontua 6.446 pares consulta-troca em 90,16 s com concorrência de 100, cerca de 71,5 pontuações por segundo. O custo relatado é de US$ 0,00834 por pergunta do InMind (241 candidatos) e, em média, US$ 0,03103 por pergunta do MAB (média de 1.258,2 conversas). Os autores enfatizam que o número de 90,16 s mede o throughput em lote, e não a latência online por pergunta.

## 🧪 Evidências
| Configuração | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| InMind-125, 241 trocas por consulta | BM25, turno do usuário: 1,6 | Jetriever: 84,8 | R@1 (%) |
| InMind-125, 241 trocas por consulta | MiniLM, turno do usuário: 0,0 | Jetriever: 84,8 | R@1 (%) |
| InMind-125, 241 trocas por consulta | BM25, turno do usuário: 4,8 | Jetriever: 92,0 | R@5 (%) |
| InMind-125, 241 trocas por consulta | MiniLM, turno do usuário: 8,8 | Jetriever: 94,4 | R@10 (%) |
| InMind-125, aplicação de ponta a ponta | text-embedding-3-large: 16,0 | Jetriever, cinco primeiros: 76,0 | Aplicação (%) |
| InMind-125, aplicação de ponta a ponta | Backbone, fato alvo no contexto: 84,0 | Jetriever, cinco primeiros: 76,0 | Aplicação (%) |
| 20 perguntas do InMind, 241 candidatos | Qwen3.5 9B: 40 (8 de 20) | Jev: 85 (17 de 20) | R@1 (%) |
| 20 perguntas do InMind, 241 candidatos | Qwen3.5 9B: 90 (18 de 20) | Jev: 90 (18 de 20) | R@50 (%) |
| 20 perguntas do InMind, 241 candidatos | Laya: 30 (6 de 20) | Jev: 90 (18 de 20) | R@50 (%) |
| Amostra de 60 perguntas do MAB | ReFind: 44,3 | Jetriever: 55,4 | Média macro das quatro dimensões (%) |
| InMind-125, custo de API de recuperação | não se aplica | US$ 0,00834 por pergunta | USD, apenas recuperação |
| Amostra de 60 perguntas do MAB, custo de API de recuperação | não se aplica | US$ 0,03103 por pergunta | USD, apenas recuperação |

Os resultados do InMind são a evidência central: em conjuntos idênticos de 241 trocas, o Jetriever coloca a troca de referência em primeiro lugar em 106 das 125 tarefas e entre as cinco primeiras em 115, enquanto os controles baseados no turno do usuário chegam a 6 e 3 de 125 na posição cinco. Trocar a unidade de candidato de 241 trocas para 47 sessões reduz o R@1 de 84,8% para 49,6%, e o artigo interpreta isso como uma sessão longa que enterra o fato. No MAB, a média macro da amostra é 55,4% contra 44,3% do ReFind, com as quatro dimensões em AR 65,5 vs 49,8, TTL 40,0 vs 30,0, LRU 59,0 vs 55,5 e SF 57,1 vs 41,7. Os resultados não mostram o comportamento em armazenamentos maiores, a latência por pergunta, nem uma comparação executada pelo mesmo pipeline de respostas.

## ⚠️ Limitações
- Os autores afirmam que a amostra do MAB são 60 de 3.671 perguntas, sem perguntas do SH-QA, então suas médias não são as pontuações oficiais do benchmark completo.
- Os autores afirmam que a comparação com o ReFind é em nível de sistema: as respostas do ReFind vêm de uma execução anterior e não foram reexecutadas com o pipeline de respostas do Jetriever.
- Os autores afirmam que a pontuação sobre o conjunto completo é prática nos tamanhos testados, mas não é gratuita em nenhuma escala, e que uma cascata de primeira etapa mais barata, distribuições de latência por pergunta e testes diretos de calibração continuam não testados.
- Os autores descrevem o artigo como "um artigo modesto" que não atinge o padrão de uma conferência de IA convencional, e o marcam como rascunho de pesquisa.
- 🔎 Contexto adicional: a configuração do InMind planta uma memória decisiva entre 240 trocas de fundo. Armazenamentos reais podem conter várias memórias quase duplicadas ou contraditórias, e o artigo não testa o ranking nessas condições.
- 🔎 Contexto adicional: o InMind é o benchmark anterior do mesmo grupo (a referência 1 compartilha três autores com este artigo), então a evidência principal usa um benchmark que os autores desenharam para o fenômeno que estudam.

## 🏗️ O que isso significaria para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. O Jetriever ficaria como uma etapa de rerank sobre um conjunto de memórias candidatas: para um armazenamento por usuário de algumas centenas de itens, o custo relatado por consulta é pequeno perto de uma chamada de resposta, mas um armazenamento com dezenas de milhares de memórias multiplicaria decisões e latência. Os autores sugerem uma primeira etapa barata, mas ela precisa manter os candidatos indiretos, que filtros lexicais ou de embeddings podem descartar. Antes de confiar nisso, você precisaria de um conjunto próprio de consultas indiretas com gabarito, de uma verificação de que as pontuações separam candidatos no seu domínio, e de latência medida por requisição na concorrência de produção, já que o artigo não fornece esses dados. A questão que isso levanta para você é quais dos seus armazenamentos de memória guardam fatos que importam por conhecimento de mundo, e não por palavras em comum, e se uma memória perdida é custosa o suficiente para justificar pontuação por item com LLM.

## 🔑 Principais conclusões
- A recuperação por similaridade falha quando o vínculo com a consulta é conhecimento de mundo: com indexação pelo turno do usuário, o BM25 coloca a troca de referência em primeiro lugar em 1,6% das tarefas do InMind, e o MiniLM em 0,0%.
- O Jetriever ranqueia cada memória armazenada com a pontuação de julgamento binário do Jev e chega a 84,8% na posição um (106 de 125), 92,0% entre as cinco primeiras e 94,4% entre as dez primeiras.
- Encontrar a memória de referência entre as 50 primeiras não é o mesmo que ranqueá-la em primeiro: Jev e Qwen3.5 9B encontram ambas 90% até a posição 50, mas na posição um o Jev chega a 85% e o Qwen3.5 9B a 40%.
- A unidade de pontuação importa: o R@1 cai de 84,8% com 241 trocas para 49,6% com 47 sessões.
- Em uma amostra de 60 perguntas do MAB, a média macro das quatro dimensões é 55,4%, contra 44,3% do ReFind nas mesmas perguntas.
- O custo de recuperação é pequeno nesses tamanhos de conjunto (US$ 0,00834 por pergunta do InMind, US$ 0,03103 por pergunta do MAB), mas o artigo deixa em aberto a escalabilidade para armazenamentos muito maiores.

## 📚 Plano de leitura
### A partir do artigo
- [Keep It InMind (referência 1)](https://arxiv.org/abs/2607.24368) — o benchmark por trás das 125 tarefas e do argumento do ponto cego.
- [Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions (referência 2)](https://arxiv.org/abs/2507.05257) — MemoryAgentBench, o segundo benchmark.
- [When Your Agent Opens the Chat App (referência 3)](https://arxiv.org/abs/2608.12888) — ReFind, a fonte da comparação no MAB.
- [BM25 Wins at Scale (referência 4)](https://arxiv.org/abs/2607.26497) — evidência de que o BM25 é uma linha de base forte para RAG.
- [Is Complex Agentic Memory Always Needed? (referência 5)](https://arxiv.org/abs/2610.30755) — RAG ingênuo versus sistemas de memória.
- [A-RAG (referência 7)](https://arxiv.org/abs/2602.03442) — busca por palavras-chave e semântica escolhida pelo agente, uma linha de base.
- [TypeSafe, AI primer: RLCD and calibrated decisions (referência 8)](https://docs.typesafe.ai/introduction/machine-learning-primer) — como o Jev é treinado para decisões calibradas.
- [Introducing System One Models & Jev (referência 9)](https://typesafe.ai/blog/introducing-system-one-models-and-jev) — o modelo de decisão que o Jetriever usa.
- [Laya Typed-Decisions model card (referência 10)](https://huggingface.co/convaiinnovations/laya-typed-decisions) — o pontuador baseado em BERT na comparação de 20 perguntas.
- [Qwen3.5-9B model card (referência 11)](https://huggingface.co/Qwen/Qwen3.5-9B) — o controle com pontuador LLM.
- [all-MiniLM-L6-v2 model card (referência 16)](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) — o encoder do controle por similaridade.
- [LongMemEval (referência 20)](https://arxiv.org/abs/2410.10813) — o benchmark de memória de longo prazo usado na execução de throughput em lote.
- [Repositório do Jetriever](https://github.com/imlrz/Jetriever) — o código e os dados do projeto, como vinculado na capa do artigo.

### Para ir além
- The Probabilistic Relevance Framework: BM25 and Beyond (Robertson e Zaragoza, 2009) — por que a pontuação por palavras-chave é uma linha de base tão forte.
- Calibração de probabilidades de decisão de LLMs — o artigo diz que testes diretos de calibração ainda são necessários.
- Recuperação em cascata: uma primeira etapa barata antes da pontuação por item, cara — o próximo experimento proposto pelo artigo.
- Índices aproximados de vizinhos mais próximos — a alternativa que o desenho de rerank completo do artigo evita.
