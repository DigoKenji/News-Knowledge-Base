---
type: Paper Summary
title: 'MemFolder: Dobramento de Memória Guiado por Estrutura para Gerenciamento Eficiente de Memória de Agentes'
description: Consolidar registros de memória de agentes em unidades agrupadas mantém 99.0% da acurácia do Mem0 com 50% menos registros.
resource: https://arxiv.org/abs/2611.26427
tags:
- ai-ml
- ai-agent
- rag
- llm-as-a-judge
- agent-memory
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26427
  title: 'MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management'
  author: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
status: stable
source_url: https://arxiv.org/abs/2611.26427
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: ai-ml
concepts:
- ai-agent
- rag
- llm-as-a-judge
- agent-memory
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d8148b197f948b6a07596
lang: pt-br
translation_of: /notes/ai-ml/2026-11-21-memfolder-structure-guided-memory-folding-for-efficient-agen.md
---

## 🎯 Em uma frase
O MemFolder reduz o armazenamento de memória de um agente em duas etapas: primeiro mescla registros relacionados em menos unidades de recuperação e depois comprime o texto de cada unidade. No Mem0, com 50% menos registros, sua etapa RecordFolder mantém 99.0% do desempenho médio original nos três benchmarks de texto.

## ❓ O problema
Agentes de longa duração salvam cada interação como um registro de texto em um armazenamento de memória externo, e a recuperação traz registros de volta para o prompt do modelo a cada pergunta. Dois custos crescem com esse armazenamento. Um número maior de registros significa mais armazenamento, um índice maior e um espaço de busca mais amplo para cada consulta. Um contexto recuperado mais longo significa mais prefill (o trabalho do modelo para ler a entrada) e mais trabalho de atenção durante a decodificação, em toda chamada. Os armazenamentos sem compressão dos autores têm 6,817 registros totalizando 61,273 tokens nas dez amostras do LoCoMo, e 310,120 registros totalizando 3.7M tokens no LongMemEval. As soluções existentes atacam cada um desses custos isoladamente. A poda remove registros e pode descartar evidências úteis, enquanto a fusão gera menos registros, mas unidades mais longas: com 50% de retenção no LoCoMo, as unidades do MemForest e do ToMe tiveram em média 2.12x e 2.48x o comprimento original dos registros. A parte difícil é decidir quais registros pertencem juntos sem perder datas, nomes ou mudanças de estado.

## 💡 A ideia
Registros sobre a mesma entidade ou evento devem compartilhar uma unidade de recuperação, e o texto dessa unidade deve declarar os fatos compartilhados uma única vez, mantendo detalhes específicos de cada registro, como datas. Os autores separam duas decisões: quais registros formam um grupo (nível de registro) e como codificar cada grupo de forma compacta (nível de contexto). As duas decisões se ligam pelas atribuições de grupo, mas cada uma pode ser ajustada por conta própria. Os autores não dão analogia; seu argumento central é que o agrupamento reduz o número de unidades, a compressão reduz os tokens dentro de cada unidade, e juntos eles reduzem os dois custos sem a perda da poda.

## ⚙️ Como funciona
### 1. RecordFolder: consolidar N registros em K unidades
O RecordFolder recebe os registros do armazenamento com seus embeddings e timestamps e os mapeia para K grupos, em que K = max{1, round(κN)} para uma razão de manutenção κ. Um LLM primeiro extrai entidades (pessoas, obras, atividades, tópicos) e lhes atribui rótulos canônicos curtos, mantendo um registro de rótulos entre sessões e mesclando variantes de rótulo com similaridade de embedding de pelo menos θ_ent = 0.85. As âncoras são escolhidas uma por vez: a primeira por agrupamento por picos de densidade, as seguintes por novidade, de modo que cada nova âncora seja localmente densa e distante das âncoras já escolhidas. Em seguida, os registros são atribuídos à âncora mais próxima, e o emparelhamento húngaro liga os registros em cadeias temporais sem ramificações, mantendo uma ligação apenas quando seu ganho sobre a âncora do registro é positivo. Cada grupo é mesclado em uma única chamada ao LLM, em ordem temporal, e os registros singulares são mantidos como estão.

A afinidade usada para a ligação combina sobreposição de entidades com similaridade de embedding:

```text
affinity(i, j) = α_ij * e_ij + (1 − α_ij) * c_ij
e_ij = µ(E_i ∩ E_j) / µ(E_i ∪ E_j)
```

Aqui, e_ij é a similaridade de entidades entre os registros i e j, E_i é o conjunto de entidades do registro i, e µ(S) é a soma dos pesos de entidade w(e) = log(N / df(e)), de modo que entidades compartilhadas e raras contam mais. c_ij é a similaridade de cosseno entre os dois embeddings dos registros. α_ij é um peso de mistura adaptativo definido a partir das variâncias dos sinais, de modo que o sinal com maior dispersão recebe mais peso. Quando µ_ij é zero, a afinidade é igual à similaridade de cosseno.

