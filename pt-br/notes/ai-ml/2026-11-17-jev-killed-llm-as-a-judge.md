---
type: Article Summary
title: O Jev matou o LLM-as-a-Judge?
description: Juízes LLM carregam vieses e pontuações não calibradas, então as faixas de avaliação binária devem migrar para modelos de decisão calibrados após validação.
resource: https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
tags:
- ai-ml
- llm-engineering
- llm-as-a-judge
- rag
- calibration
- position-bias
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
  title: Jev killed LLM-as-a-Judge?
  author: Sakshi Jaiswal
status: stable
source_url: https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
source: Women in Technology
source_type: medium
published: '2026-09-23'
domain: ai-ml
subdomain: llm-engineering
concepts:
- llm-as-a-judge
- rag
- calibration
- position-bias
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d8125a083ddaf118c8887
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-jev-killed-llm-as-a-judge.md
---

# O Jev matou o LLM-as-a-Judge?

**Fonte:** https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
**Autor:** Sakshi Jaiswal
**Data:** 2026-09-23
**Conceitos:** [LLM-as-a-judge](/pt-br/concepts/llm-as-a-judge.md) · [Geração aumentada por recuperação](/pt-br/concepts/rag.md) · [Calibração](/pt-br/concepts/calibration.md) · [Viés de posição](/pt-br/concepts/position-bias.md)

---

## 🎯 Tese central
Faixas de avaliação binária devem migrar de juízes LLM para modelos de decisão calibrados, como o Jev, depois de validados com os seus próprios dados, porque os veredictos dos juízes variam com o tempo e as notas dos juízes não são probabilidades.

## 🧭 Por que o autor escreveu o artigo
O autor rodou duas vezes o mesmo benchmark de RAG com 500 perguntas, com 24 horas de intervalo, com o mesmo modelo e o mesmo prompt, e 43 veredictos mudaram, não porque as respostas mudaram, mas porque o juiz mudou de ideia. O artigo trata isso como um problema estrutural, e não como um problema de teste, e afirma que o Jev, lançado pela TypeSafe AI em setembro de 2026, muda as opções disponíveis. Se essa tese se sustentar, as equipes devem manter os juízes LLM para avaliações abertas e que exigem justificativas, e migrar verificações binárias, como fidelidade, segurança, roteamento e guardrails, para um modelo que devolve probabilidades calibradas.

## O que é o LLM-as-a-Judge (e por que as equipes adotaram tão rápido)
O padrão é simples. Em vez de pagar anotadores humanos para avaliar as saídas de um modelo, você envia essas saídas a um LLM capaz, com uma rubrica, e pede que ele atribua uma nota ou compare as respostas. A técnica se espalhou por motivos operacionais, e o atrativo é em parte arquitetural, porque um juiz consegue avaliar qualidades que as métricas antigas deixam passar.

### LLM-as-a-Judge
O artigo original do MT-Bench, de Zheng et al. (2023), testou essa ideia com números concretos. Ele avaliou 80 perguntas de múltiplos turnos em escrita, raciocínio, matemática e código, usou o GPT-4 como juiz e mediu a concordância com pares de especialistas humanos. A concordância entre o GPT-4 e os humanos ficou na mesma faixa da concordância entre dois humanos. A comparação prática do artigo torna a economia concreta: três avaliadores, 200 exemplos e uma rubrica com múltiplos critérios são uma semana de trabalho, enquanto um juiz LLM roda a mesma avaliação em 12 minutos por US$ 4. Para equipes que iteram rápido sobre prompts ou fine-tunings, essa proporção é difícil de recusar.

### Avaliação em pares e avaliação pontual
Os juízes costumam trabalhar em um de dois modos. Na avaliação em pares, o juiz compara duas respostas candidatas e escolhe a preferida. Na avaliação pontual, ele atribui uma nota numérica a uma única saída. O artigo diz que ambos os modos são hoje onipresentes em sistemas de IA em produção.

### BLEU e ROUGE
São as métricas que os juízes deveriam substituir. O artigo diz que elas não capturam coerência, precisão factual e relevância. Um juiz baseado em modelo de linguagem pode ler uma resposta como um usuário leria, perceber que ela está tecnicamente correta mas não ajuda, e sinalizar isso. O artigo chama isso de capacidade genuína e diz que o problema está no que vem junto com ela.

