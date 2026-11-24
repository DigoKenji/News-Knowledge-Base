---
type: Paper Summary
title: 'Callo: atendendo requisições com cache miss via decodificação especulativa sobre KV invalidado, sob desagregação prefill-decode'
description: O Callo gera rascunhos de tokens sobre um cache KV desatualizado enquanto o prefill exato roda, confirmando apenas os tokens verificados, com speedups de 1,05 a 1,19x sobre o vLLM em uma carga de trabalho do SWE-agent.
resource: https://arxiv.org/abs/2611.18459
tags:
- ai-ml
- llm-engineering
- kv-cache
- speculative-decoding
- prefill-decode-disaggregation
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.18459
  title: 'Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation'
  author: Jianjun Ou, Matthew Lentz
status: stable
source_url: https://arxiv.org/abs/2611.18459
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-13'
domain: ai-ml
subdomain: llm-engineering
concepts:
- kv-cache
- speculative-decoding
- prefill-decode-disaggregation
- inference-tradeoffs
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d8191acbce9e684f19975
lang: pt-br
translation_of: /notes/ai-ml/2026-11-16-callo-speculative-decoding-invalidated-kv.md
---

# Callo: atendendo requisições com cache miss via decodificação especulativa sobre KV invalidado, sob desagregação prefill-decode

**Fonte:** https://arxiv.org/abs/2611.18459
**Autores:** Jianjun Ou, Matthew Lentz
**Instituição:** Duke University
**Publicado:** novembro de 2026 (submetido em 13 nov 2026, arXiv:2611.18459v1) | **Veículo:** Pré-print
**Tags:** #llm-serving #speculative-decoding #agents #prefill-decode-disaggregation

---

## 📄 Resumo (em linguagem simples)

Sessões de agentes frequentemente editam o seu contexto, o que invalida o [cache KV](/pt-br/concepts/ai-ml/kv-cache.md) do sufixo editado e força um novo prefill antes da decodificação. O CALLO gera rascunhos de tokens a partir desse KV invalidado enquanto o KV exato é recalculado, e então verifica os rascunhos contra o KV exato antes de confirmá-los. Em um testbed com duas GPUs e o SWE-agent, ele relata speedups de 1,05 a 1,19× sobre o vLLM para todas as requisições, com carga moderada.

## 🎯 Contribuição central

Os autores argumentam que a penalidade do miss é uma espera na instância de decodificação, e que essa instância já contém o KV invalidado, que está próximo o bastante do KV exato para servir de base aos rascunhos [Kwon et al., 2023]. As soluções existentes não se encaixam neste caso: a [Desagregação prefill-decode](/pt-br/concepts/ai-ml/prefill-decode-disaggregation.md) [Zhong et al., 2024] tira o prefill do caminho de decodificação, mas não ajuda a sessão afetada, e métodos de KV aproximado, como o de [Ma et al., 2026], podem alterar as saídas. O CALLO se mantém sem perdas ao usar o KV invalidado apenas para propor tokens e ao confirmar somente o que o KV exato confirma.

## 🏗️ Metodologia

### Formulação do problema

Um miss induzido por edição coloca o re-prefill exato no caminho crítico. O objetivo é encurtar essa espera sem alterar as saídas; a penalidade do miss é o tempo até o KV exato chegar à instância D.

### Método proposto: CALLO

O CALLO roda sobre um vLLM desagregado, com uma instância P (prefill) e uma instância D (decodificação). A instância D gera rascunhos com o modelo alvo sobre o KV invalidado reposicionado, usando [Decodificação especulativa](/pt-br/concepts/ai-ml/speculative-decoding.md) no sentido de rascunhar e verificar de [Leviathan et al., 2023] e [Chen et al., 2023].

### Staging provisório do KV

A instância D alinha os tokens da requisição anterior (incluindo a sua resposta) com o novo prompt, copia o KV das posições mapeadas para as novas posições e recalcula as chaves com um kernel Triton fundido. Os trechos não mapeados são calculados como chunks comuns de prefill, de no máximo 112 tokens; a mediana é de 424 tokens por requisição com miss especulada.

### Rascunho sobreposto e streaming

A instância P envia o KV exato de cada camada assim que ele é calculado, e a instância D gera rascunhos sobre os mesmos blocos, sem locks.

### Verificação