### 2. ContextFolder: comprimir cada grupo sob um orçamento de tokens
O ContextFolder mantém fixa a composição de cada grupo e reescreve seus registros em uma codificação compacta. Um planejador (um LLM) lista os fatos atômicos do grupo, classifica cada um como de importância alta, média ou baixa, marca os fatos que podem ser inferidos de outros fatos e escreve uma pergunta de verificação com a resposta esperada para cada um. Um compressor escreve a codificação sob um orçamento que escala com o tamanho do grupo, e um verificador cego responde às perguntas de verificação apenas a partir da codificação, enquanto um juiz compara essas respostas com as respostas esperadas pelo planejador. O ciclo roda no máximo três rodadas, e a codificação é aceita quando os fatos obrigatórios passam e ela cabe em (1 + ε_B) vezes o orçamento. Se nenhum candidato passa, mantém-se a melhor recuperação ponderada, e se a compressão não encurtar o grupo, os textos de origem são mantidos como uma única unidade.

O orçamento é definido por grupo:

```text
B_G = max{3, round(τ * S_G)},    S_G = Σ_{m in G} ℓ(m)
```

B_G é o orçamento de tokens do grupo G, τ é a fração-alvo dos tokens de origem a manter, S_G é o comprimento total de origem do grupo, e ℓ(m) é o comprimento em tokens do registro m. O orçamento mínimo é de 3 tokens. A codificação usa uma pequena gramática que todo leitor recebe como legenda: @YYYY-MM-DD marca uma data de evento, A -> B uma mudança de estado, E: x um atributo, ~ uma negação e ? uma incerteza. Os fatos compartilhados são escritos uma única vez, e o verificador e o agente posterior decodificam a mesma legenda.

## 🧪 Evidências
| Cenário | Referência | Este artigo | Métrica |
|---|---|---|---|
| Mem0, 50% menos registros, média de 3 benchmarks de texto | MemForest: 59.3 (97.1% retido) | RecordFolder: 60.5 (99.0% retido) | Acurácia média; % do Mem0 não comprimido |
| Mem0, 50% menos registros, LongMemEval | MemForest: 51.8 | RecordFolder: 55.0 | Acurácia geral (%) |
| Mem0, 50% menos registros, LoCoMo | MemForest: 58.4 | RecordFolder: 59.3 | Acurácia geral (%) |
| Mem0, 70% menos registros, média de 3 benchmarks de texto | MemForest: 57.0 (93.3% retido) | RecordFolder: 59.0 (96.6% retido) | Acurácia média; % retido |
| Mem0, 70% menos registros, LongMemEval | MemForest: 52.2; ToMe: 49.6 | RecordFolder: 55.0 | Acurácia geral (%) |
| MemOS, 50% / 70% menos registros | MemOS não comprimido: 100% | RecordFolder: 91.0% / 84.8% retido | % do desempenho original |
| M3-Agent, 50% / 70% menos registros | M3-Agent não comprimido: 39.1 | RecordFolder: 39.6 / 40.1 (101.3% / 102.6%) | Acurácia média, subconjuntos robot e web |
| LongMemEval, 30% dos registros mantidos | Sem compressão: P95 97.5 ms | RecordFolder: P95 15.5 ms; speedup médio 5.34x | Latência de recuperação (média e P95) |
| PersonaMem, 30% dos registros mantidos | Sem compressão: média 1,900 ms | RecordFolder: média 538 ms (72% menor) | Latência média de recuperação |
| LoCoMo, 30% dos registros mantidos | Latência média sem compressão: 100% | RecordFolder: latência média 56% menor (2.26x) | Latência média de recuperação |

Os resultados em nível de registro são consistentes: o RecordFolder lidera em cada razão testada no Mem0, e sua maior vantagem sobre a melhor referência está no LongMemEval (55.0 contra 51.8 do MemForest a 50%), enquanto a margem no LoCoMo é de 0.9 ponto. As ablações no LoCoMo e no LongMemEval mostram que remover a similaridade de cosseno custa 3.4 pontos, a maior queda entre os sinais de afinidade e de encadeamento, e que a fusão supera a poda em 6.1 pontos em média sob as mesmas âncoras DPC. A avaliação usa o gpt-4o-mini para julgar respostas no Mem0 e no MemOS, e o gpt-4o no M3-Agent; cada resultado é a média de três execuções, mas as tabelas não reportam variância.