> 🔎 Contexto adicional: BLEU e ROUGE medem a sobreposição de n-gramas entre o texto gerado e o texto de referência. Elas recompensam palavras coincidentes, não significado, e por isso deixam passar as qualidades que o artigo lista.

### Verificação de fidelidade como juiz pontual
O artigo apresenta um juiz pontual mínimo que verifica se uma resposta de RAG é fiel ao contexto recuperado. Ele pede ao modelo que liste cada afirmação, encontre a frase de suporte e marque a resposta como não fiel se alguma afirmação não tiver sustentação. O veredicto é convertido em um modelo Pydantic tipado, e a chamada fixa um snapshot do modelo com temperature 0.0.

```py
# Minimal pointwise judge — faithfulness check for a RAG response
from openai import OpenAI
from pydantic import BaseModel

client = OpenAI()

JUDGE_PROMPT = '''
You are evaluating whether an assistant response is faithful to a retrieved context.
Faithful means every factual claim is explicitly supported by the context.
Steps:
1. List every factual claim in the response.
2. Locate the exact context sentence supporting each claim.
3. If any claim has no support, respond unfaithful.

Context: {context}
Response: {response}
'''

class Verdict(BaseModel):
    faithful: bool
    reason: str

def judge_faithfulness(context: str, response: str) -> Verdict:
    result = client.beta.chat.completions.parse(
        model='gpt-4o-2024-11-20',
        messages=[{'role': 'user', 'content': JUDGE_PROMPT.format(
            context=context, response=response)}],
        response_format=Verdict,
        temperature=0.0,
    )
    return result.choices[0].message.parsed
```

**O que isto faz:**
- `JUDGE_PROMPT` faz o juiz listar as afirmações e localizar o suporte antes de dar o veredicto, então o veredicto se apoia em etapas explícitas de verificação.
- `Verdict` dá à saída um `faithful: bool` tipado mais um `reason: str`, então o código a jusante pode decidir com base em um booleano e ainda ler uma justificativa.
- `beta.chat.completions.parse` com `response_format=Verdict` devolve um objeto parseado, não um texto livre que precise ser extraído à força.
- `model='gpt-4o-2024-11-20'` fixa um snapshot com data, então o juiz não muda silenciosamente entre execuções. Isso importa para o problema dos 43 veredictos invertidos da história de abertura.
- `temperature=0.0` remove a aleatoriedade de amostragem da chamada ao juiz, na medida em que a API permite.

## O catálogo de vieses: o que a pesquisa realmente mostra
O resultado de concordância do MT-Bench é real, mas depende de condições que a maioria dos pipelines de produção não mantém por completo. A pesquisa de 2024 e 2025 produziu uma taxonomia de falhas. As três mais documentadas de forma consistente são o viés de posição, o viés de verbosidade e o viés de autopreferência, e elas interagem de maneiras que tornam os pipelines silenciosamente pouco confiáveis.

### Viés de posição
Viés de posição significa que o veredicto do juiz muda quando você troca qual resposta aparece primeiro. Um estudo com 15 juízes e cerca de 150.000 instâncias de avaliação constatou que, ao trocar as posições, os juízes ou invertem o veredicto (instabilidade de repetição) ou o mantêm (estabilidade de repetição). A consequência prática é que, em uma avaliação em pares, rodar apenas a ordem A-B significa medir a preferência do juiz tanto quanto a qualidade da resposta.

> ❌ **Anti-padrão — avaliação em pares com uma única ordem:** Cada par é julgado uma vez, como A e depois B, e o vencedor é registrado. **Por que as pessoas fazem isso:** rodar uma ordem só reduz pela metade as chamadas ao juiz, o que o torna o padrão barato. **Consequência:** o resultado mistura a qualidade da resposta com a preferência de posição do juiz, e o erro não aparece na saída. **Correção:** rodar as duas ordens, contar com que frequência os veredictos invertem e acompanhar essa taxa de inversão ao longo do tempo.