Quando o KV exato está completo, uma única passagem forward sobre os rascunhos aceita-os até a primeira rejeição, usando apenas o KV exato.

### Decisões de design principais

- **Somente commit sem perdas:** rascunhos feitos com KV aproximado nunca são confirmados, então as saídas coincidem com a regra de decodificação do modelo alvo.
- **Entrega por camada:** os rascunhos são aceitos com bem mais frequência quando metade das camadas já é exata; a entrega por posição não traz ganho significativo em relação ao KV invalidado.
- **Parada antecipada cumulativa (θ = 0,3):** a geração de rascunhos para quando o produto das probabilidades de concordância de cada rascunho cai abaixo de 0,3. Isso reduz em 30% os rascunhos descartados, mantendo 97% dos aceitos.
- **Cessão sob contenção:** requisições de rascunho com a menor aceitação cumulativa cedem os slots primeiro, mantendo seus rascunhos.

## 🧪 Configuração experimental

Duas GPUs H200 NVL (uma instância P e uma instância D) rodam o vLLM 0.29 com transferência NIXL. Os modelos são Llama-3.3-70B-Instruct (FP8), Gemma-4-31B e Mistral-Small-3.2-24B (bf16). A carga de trabalho é o SWE-agent 1.1 em 100 issues do SWE-bench Verified, mantendo as últimas cinco (L5) ou dez (L10) saídas de ferramentas, com cargas de 12%, 52% e 100% da capacidade, derivadas de tráfego de programação da Azure [Stojkovic et al., 2025]. As linhas de base são o vLLM sem modificações e o CALLO-Stream (o caminho de transferência do CALLO com a especulação desativada). A métrica é a razão entre a latência média do vLLM e a do CALLO, informada para as requisições com miss e para todas as requisições.

## 📊 Resultados

### Resultados principais

Razões de latência média (vLLM / CALLO), com limite de lote da instância D igual a 16:

| Carga de trabalho | Leve: miss | Leve: total | Moderada: miss | Moderada: total | Plena: miss | Plena: total |
|---|---|---|---|---|---|---|
| Llama L5 | 1.13 | 1.10 | 1.19 | 1.14 | 1.32 | 1.27 |
| Llama L10 | 1.24 | 1.17 | 1.24 | 1.19 | 1.21 | 1.17 |
| Gemma L5 | 1.07 | 1.05 | 1.06 | 1.05 | 1.10 | 1.09 |
| Gemma L10 | 1.11 | 1.07 | 1.11 | 1.08 | 1.12 | 1.10 |
| Mistral L5 | 1.15 | 1.14 | 1.16 | 1.14 | 1.16 | 1.15 |
| Mistral L10 | 1.21 | 1.15 | 1.19 | 1.14 | 1.15 | 1.13 |

Para o Llama L5, o speedup em todas as requisições sobe de 1,10× com carga leve para 1,27× com carga plena. Só a especulação responde por 8% da latência média de todas as requisições com carga leve e por 10% com carga plena, em relação ao CALLO-Stream. Com limite de lote igual a dois, o speedup fica perto de 1,10–1,12×, sobretudo pelo caminho de transferência. Nas requisições com miss especuladas, com carga moderada, o KV exato chega 0,4 s antes e o restante da decodificação encolhe mais 0,4 s, eliminando 47% da penalidade do miss.

### Estudos de ablação

Execuções com uma única requisição no Llama-3.3-70B (Tabela 4):

- O CALLO com empate fixo esconde 49% da penalidade do miss (1,15×); o CALLO-Stream esconde 4,5% (1,01×).
- A verificação any-max esconde 54% (1,17×); a amostragem especulativa com T = 0,8 esconde 45% (1,14×).
- Os n-gramas de prompt-lookup [Saxena, 2023] escondem apenas 18% (1,05×), com 7,5 rascunhos aceitos contra 22,6.

### Corretude e custos

Em 1.928 eventos de verificação e 44.792 tokens confirmados, as execuções instrumentadas não mostram divergências de fingerprint de KV nem violações de aceitação, saída ou rollback. Requisições sem especulação no mesmo D-instance ficam cerca de 2% mais lentas com carga baixa e 6% mais lentas perto da capacidade; os ganhos informados já incluem esse custo. O resultado é um trade-off entre latência e capacidade do tipo discutido em [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md).

### Simulação em escala de cluster