## ⚠️ Limitações
- O artigo não tem uma seção de limitações dedicada. As ressalvas abaixo são as que os autores declaram no texto.
- A verificação cega é imperfeita nos dois sentidos. O leitor pode deixar de recuperar um fato presente na codificação (Apêndice B.4), e um fato omitido pode passar pela verificação sem ser notado; os autores chamam o ciclo de uma heurística assimétrica de detecção de falhas, não de uma garantia.
- Os ganhos são menores em cenários multimodais: os autores observam que os ganhos de latência de cauda no M3-Bench-robot e no M3-Bench-web são menores que nos benchmarks de texto, porque a recuperação de registros é apenas parte da latência ali.
- Registros curtos e densos em informação deixam pouca redundância para o ContextFolder remover; no M3-Bench-robot, os autores relatam que o BabelTele atingiu acurácia maior (32.13%) que o ContextFolder, com um custo de tokens mais alto.
- 🔎 Contexto adicionado: o artigo apresenta duas reduções de tokens diferentes para o M3-Bench-robot, 31% na introdução e 34.1% na Seção 4.2 (20.9 contra 31.7 tokens por registro). Trate o valor exato como não resolvido.
- 🔎 Contexto adicionado: o artigo não informa o custo da etapa offline. Cada grupo pode exigir várias chamadas ao LLM (fundamentação, uma fusão, planejador, compressor e até três rodadas de verificação), e o mesmo modelo gpt-4o-mini escreve as memórias mescladas e julga as respostas, então os resultados dependem desse modelo.

## 🏗️ O que isso significa para construir sistemas
Interpretação, não afirmação dos autores: se você executa um assistente de longa duração sobre um armazenamento no estilo Mem0, o MemFolder poderia funcionar como uma tarefa offline de compactação após as escritas, reescrevendo grupos de registros em unidades antes de recalcular os embeddings e reindexar, de modo que a API de recuperação online possa permanecer a mesma. Seus custos são principalmente chamadas ao LLM durante a ingestão ou a compactação periódica, além da reindexação, e seus ganhos de latência só importam se a recuperação for o verdadeiro gargalo. Antes de confiar nele com seus próprios dados, você precisaria reproduzir traces reais e verificar se datas, negações e vínculos de entidades sobrevivem, porque o verificador cego do artigo é uma heurística, não uma prova. Do ponto de vista de governança, as unidades mescladas substituem os originais como forma armazenada, então seria preciso decidir se os registros brutos são mantidos para auditoria e para pedidos de exclusão.

## 🔑 Principais conclusões
- Com 50% menos registros no Mem0, o RecordFolder tem média de 60.5 contra 59.3 do MemForest, retendo 99.0% do desempenho original contra 97.1%.
- O ganho é mais claro no LongMemEval (55.0 contra 51.8 do MemForest a 50%); no LoCoMo a margem é de 0.9 ponto (59.3 contra 58.4).
- A similaridade de cosseno é o principal sinal de atribuição: removê-la reduz a média em 3.4 pontos, enquanto os termos de entidade e temporais acrescentam ganhos menores, de 1.9, 1.1 e 0.6 ponto.
- A seleção de âncoras por densidade e depois novidade (DPC) dá a melhor acurácia média, 57.2, entre os métodos de âncora testados na Tabela 4.
- Depois que o RecordFolder reduz pela metade o número de registros, o ContextFolder corta os tokens por registro em 40.3% no LoCoMo e 37.9% no LongMemEval, com perda de acurácia limitada.
- Com 30% dos registros mantidos, a latência média de recuperação fica 5.34x mais rápida no LongMemEval, e a latência de cauda cai de 97.5 para 15.5 ms, então o ganho não está apenas no caso médio.

## 📚 Plano de leitura
### Do artigo
- [Repositório de código do MemFolder](https://github.com/xuyang-liu16/MemFolder) — a implementação publicada; compare seus prompts com o Apêndice C antes de reutilizar qualquer um deles.
- [Token merging: Your ViT but faster (Bolya et al., 2023)](https://openreview.net/forum?id=JroZRaRw7Eu) — o método de fusão de tokens ToMe, que o artigo adapta de tokens visuais para registros.
- [Know me, respond to me: PersonaMem (Jiang et al., 2025)](https://openreview.net/forum?id=6ox8XZGOqP) — o benchmark de perfilamento de usuários por trás dos resultados do PersonaMem.
### Para aprofundar
- MemForest: EventTree partitioning and progressive merging (Wang et al., 2026b) — a referência mais forte; leia-o para ver exatamente o que o RecordFolder muda.
- Mem0: scalable long-term memory for production AI agents (Chhikara et al., 2025) — o framework de memória por trás dos principais resultados.
- LoCoMo: evaluating very long-term conversational memory of LLM agents (Maharana et al., 2024) — o principal benchmark de texto, útil para julgar se as diferenças importam.
- Clustering by fast search and find of density peaks (Rodriguez and Laio, 2014) — o critério de picos de densidade usado para escolher a primeira âncora.

## Conceitos
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [rag](/pt-br/concepts/ai-ml/rag.md)
- [llm-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md)
- [agent-memory](/pt-br/concepts/ai-ml/agent-memory.md)