O segundo bloco de código do artigo implementa essa correção.

```py
# Cheap position-bias check: run both orderings, compare verdicts
def check_position_bias(judge_fn, prompt, response_a, response_b):
    verdict_ab = judge_fn(prompt, response_a, response_b)  # A first
    verdict_ba = judge_fn(prompt, response_b, response_a)  # B first
    flipped = verdict_ab['winner'] != swap_winner(verdict_ba['winner'])
    return {'flipped': flipped, 'ab': verdict_ab, 'ba': verdict_ba}

def swap_winner(winner):
    return 'B' if winner == 'A' else 'A'

# Run across a sample and report the flip rate
flips = [check_position_bias(my_judge, p, a, b) for p, a, b in eval_set]
flip_rate = sum(f['flipped'] for f in flips) / len(flips)
print(f'Position bias flip rate: {flip_rate:.1%}')
```

**O que isto faz:**
- Chama o juiz duas vezes, uma com A primeiro e outra com B primeiro, de modo que cada par é julgado nas duas ordens.
- `swap_winner` converte o vencedor da segunda rodada de volta para a rotulação original antes de comparar os dois veredictos.
- `flipped` é True quando as duas ordens discordam. Essa discordância é o sinal de viés de posição.
- `flip_rate` agrega sobre `eval_set`, e o percentual impresso é o número a acompanhar ao longo do tempo.

### Viés de verbosidade
Viés de verbosidade é a tendência de favorecer respostas mais longas, quer o tamanho extra agregue valor ou não. O artigo relata que a maioria dos modelos apresenta um viés positivo de verbosidade. Em um estudo sistemático, o Llama 3.3–70B e o Gemini 2.5 Pro ficaram com notas fortemente positivas, enquanto o Claude Sonnet 4 foi o único a mostrar uma leve preferência por concisão.

### Viés de autopreferência
Viés de autopreferência significa que um modelo tende a avaliar melhor as próprias saídas. O artigo o chama de o mais insidioso em sistemas agênticos. Em um ciclo de treinamento automatizado, ele não é apenas ruído, porque o favoritismo do juiz vira o sinal de feedback que molda o próximo modelo.

> ❌ **Anti-padrão — juiz da mesma família em um ciclo automatizado:** O juiz vem da mesma família de modelos do gerador que ele avalia. **Por que as pessoas fazem isso:** a API de um provedor já está na stack, então é o juiz mais fácil de conectar. **Consequência:** o viés de autopreferência realimenta o treinamento ou a seleção, e o sinal se desloca para o estilo do próprio gerador. **Correção:** usar um juiz de outra família de modelos e tratá-lo como um controle entre vários.

### Viés de autoridade, omissão de falácias e leniência
Os pesquisadores também catalogaram três outras falhas. O viés de autoridade favorece respostas que citam fontes, mesmo fabricadas. A omissão de falácias significa que erros lógicos em cadeias de raciocínio passam despercebidos. A leniência significa que os juízes aprovam demais, o que esconde regressões.

> ❌ **Anti-padrão — confiar no painel verde de um juiz leniente:** O juiz aprova a maioria das saídas, e o painel parece saudável. **Por que as pessoas fazem isso:** um painel verde é o sinal que as equipes estão acompanhando. **Consequência:** a regressão que a avaliação foi criada para detectar passa despercebida. **Correção:** comparar a taxa de aprovação do juiz com uma amostra rotulada, incluindo saídas sabidamente ruins, antes de confiar na linha de tendência.

### Taxas de falha medidas e mitigações
O CALM, framework da IBM Research, identificou 12 tipos distintos de viés em modelos populares. O JudgeBiasBench constatou que modelos de fronteira chegam a mais de 50% de erro em testes avançados de viés. O artigo diz que, mesmo com todas as mitigações aplicadas (embaralhar a ordem, usar um juiz de outra família de modelos e restrições explícitas na rubrica), os vieses não podem ser eliminados por completo.

## A lacuna de calibração: por que uma nota não é uma probabilidade
O artigo afirma que o número que um juiz devolve não é uma probabilidade. Quando um juiz devolve "4 de 5" ou "PASS", o veredicto não carrega nenhum sinal de confiança calibrado. Você não consegue usar um limiar sobre ele, roteá-lo ou decidir quando escalar para um revisor humano. A expressão do artigo é que é "um rótulo vestido de número".

