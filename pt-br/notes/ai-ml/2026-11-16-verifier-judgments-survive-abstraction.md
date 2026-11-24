---
type: Paper Summary
title: Os julgamentos de verificadores sobrevivem à abstração? Medindo o transporte de verificação para avaliação de agentes LLM
description: Julgamentos de verificadores corretos em trajetórias completas de agentes mudam em cerca de 20–31% dos casos quando o rastro é abstraído, e restaurar a proveniência recupera 85,5% deles.
resource: https://arxiv.org/abs/2611.18471
tags:
- ai-ml
- ai-agents
- llm-as-a-judge
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.18471
  title: Do Verifier Judgments Survive Abstraction? Measuring Verification Transport for LLM Agent Evaluation
  author: Disha Sheshanarayana, Sai Linisha Reddy Goli
status: stable
source_url: https://arxiv.org/abs/2611.18471
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-13'
domain: ai-ml
subdomain: ai-agents
concepts:
- llm-as-a-judge
- multi-agent-orchestration
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d81ffbfd4cd7380001ea9
lang: pt-br
translation_of: /notes/ai-ml/2026-11-16-verifier-judgments-survive-abstraction.md
---

# Os julgamentos de verificadores sobrevivem à abstração? Medindo o transporte de verificação para avaliação de agentes LLM

**Fonte:** https://arxiv.org/abs/2611.18471
**Autores:** Disha Sheshanarayana, Sai Linisha Reddy Goli (contribuição igual)
**Instituição:** Manipal University Jaipur
**Publicado:** 13 nov 2026 (arXiv v1) | **Veículo:** Preprint; artigo de workshop, "Who Verifies the Agents? Toward Reliable Agent Development", NeurIPS 2026
**Tags:** #agent-evaluation #llm-as-a-judge #verification #observability

---

## 📄 Resumo (em linguagem simples)
Verificadores LLM são usados para avaliar as trajetórias de agentes, mas a confiabilidade deles costuma ser medida em uma única visão fixa da trajetória. Em sistemas em produção, a mesma execução pode chegar ao verificador por meio de ferramentas compostas, sub-agentes delegados ou um histórico compactado, então ele enxerga menos das evidências subjacentes. Os autores verificam se um veredicto que está correto na visão completa continua correto quando a mesma execução congelada é abstraída. Em quatro benchmarks, sete backbones, quatro protocolos e três fronteiras de abstração, de 20% a 31% dos veredictos corretos na visão nativa falham, e a maioria das falhas se recupera quando a proveniência original é restaurada.

## 🎯 Contribuição central
O artigo define o **Transporte de Verificação** como um critério de confiabilidade: um julgamento correto para uma execução deve continuar correto quando apenas a representação dessa execução visível ao verificador muda. Trabalhos anteriores sobre a confiabilidade do [LLM-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md) variam sobretudo a política, o prompt ou o contexto, e assumem que a trajetória é mostrada em uma forma fixa. Os autores mantêm fixos a trajetória, o resultado, o ground truth, o critério e o verificador, e variam apenas a observabilidade. A afirmação deles é que a acurácia apenas na visão nativa não consegue caracterizar a confiabilidade de um verificador, porque um verificador pode ser preciso na visão completa e ainda falhar quando as evidências são abstraídas.

## 🏗️ Metodologia

### Formulação do problema
Para uma trajetória T com rótulo Y, um verificador V recebe uma visão O_c(T) sob a condição c e devolve um veredicto ou nenhum veredicto utilizável (⊥). Um julgamento correto na visão nativa (C0) falha no transporte se o seu veredicto abstraído (Ca) estiver errado ou for ⊥. Veredictos errados e ⊥ são reportados separadamente, porque ⊥ é uma perda de cobertura, não um julgamento incorreto.

### Método proposto: arcabouço de abstração e restauração
Cada execução recebe três visões pareadas, C0 → Ca → Ca-R. A condição de restauração Ca-R reinsere a proveniência original, então apenas a evidência do verificador muda. Três fronteiras são instanciadas:

- **Abstração macro (CM):** operações de baixo nível ficam atrás de uma ação macro de nível mais alto. A restauração reinsere as operações internas exatas.
- **Delegação (CD):** parte da execução fica oculta atrás de uma fronteira de sub-agente, e o verificador vê apenas o resultado delegado. A restauração reinsere a trajetória delegada.
- **Compactação do histórico (CC):** o histórico anterior é substituído por um marcador, mantendo as evidências recentes, incluindo o par final chamada-de-ferramenta/resultado. A restauração reinsere os eventos originais.

### Protocolos de verificação
Todos os protocolos compartilham o backbone, o critério e a representação, e diferem apenas na forma como usam as evidências:

- **V-LLM:** veredicto direto em uma única passagem.
- **V-EVIDENCE:** nomeia as evidências visíveis e verifica primeiro a sua suficiência.
- **V-AGENTIC:** pode solicitar mais proveniência da mesma trajetória.
- **V-DELIBERATIVE:** reutiliza o veredicto do V-EVIDENCE e depois executa uma auditoria de evidências separada, sem proveniência extra. Isso isola a deliberação da aquisição de proveniência.

### Escolhas de design
As visões são construídas de forma determinística a partir de trajetórias congeladas antes de qualquer chamada ao verificador, com temperature 0 e saídas estruturadas, de modo que apenas a visibilidade das evidências varia. As taxas agregadas somam numeradores e denominadores entre as células, em vez de fazer a média de percentuais.

## 🧪 Configuração experimental

### Datasets e benchmarks
- **AgentProcessBench (APB), 250 trajetórias:** qualidade de processo em nível de passo [Fan et al., 2026].
- **ATBench, 758 trajetórias:** segurança de trajetórias de uso de ferramentas [Li et al., 2026c].
- **PlanRewardBench (PRB), 441 comparações em pares:** preferência em nível de trajetória [Wang et al., 2026].
- **AgentDojo, 754 trajetórias:** agentes interativos sob prompt injection [Debenedetti et al., 2024].

As contagens são as instâncias que atenderam às regras de elegibilidade de cada benchmark para a construção pareada.

### Baselines
Não há baselines no sentido usual. Os eixos de comparação são a visão nativa, as três fronteiras, as três condições de restauração e os quatro protocolos. São usados sete backbones: Gemini-3.7-Flash, Gemini-2.5-Flash, GPT-OSS-120B, GPT-OSS-20B, Gemma-4-31B, Gemma-4-26B-A4B e Llama-4-Maverick.

### Métricas de avaliação
- **C0 OBA:** acurácia balanceada operacional na visão nativa.
- **VTFR:** parcela dos julgamentos corretos na visão nativa que falham após a abstração. Mede a estabilidade dos julgamentos corretos, não a acurácia geral.
- **RRR:** parcela das falhas induzidas pela abstração que é recuperada pela restauração.
- **RDR:** parcela dos julgamentos que sobreviveram à abstração, mas são degradados pela restauração.

## 📊 Resultados

### Resultados principais
Ponderados pelo denominador em todos os benchmarks, modelos e protocolos:

| Fronteira | VTFR | RRR | RDR |
|---|---|---|---|
| Abstração macro | 31,0% | 87,9% | 5,3% |
| Delegação | 30,9% | 86,9% | 5,4% |
| Compactação do histórico | 20,3% | 79,8% | 4,9% |
| **Todas as fronteiras** | | **85,5%** | **5,2%** |

VTFR por protocolo (macro / delegação / compactação):

| Protocolo | Macro | Delegação | Compactação |
|---|---|---|---|
| V-LLM | 16,4% | 17,3% | 14,1% |
| V-AGENTIC | 25,0% | 23,6% | 18,2% |
| V-EVIDENCE | 34,5% | 34,0% | 18,7% |
| V-DELIBERATIVE | 49,3% | 50,2% | 30,7% |

A ordem dos protocolos diz respeito a modos de falha, não apenas a totais. Sob abstração macro, o V-LLM gera 15,8% de veredictos errados e 0,7% de não-veredictos; o V-EVIDENCE gera 10,6% e 24,0%; e o V-DELIBERATIVE gera 9,4% e 39,9%. Um VTFR mais baixo pode, portanto, esconder cobertura de veredictos perdida. O efeito se mantém entre os backbones, mas o seu tamanho varia (Tabelas 1–2, Figura 3).