Em um cluster simulado de 96 GPUs, o speedup da especulação nas requisições com miss em relação ao CALLO-Stream é de 1,110–1,131× com carga leve, em diferentes razões P:D. Ponderado pela carga semanal da Azure, ele é de 1,130×, 1,095×, 1,044× e 1,027× para as razões P:D de 1:2, 1:1, 2:1 e 3:1. Essas comparações são com o CALLO-Stream, e não com o vLLM.

### Limitações reconhecidas pelos autores

- Os resultados do testbed cobrem duas GPUs e um conjunto pequeno de issues; os resultados de cluster são simulados.
- O CALLO precisa de afinidade de sessão, do KV da requisição anterior ainda em cache e de capacidade ociosa de decodificação; caso contrário, as requisições seguem o caminho comum.
- Os rascunhos falham onde o modelo hesita, então os ganhos dependem da confiança do modelo; para saídas de raciocínio muito longas, o ganho relativo é pequeno.
- Além da capacidade, as filas continuam crescendo e as razões não representam ganhos em regime estacionário.
- Não se sabe se caches calculados antes de uma troca de adaptador geram bons rascunhos; o assunto fica em aberto.

## 💡 Principais insights

- O KV invalidado erra apenas na edição, então o erro dele é estruturado: o próximo token previsto coincidiu com o KV exato em 98,3% das posições.
- As falhas dos rascunhos são previsíveis pela diferença de logit do próprio modelo alvo. Quando essa diferença é de pelo menos um, a concordância com o modelo exato fica entre 97% e 100%; abaixo de 0,5, ela cai para 65%. Os empates próximos são 2,4% das posições de rascunho, mas 60% das primeiras divergências.
- Os autores posicionam o CALLO para tarefas sensíveis à latência com respostas curtas, nas quais a espera pelo re-prefill é uma parte grande da latência restante.

## 🔍 Referências para aprofundamento

Extraídas da própria bibliografia do artigo.

### Trabalhos fundacionais em que este artigo se apoia

- 📚 Leviathan et al., 2023 — *Fast inference from transformers via speculative decoding* (ICML) → O esquema de rascunhar e verificar do qual dependem as regras de verificação do CALLO.
- 📚 Chen et al., 2023 — *Accelerating large language model decoding with speculative sampling* (arXiv:2302.01318) → Amostragem especulativa, um dos modos de verificação do CALLO.
- 📚 Kwon et al., 2023 — *Efficient memory management for large language model serving with PagedAttention* → O motor vLLM e a semântica de cache de prefixo sobre os quais o CALLO se constrói.
- 📚 Zhong et al., 2024 — *DistServe: Disaggregating prefill and decoding* (OSDI) → Desagregação prefill-decode, que o CALLO estende.

### Métodos concorrentes ou relacionados citados diretamente

- 📄 Ma et al., 2026 — *Leyline: KV-cache edit directives for efficient agentic reasoning* → Reposiciona o sufixo após uma edição de trecho, mas decodifica com KV aproximado.
- 📄 Mao et al., 2026 — *Contiguity, not importance* → Reparo com orçamento das posições locais à edição; também decodificação com KV aproximado.
- 📄 Yang et al., 2026 — *PatchKV* → Recalcula as posições locais à edição; também decodificação com KV aproximado.
- 📄 Anônimo, 2026 — *Speculating on the past: Lossless reuse of stale KV caches via self-speculative verification* (https://openreview.net/forum?id=96mkAG7gh1) → Trabalho concorrente com a mesma ideia central; o artigo diz que o CALLO vai além em streaming e escalonamento.

### Datasets, benchmarks e ferramentas citados

- 🔗 SWE-agent (Yang et al., 2024) → Arcabouço e as configurações de gestão de contexto L5/L10 usadas em todas as cargas de trabalho.
- 🔗 Traço de tráfego de programação da Azure, via Stojkovic et al., 2025 → Níveis de carga e distribuição semanal para a simulação de cluster.
- 🔗 SWE-chat (Baumann et al., 2026, https://arxiv.org/abs/2604.20779) → Fonte das estatísticas de sessões do Claude Code na introdução.

### Leitura externa sugerida (não está no artigo)

Nenhuma adicionada; este resumo usa apenas o texto e a bibliografia do artigo.

---
*Resumo gerado: 2026-11-16*