### Calibração
> 🔎 Contexto adicional: Um modelo está bem calibrado quando sua confiança declarada corresponde à frequência com que ele acerta. Um 0,8 bem calibrado está certo cerca de 80% das vezes.

Limiares, caminhos de escalonamento e monitores de drift precisam de uma nota probabilística. Um painel de avaliação pode parecer verde enquanto esconde uma taxa de falha real, porque o juiz não tem como dizer "não tenho certeza sobre este caso".

### Descalibração no JudgeBench
Um estudo de 2025 com 14 modelos no JudgeBench constatou que os juízes concentravam suas previsões entre 90% e 100% de confiança, enquanto a acurácia ficava bem abaixo disso. Alta confiança declarada combinada com acurácia menor é o que significa descalibração. O artigo argumenta que isso importa mais em sistemas agênticos, onde a avaliação decide se um resultado será repetido, escalado ou aprovado. Ali, um juiz descalibrado é imprevisível na direção errada, e os sistemas que mais precisam de sinais confiáveis são justamente aqueles em que a descalibração custa mais.

> ❌ **Anti-padrão — aplicar limiar sobre a nota bruta de um juiz:** O pipeline roteia ou escala quando a nota do juiz cruza uma linha, como "4 ou mais passa". **Por que as pessoas fazem isso:** uma nota parece uma medição, e é fácil de ler. **Consequência:** o corte não tem nenhuma confiança declarada por trás, então erros de roteamento acontecem em silêncio na fronteira. **Correção:** derivar uma probabilidade, ou medir a concordância, antes de definir qualquer limiar.

### Mitigações parciais e seu custo
O artigo lista duas correções parciais. Uma amostra várias respostas do juiz e calcula a concordância empírica. A outra usa um painel de juízes diversos e trata a discordância como um sinal de baixa confiança. O ponto do artigo é que as duas multiplicam substancialmente o custo de inferência.

## O que é o Jev e o que ele não é
A TypeSafe AI lançou o Jev em 18 de setembro de 2026. O artigo enfatiza que o Jev é um modelo de decisão, não um modelo de chat e não um LLM menor. Um LLM generativo prevê tokens e pode produzir uma string aberta. Modos de saída estruturada restringem essa saída a um esquema, mas o modelo ainda roda por uma interface de modelo de linguagem. O Jev, em vez disso, parte de um espaço de respostas fechado. Você define as opções, e ele devolve o valor selecionado mais a distribuição de probabilidade completa sobre os resultados permitidos. Ele não escreve nenhuma explicação.

### Modelo de decisão e espaço de respostas fechado
No Jev, você define as opções de antemão, e o modelo escolhe entre elas. A saída é uma escolha e uma distribuição, não prosa. Essa é uma forma diferente de um juiz que escreve um veredicto e uma justificativa.

### Choice, Score e Noul
O Jev expõe três primitivas de pergunta. Choice seleciona entre opções nomeadas que você fornece. Score devolve um valor em uma rubrica definida pelo desenvolvedor, com 2 a 10 degraus. Noul é um sim-ou-não probabilístico. Várias perguntas sobre o mesmo estado de entrada são avaliadas em paralelo em uma única chamada de API.

### RLCD
A TypeSafe chama seu objetivo de treinamento de Reinforcement Learning for Calibrated Decisions (RLCD). Ele mira qualidade de decisão e estimativas de probabilidade úteis, e não respostas conversacionais preferidas. O artigo diz que se trata de um objetivo genuinamente diferente do RLHF padrão.

### Acesso via OpenRouter e custo
No lançamento, o Jev está disponível no OpenRouter como `typesafe/jev-latest`, pelo endpoint `/api/alpha/decisions` do OpenRouter. Custa US$ 0,042 por milhão de tokens de entrada e US$ 0,00 por milhão de tokens de saída, porque não produz tokens de saída. A TypeSafe afirma ser até 200 vezes mais rápida e 400 vezes mais barata que LLMs de fronteira em tarefas de classificação comparáveis. Um teste independente constatou que um modelo de uso geral gastou cerca de 910 tokens de saída raciocinando para cada resposta sim-ou-não, enquanto o Jev gastou 85 e não cobra por eles.