### Estudos de ablação
- **Severidade:** ocultar 25%, 50% e 75% das evidências eleva o VTFR nos extremos. No APB, o Gemini vai de 24,3% para 31,4% e o GPT-OSS de 21,5% para 29,6%. No ATBench, os valores vão de 17,7% para 21,1% e de 16,9% para 22,9%.
- **Fidelidade da proveniência:** a recuperação segue Requery < LossyR < ExactR. No APB, o RRR do Gemini é de 22,5%, 42,8% e depois 80,0%, e o do GPT-OSS é de 24,6%, 34,0% e depois 82,5%.
- Reavaliar sobre as mesmas evidências abstraídas recupera o menos, então a recuperação depende das evidências, não de outra chamada ao verificador.

## 💡 Principais insights
- A acurácia na visão nativa superestima a confiabilidade do verificador. Um veredicto na visão completa muda em cerca de 20–31% dos casos, dependendo da fronteira.
- A abstenção é um modo de falha separado. Protocolos que buscam evidências perdem sobretudo cobertura sob abstração macro, em vez de darem respostas erradas.
- A maioria das falhas é recuperável. A proveniência exata recupera 85,5% no total, enquanto a proveniência com perdas recupera bem menos (APB Gemini: 42,8% contra 80,0%), então o acesso às evidências é um gargalo maior do que a capacidade do verificador.
- Deliberação extra não ajuda aqui. O V-DELIBERATIVE tem o maior VTFR, com 49,3% e 50,2% sob abstração macro e delegação.

## 🔍 Referências de acompanhamento

### Trabalhos fundamentais em que este artigo se apoia
- 📚 Zheng et al., 2025 — *ProcessBench: Identifying process errors in mathematical reasoning* → Localização de erros em nível de processo, que motiva a verificação em nível de trajetória.
- 📚 Lightman et al., 2023 — citado para a verificação em nível de passo do raciocínio. O título não é reproduzido aqui.

### Métodos concorrentes ou relacionados citados diretamente
- 📄 Fan et al., 2026 — *AgentProcessBench* ([arXiv:2603.14465](https://arxiv.org/abs/2603.14465)) → Um dos quatro benchmarks (250 trajetórias).
- 📄 Weng et al., 2026 — *Beyond accuracy: Policy invariance as a reliability test for LLM safety judges* ([arXiv:2605.06161](https://arxiv.org/abs/2605.06161)) → O mais próximo em espírito; testa a estabilidade do juiz sob transformações de política, e não a visibilidade das evidências.
- 📄 Zhuge et al., 2025 — *Agent-as-a-Judge: Evaluate agents with agents* (ICML 2025) → Avaliação agêntica, o cenário que o V-AGENTIC se assemelha.
- 📄 Han et al., 2026 — *SkillTV-Bench* ([arXiv:2608.05573](https://arxiv.org/abs/2608.05573)) → Avaliação de juízes sobre execuções de agentes, um vizinho próximo em cenário.

### Datasets, benchmarks ou ferramentas citados
- 🔗 ATBench — [arXiv:2604.02022](https://arxiv.org/abs/2604.02022) → Benchmark de segurança de trajetórias; 758 trajetórias usadas.
- 🔗 PlanRewardBench — *Aligning agents via planning: A benchmark for trajectory-level reward modeling* (ACL 2026; Wang et al., 2026) → 441 comparações em pares.
- 🔗 AgentDojo — [OpenReview](https://openreview.net/forum?id=m1YYAQjO3w) → Ambiente de prompt injection; 754 trajetórias usadas.

### Leitura externa sugerida (não está no artigo)
Nenhuma proposta. O Apêndice A (trabalhos relacionados estendidos) e o Apêndice G (limitações) são as referências mais próximas dentro da própria fonte.

## ⚠️ Limitações reconhecidas pelos autores
O texto principal não tem uma seção de limitações. O Apêndice G afirma que apenas três fronteiras controladas são estudadas, que as visões vêm de trajetórias congeladas e não de abstrações ao vivo, e que a restauração pressupõe que a proveniência original pode ser reinserida, com disponibilidade e fidelidade que podem variar. Ele também observa que o VTFR se condiciona aos julgamentos corretos na visão nativa, então mede a preservação da verificação correta, não a competência geral. Os resultados podem não se transferir para outros domínios ou desenhos de verificadores.

---
*Resumo gerado: 2026-11-16*