> ❌ **Anti-padrão — modelo generativo com raciocínio pesado para verificações sim/não:** Um modelo de uso geral escreve uma cadeia de raciocínio para responder a uma pergunta binária, como fidelidade. **Por que as pessoas fazem isso:** é a ferramenta que já está na stack, e um único modelo cobre todas as verificações. **Consequência:** o teste do artigo mostra cerca de 910 tokens de saída por resposta, cobrados em cada chamada, para produzir um resultado binário. **Correção:** rotear as faixas binárias para um modelo de espaço fechado onde essa troca compensa, e manter os juízes generativos para trabalhos que exigem justificativas.

### Chamando o Jev pelo experimental_evaluate do AI SDK
O terceiro bloco de código do artigo mostra a chamada. O SDK em Python não estava disponível no lançamento, então esta forma em TypeScript é a única rota que o artigo oferece.

```ts
// Jev call via AI SDK 7+ (experimental_evaluate)
// Note: Python SDK not yet available at launch — verify current docs
import { experimental_evaluate } from 'ai';

const result = await experimental_evaluate({
  model: 'typesafe/jev-latest',
  state: `User asked: ${userQuery}\nAssistant replied: ${assistantResponse}`,
  questions: [
    {
      type: 'noul',  // probabilistic yes/no
      question: 'Is every factual claim in the assistant reply directly supported by the retrieved context?',
    },
    {
      type: 'score',
      question: 'How helpful is this response?',
      scale: { min: 1, max: 5, labels: { 1: 'Not helpful', 5: 'Highly helpful' } },
    },
  ],
});

// result.answers[0].value -> probability (0.0 to 1.0)
// result.answers[1].value -> score (1 to 5)
// result.answers[1].distribution -> full probability over {1,2,3,4,5}
console.log('Faithful?', result.answers[0].value > 0.85);
console.log('Helpfulness score:', result.answers[1].value);
```

**O que isto faz:**
- Envia uma string `state` e duas perguntas juntas, então as duas respostas voltam em uma única chamada.
- A pergunta `noul` devolve uma probabilidade em `answers[0].value`. O código a compara com 0,85, que é o corte ilustrativo do autor e não um limiar validado.
- A pergunta `score` devolve um valor em uma escala de 1 a 5, mais uma `distribution` sobre os degraus. Essa distribuição é a informação de calibração que uma nota isolada não tem.
- O comentário diz que o SDK em Python ainda não estava disponível no lançamento, então o exemplo é apenas em TypeScript.

## Onde o Jev fica aquém: o balanço honesto
O artigo é direto sobre os limites. Ele os trata como restrições de design com consequências reais, e não como lacunas menores.

### Sem justificativa
O Jev não consegue escrever uma frase. Se uma avaliação precisa explicar por que uma resposta falhou, para que um engenheiro consiga corrigir o prompt ou o modelo, o Jev não fornece isso. Você recebe o veredicto e a distribuição de probabilidade, e o sinal diagnóstico que um juiz LLM bem pensado oferece, mesmo com seus vieses, está ausente. Equipes que usam justificativas do juiz para rotular dados de treinamento, ou para montar fluxos de depuração baseados em rubrica, enfrentam uma lacuna real.

### Janela de contexto
A janela de contexto é de 32 mil a 64 mil tokens, dependendo da rota de acesso. O artigo diz que isso basta para a maioria das tarefas de avaliação de turno único, mas limitará sessões agênticas longas e avaliações em nível de documento.

### Acesso antecipado e o endpoint alpha
O acesso ainda era antecipado em 20 de setembro de 2026. A superfície de API no lançamento é apenas JavaScript, pelo `experimental_evaluate` do AI SDK 7+, sem rota HTTP estável para equipes em Python. O artigo observa que o endpoint `/api/alpha/decisions` está marcado como alpha por um motivo.

### Afirmações não replicadas e evidência de calibração restrita
Os números de 200 vezes de velocidade e 400 vezes de custo da TypeSafe vêm de seus próprios benchmarks. O artigo diz que a replicação independente em escala, sobre distribuições reais de tarefas, não tinha sido publicada até a data em que foi escrito. Os resultados de calibração publicados (e-mails com nota abaixo de 0,1 pelo Jev eram 0,1% spam, e os acima de 0,9 eram 99,9% spam) vêm de uma tarefa restrita de classificação de spam, não de avaliação de qualidade de respostas abertas.

### Limite de capacidade e rubricas fechadas
A preocupação mais aguda do artigo é que as tarefas em que os juízes LLM são mais poderosos são justamente aquelas em que os critérios são difíceis de especificar de antemão, como utilidade nuançada, adequação de tom e qualidade criativa. O espaço de respostas fechado do Jev significa que você precisa especificar tudo logo no início.

> ❌ **Anti-padrão — confiar em uma nota barata sobre uma rubrica não verificada:** A rubrica é escrita uma vez, e as notas do Jev são usadas como verdade. **Por que as pessoas fazem isso:** o custo e a velocidade tornam a pontuação em alto volume segura de automatizar. **Consequência:** se a rubrica estiver errada, o Jev pontua com confiança e de forma barata contra o padrão errado. O artigo diz que um juiz LLM descalibrado falha de maneiras visíveis, enquanto um Jev descalibrado "pode falhar em silêncio e em escala". **Correção:** validar o conjunto de opções e a rubrica contra uma amostra rotulada da sua distribuição real de tarefas antes de pontuar em volume.

## Um caminho prático de migração: rodar os dois em modo sombra
O artigo recomenda uma rota cautelosa. Mantenha o juiz atual, rode o Jev ao lado dele, registre os dois veredictos por avaliação, nunca aja sobre a saída do Jev e meça a concordância.

### Modo sombra
> 🔎 Contexto adicional: O modo sombra roda um sistema candidato ao lado do sistema em produção sem deixar que as saídas dele afetem nada. Só os resultados do sistema em produção são usados, e os resultados do candidato são registrados para comparação.

Depois de algumas centenas de avaliações, diz o artigo, você terá um sinal real. Pergunte onde os dois juízes concordam, onde divergem e se as divergências são ruído aleatório ou sistemáticas. Por exemplo, o Jev falha de forma consistente com um certo tamanho de resposta ou tipo de consulta? O artigo argumenta que essa comparação, rodada sobre a sua própria distribuição de tarefas, vale mais do que qualquer benchmark publicado.

> ❌ **Anti-padrão — agir sobre o Jev antes da verificação de calibração:** Os veredictos do Jev entram direto no pipeline porque os números do fornecedor parecem bons. **Por que as pessoas fazem isso:** o custo e a velocidade tornam a troca atraente. **Consequência:** o ponto do artigo é que um veredicto do Jev com 82% de confiança que se revela errado continua sendo um veredicto errado, e agora acontece mais rápido e mais barato. **Correção:** rodar primeiro o modo sombra, medir a concordância e só então agir sobre a saída.

### Portão de limiar de confiança
Uma vez medida a concordância, o próximo passo é um limiar de probabilidade. Acima dele, o veredicto do Jev vai direto para o pipeline. Abaixo dele, o caso volta para o juiz LLM ou para revisão humana. O teste de spam do artigo mostra a forma. Entre as entradas que o Jev pontuou entre 0,5 e 0,6, apenas 38% eram realmente positivas, então essa faixa intermediária é exatamente onde a revisão humana é necessária. Encaminhar apenas esses casos incertos a um humano deixou o restante do conjunto de avaliação com 99,5% de acurácia. O artigo diz que o limiar e o fallback variarão conforme a tarefa, mas o padrão se aplica.

O artigo faz mais uma ressalva. Quando o agente e o juiz vêm do mesmo provedor, o viés de autopreferência é um risco, e um juiz leniente esconde as regressões que ele foi criado para detectar. Mesmo uma migração parcial, limitada às faixas de classificação binária, vale a pena ser testada, porque a economia de custo financia mais cobertura.

### Arcabouço do modo sombra
O quarto bloco de código do artigo é um arcabouço para esse padrão.

```py
# Shadow-mode harness: run both judges, log agreement, never act on Jev
import json
from dataclasses import dataclass, asdict

@dataclass
class ShadowResult:
    input_id: str
    llm_judge_verdict: bool
    jev_probability: float   # 0.0 to 1.0 from Jev noul answer
    jev_verdict: bool        # probability > 0.5
    agree: bool

def shadow_eval(input_id, context, response, jev_client, llm_judge_fn):
    # LLM judge (acts on pipeline)
    llm_verdict = llm_judge_fn(context, response)

    # Jev (shadow — logged only, never acted on)
    jev_result = jev_client.evaluate(
        state=f'Context: {context}\nResponse: {response}',
        questions=[{'type': 'noul',
                    'question': 'Is this response faithful to the context?'}]
    )
    jev_prob = jev_result['answers'][0]['value']
    jev_verdict = jev_prob > 0.5

    return ShadowResult(
        input_id=input_id,
        llm_judge_verdict=llm_verdict,
        jev_probability=jev_prob,
        jev_verdict=jev_verdict,
        agree=(llm_verdict == jev_verdict),
    )

# After N runs: compute agreement rate and inspect disagreements
results = [shadow_eval(i, c, r, jev, my_judge) for i, c, r in eval_set]
agreement = sum(r.agree for r in results) / len(results)
print(f'Agreement: {agreement:.1%}')
# Inspect disagreements manually — they tell you where your rubric is ambiguous
disagreements = [r for r in results if not r.agree]
with open('disagreements.jsonl', 'w') as f:
    for d in disagreements:
        f.write(json.dumps(asdict(d)) + '\n')
```

**O que isto faz:**
- O juiz LLM continua conduzindo o pipeline. A saída do Jev é apenas registrada em `ShadowResult`, então a execução em modo sombra não pode mudar nenhuma decisão.
- `jev_verdict = jev_prob > 0.5` transforma a probabilidade do Jev em sim/não apenas para comparação. O limiar é uma conveniência para a contagem de concordância, não um corte calibrado.
- `agree` registra a concordância item a item, e a taxa geral de concordância é o primeiro número a ler.
- As divergências são gravadas em JSONL para revisão manual. O artigo diz que elas mostram onde a rubrica é ambígua, que é a saída mais útil da execução.

## Encerrando
O argumento final do artigo é que o Jev não encerra o LLM-as-a-Judge. Ele encerra a ideia de que o juiz LLM é a única ferramenta para avaliação estruturada. Interpretação nuançada de rubrica, geração de justificativas e julgamento de preferência em pares sobre saídas abertas continuam sendo trabalho de juiz. Decisões tipadas, probabilidades calibradas, ausência de prosa alucinada, zero tokens de saída e uma fração do custo combinam com as faixas binárias: fidelidade, segurança, roteamento e guardrails. A frase do artigo é que o Jev "matou a parte que deveria ter sido um classificador desde o início". Ele também diz que usar um modelo de fronteira para gerar 910 tokens para uma resposta de fidelidade sim-ou-não sempre foi um contorno, e que o Jev torna esse custo visível.

## 🧠 Modelos mentais
### Um rótulo vestido de número
**O modelo:** o "4 de 5" ou o "PASS" de um juiz parece uma medição, mas não carrega confiança calibrada. Ele não pode guiar um limiar, uma rota ou uma escalação.
**Como aplicar:** não aplique limiar, roteamento ou escalação sobre a nota bruta de um juiz. Derive uma probabilidade ou meça a concordância antes.

### Meça o juiz, não apenas a resposta
**O modelo:** os veredictos mudam conforme a ordem dos candidatos, o tamanho da resposta e a família do próprio juiz, então um resultado em pares pode refletir a preferência do juiz tanto quanto a qualidade.
**Como aplicar:** rode as duas ordens nas avaliações em pares, e acompanhe a taxa de inversão e a relação de família entre juiz e gerador junto com as notas.

### Roteie por confiança, escale a faixa intermediária
**O modelo:** veredictos confiantes nos extremos podem ser automatizados. A faixa incerta no meio precisa de um humano, e é ali que os erros se concentram.
**Como aplicar:** defina um limiar de probabilidade, meça a acurácia dentro da faixa intermediária e envie essa faixa para revisão.

### Espaço de respostas fechado significa especificar tudo de antemão
**O modelo:** um modelo de decisão só pode pontuar contra as opções e a rubrica que você definiu. Uma rubrica errada produz veredictos confiantes, baratos e errados.
**Como aplicar:** valide o conjunto de opções e a rubrica contra exemplos rotulados da sua distribuição real de tarefas antes de pontuar em volume.

### Classificadores para verificações binárias, juízes para qualidade aberta
**O modelo:** verificações binárias, como fidelidade, segurança e roteamento, têm forma de classificador. Julgamentos de qualidade nuançados têm forma de juiz.
**Como aplicar:** classifique suas faixas de avaliação pela forma, e mova as que têm forma de classificador para um modelo de decisão calibrado depois de testes em modo sombra.

## 🔑 Principais conclusões
- Em um benchmark de RAG com 500 perguntas rodado duas vezes, com 24 horas de intervalo, com o mesmo modelo e o mesmo prompt, 43 veredictos mudaram porque o juiz mudou, o que o autor chama de problema estrutural.
- Uma avaliação que roda comparações em pares em apenas uma ordem mede a preferência de posição do juiz tanto quanto a qualidade da resposta, e a taxa de inversão entre as duas ordens é o número a acompanhar.
- Um estudo de 2025 com 14 modelos no JudgeBench constatou juízes concentrados entre 90% e 100% de confiança declarada, enquanto a acurácia ficou bem abaixo disso.
- O Jev custa US$ 0,042 por milhão de tokens de entrada e US$ 0,00 por milhão de tokens de saída; a TypeSafe afirma ser 200 vezes mais rápida e 400 vezes mais barata, e o artigo observa que essas afirmações não têm replicação independente.
- Um teste independente constatou que um modelo de uso geral gastou cerca de 910 tokens de saída por resposta sim-ou-não, enquanto o Jev gastou 85 e não cobra nenhum deles.
- A evidência publicada de calibração do Jev vem de uma tarefa restrita de spam, então ela ainda não mostra calibração em qualidade de respostas abertas.
- No teste de spam, as notas do Jev entre 0,5 e 0,6 eram apenas 38% positivas, então encaminhar essa faixa intermediária a humanos manteve o restante do conjunto com 99,5% de acurácia.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Veredictos de avaliação que não conseguem expressar incerteza são um problema de governança tanto quanto de engenharia, porque um painel pode parecer verde enquanto esconde uma taxa de falha. A mesma lógica se aplica aos checks de guardrail em sistemas de IA que você revisa, onde uma probabilidade com uma curva de calibração validada é muito mais fácil de defender do que uma nota. Uma pergunta para levar à sua próxima revisão: quais das suas faixas de avaliação atuais são verificações binárias que poderiam migrar para um modelo calibrado, e que evidência convenceria um auditor de que as probabilidades dele significam o que dizem?

## 📚 Plano de leitura
### A partir do artigo
O texto do artigo não cita URLs. O MT-Bench (Zheng et al., 2023) é citado sem link, então não há nada a acrescentar aqui sem inventar um.

### Para ir além
- Artigo do MT-Bench de Zheng et al. (2023) — a evidência original de LLM-as-a-Judge sobre a concordância do GPT-4 com especialistas humanos.
- JudgeBench (estudo de 2025 com 14 modelos) — o resultado de calibração que mostra alta confiança declarada com acurácia menor.
- JudgeBiasBench — o benchmark por trás da afirmação de que modelos de fronteira superam 50% de erro em testes avançados de viés.
- Framework CALM da IBM Research — o catálogo de 12 tipos de viés em modelos populares.
- Estudo de viés de posição com 15 juízes e cerca de 150.000 instâncias — a evidência de instabilidade de repetição versus estabilidade.
- Material de lançamento do Jev da TypeSafe AI e sua listagem no OpenRouter — verifique a documentação atual do fornecedor e as replicações independentes antes de se apoiar nas afirmações.
