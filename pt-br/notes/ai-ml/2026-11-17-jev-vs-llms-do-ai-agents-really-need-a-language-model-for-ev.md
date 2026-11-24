---
type: Article Summary
title: 'Jev vs LLMs: agentes de IA realmente precisam de um modelo de linguagem para cada decisão?'
description: Agentes devem encaminhar julgamentos rotineiros para modelos de decisão tipados e reservar os LLMs para tarefas que realmente precisam de linguagem gerada.
resource: https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
tags:
- ai-ml
- llm-engineering
- multi-agent-orchestration
- inference-tradeoffs
- llm-as-a-judge
- structured-decision-model
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
  title: 'Jev vs LLMs: Do AI Agents Really Need a Language Model for Every Decision?'
  author: Ravjot Singh
status: stable
source_url: https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
source: Medium
source_type: medium
published: '2026-09-22'
domain: ai-ml
subdomain: llm-engineering
concepts:
- multi-agent-orchestration
- inference-tradeoffs
- llm-as-a-judge
- structured-decision-model
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d811ca5cbf40ffc57c3cd
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-jev-vs-llms-do-ai-agents-really-need-a-language-model-for-ev.md
---

# Jev vs LLMs: agentes de IA realmente precisam de um modelo de linguagem para cada decisão?

**Fonte:** https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
**Autor:** Ravjot Singh
**Data:** 2026-09-22
**Conceitos:** [Orquestração multiagente](/pt-br/concepts/ai-ml/multi-agent-orchestration.md) · [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md) · [LLM-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md) · [Modelo de decisão estruturado](/pt-br/concepts/ai-ml/structured-decision-model.md)

---

## 🎯 Tese central
A maioria das decisões de agentes de IA (qual ferramenta chamar, se um resultado é relevante, se deve tentar de novo, para onde encaminhar) são julgamentos que o software precisa receber como respostas tipadas. Por isso, devem ir para um modelo de decisão especializado e não para um LLM generativo, que deve ser reservado para o trabalho que realmente exige linguagem gerada.

## 🧭 Por que o autor escreveu o artigo
O autor parte de um hábito que virou padrão: cada etapa de roteamento, classificação e avaliação é enviada a um LLM que escreve texto, texto esse que depois é convertido de volta em uma decisão. O artigo pergunta o que muda se a unidade mais útil for a decisão e não a string, e argumenta que a arquitetura dos agentes ficaria mais parecida com software comum, com uma camada de decisão difusa.

## Abertura: o hábito de chamar um LLM para cada escolha
A abertura lista uma sequência de perguntas comuns (responder a um cliente, classificar um chamado, escolher uma ferramenta, checar relevância, decidir uma nova tentativa, avaliar a resposta anterior do LLM) e observa que cada uma delas é respondida chamando um modelo de linguagem. O autor admite que isso funciona, mas pergunta por que um sistema cuja principal força é gerar linguagem deveria ser usado para responder perguntas com um conjunto pequeno de respostas possíveis. O ponto da abertura é a incompatibilidade entre o trabalho e a ferramenta: "Este chamado deve ir para Cobrança, Suporte Técnico ou Fraude?" precisa de um de três rótulos, não de um ensaio.

O autor também enquadra o resto do artigo em torno de uma afirmação sobre o futuro: sistemas podem usar tipos diferentes de inteligência para tarefas diferentes, então a questão não é se o próprio Jev importa, mas se a ideia de arquitetura por trás dele importa.

## Primeiro: o que é exatamente o Jev?
O Jev foi apresentado pela TypeSafe AI em 2026-09-15. A TypeSafe o descreve como o primeiro "System One Model", um modelo criado para tomar decisões rápidas e estruturadas que o software possa consumir diretamente. O autor contrasta a forma usual com a forma em torno da qual o Jev foi projetado, e cita o resumo da TypeSafe: "Decisões, não strings."

```javascript
input → generated text
```

```javascript
state + question → decision + probability
```

**O que isso significa:**
- A primeira linha é o fluxo usual: uma entrada vai para o modelo e sai texto livre, de modo que o software precisa interpretar esse texto.
- A segunda linha é o fluxo do Jev: a entrada é um estado mais uma pergunta tipada, e a saída é uma decisão com uma probabilidade associada.
- A diferença está no tipo de saída, não na entrada, e por isso o autor a chama de arquitetural e não cosmética.

O autor enfatiza que essa é uma diferença de arquitetura, não apenas de formato. Segundo a documentação da TypeSafe, o Jev recebe algum estado (uma mensagem de cliente, o estado de uma aplicação, a trajetória de um agente, um documento recuperado ou dados estruturados) e avalia perguntas tipadas sobre ele.

### Primitivas Choice, Score e Noul
O Jev expõe três primitivas. **Choice** seleciona uma de um conjunto de alternativas predefinidas. **Score** avalia algo segundo uma rubrica ordenada. **Noul** retorna a probabilidade de que uma afirmação seja verdadeira. Várias perguntas podem ser avaliadas em paralelo sobre o mesmo estado.

> 🔎 Contexto adicionado: "Noul" é o nome que o artigo dá a essa primitiva, como aparece no texto. O artigo não explica o termo além disso, então trate-o como o rótulo do artigo para uma pergunta de probabilidade de verdade.

### Perguntas tipadas paralelas sobre um mesmo estado
O exemplo do chamado mostra como isso funciona na prática. O cliente escreve "Minha conta foi cobrada duas vezes esta manhã. Preciso que isso seja resolvido imediatamente." Em vez de pedir a um LLM um objeto JSON com departamento, prioridade, risco de churn e escalonamento, o software faz quatro perguntas tipadas sobre a mesma mensagem: um Choice de departamento, um Score de urgência de 1 a 5, um Noul de escalonamento humano (human_escalation) e um Score de risco de churn de 1 a 5. O software então recebe decisões que pode usar diretamente, sem um parágrafo voltado ao cliente, sem um ensaio de raciocínio, sem engenharia de prompt JSON e sem um parser de saída.

```javascript
State:
"My account was charged twice this morning..."
Questions:
department:
Choice [billing, technical, account, fraud]
urgency:
Score [1...5]
human_escalation:
Noul
churn_risk:
Score [1...5]
```

**O que isso significa:**
- `State:` fornece a entrada uma única vez, então todas as perguntas são respondidas no mesmo contexto.
- `department` usa `Choice` com uma lista fechada, o que elimina a possibilidade de inventar um nome de departamento.
- `urgency` e `churn_risk` usam `Score` em uma escala limitada, então a resposta é uma classificação e não um texto livre.
- `human_escalation` usa `Noul`, um julgamento de sim ou não expresso como probabilidade, que o software pode comparar com um limiar.
- O trabalho do modelo é "simplesmente julgar", na expressão do autor. O controle do fluxo continua com o software.

## Então o Jev é um LLM?
O autor diz que não, no sentido usual. A TypeSafe posiciona o Jev como uma classe de modelo diferente, e não como um LLM de chat menor. Os LLMs tradicionais são otimizados para geração de linguagem: dado um prefixo, eles preveem o próximo token, um após o outro. É isso que os torna flexíveis o bastante para escrever Python, resumir um contrato, gerar SQL ou compor um e-mail.

O autor aponta o custo dessa flexibilidade. Quando o software precisa do seguinte, um gerador de texto extremamente flexível é usado para produzir uma resposta muito restrita.

```javascript
{
  "priority": "high"
}
```

**O que isso significa:**
- O objeto tem um campo, com dois valores permitidos na prática: `priority` e um rótulo como `"high"`.
- O software precisa apenas desse rótulo, então a geração ao redor dele é um custo extra, que é o que o autor aponta.
- O exemplo é a resposta restrita que o autor contrasta com a geração de texto aberta. O Jev faz o trade-off oposto: abre mão da geração livre e se especializa em decisões limitadas.

### Treinamento calibrado RLCD
A TypeSafe diz que construiu uma arquitetura de modelo separada, uma abordagem de amostragem paralela e um método de treinamento que chama de Reinforcement Learning for Calibrated Decisions (RLCD). O autor observa que a TypeSafe não divulgou publicamente tudo sobre os detalhes internos, então as afirmações sobre o funcionamento do Jev devem ser lidas com essa ressalva. A interface, segundo o autor, mostra a filosofia: um LLM tenta determinar o que dizer, enquanto o Jev tenta determinar em que o software deve acreditar sobre uma pergunta.

## A diferença real: geração versus julgamento
O autor rejeita o enquadramento simples "LLM = texto e Jev = JSON". LLMs modernos já oferecem saídas estruturadas (OpenAI, Anthropic, Google e outros), e o tool calling tornou a integração com software muito mais confiável. O argumento do autor é que a saída estruturada muda o formato da resposta de um LLM, mas não necessariamente muda o trabalho para o qual o modelo foi otimizado.

As perguntas de agentes listadas (qual ferramenta chamar, se ela teve êxito, se um documento é relevante, se deve haver nova tentativa, se a requisição foi atendida, se outro agente deve tratá-la, se um resultado é seguro, se é preciso um modelo mais capaz) são decisões, não tarefas de escrita.

### Pipeline de geração versus pipeline de decisão
O autor mostra a implementação usual e a alternativa proposta como dois pipelines.

```javascript
Agent state
     ↓
Prompt LLM
     ↓
Generate tokens
     ↓
Generate structured response
     ↓
Parse response
     ↓
Validate schema
     ↓
Execute decision
```

```javascript
Agent state
     ↓
Decision model
     ↓
Probability / choice / score
     ↓
Execute decision
```

**O que isso significa:**
- O primeiro pipeline tem quatro etapas entre o estado e a ação (gerar, estruturar, interpretar, validar) que só existem porque o modelo produz texto.
- O segundo pipeline elimina essas etapas: a saída do modelo já é uma escolha, uma nota ou uma probabilidade.
- Cada etapa removida é um ponto em que o sistema pode falhar: texto malformado, um schema que o modelo ignorou ou um parser que discorda do modelo.
- O ponto do autor é que o modelo de decisão não torna o agente mais inteligente por si só; ele remove uma camada de tradução que o agente não precisa.

> ❌ **Anti-padrão — gerar tokens para tomar uma decisão:** pedir a um LLM que escreva uma resposta estruturada, e depois interpretar e validar essa resposta, para uma pergunta com um conjunto pequeno de respostas. **Por que as pessoas fazem isso:** o LLM já está na pilha e lida bem com ambiguidade, então vira o padrão para toda etapa. **Consequência:** cada decisão paga o custo de geração completa, interpretação e validação, e as falhas aparecem na fronteira do parsing. **Correção:** encaminhe julgamentos de conjunto fechado para um modelo de decisão ou um classificador, e reserve a geração para textos que o usuário lê.

## Por que isso importa mais para agentes do que para chatbots
Para um chatbot, uma chamada de modelo de dois segundos pode ser aceitável, porque uma pessoa está lendo a resposta de qualquer forma. Para um agente autônomo, o custo é outro. O autor dá o exemplo de uma tarefa que exige 40 etapas, durante as quais o agente pode tomar 12 decisões de seleção de ferramenta, 8 verificações de relevância, 6 decisões de nova tentativa, 5 verificações de segurança, 5 verificações de conclusão e 4 decisões de roteamento. São dezenas de chamadas de modelo para uma única tarefa e, em milhares ou milhões de agentes, o gargalo passa a ser o grande número de pequenas decisões entre as ações, e não a resposta final.

O autor cita a LangChain, que fez uma observação semelhante depois do lançamento do Jev: os loops de agentes usam modelos repetidamente para decidir o que fazer, avaliar resultados e decidir o que acontece em seguida, e mesmo com tool calling e saídas estruturadas essas chamadas extras aumentam a latência e o custo.

### A arquitetura de agentes de hoje
No desenho comum, um LLM decide o que fazer, uma ferramenta é executada, outra chamada de LLM avalia o resultado e então vem outra ação. O autor observa que o LLM está sendo usado ao mesmo tempo como planejador, roteador, classificador, avaliador, verificador, redator, seletor de ferramentas e motor de conversa. O autor considera isso conveniente, mas duvida que seja a arquitetura certa para escala.

```javascript
             ┌──────────────┐
             │     LLM      │
             └──────┬───────┘
                    │
             decide what to do
                    │
                    ▼
                 Tool
                    │
                    ▼
             ┌──────────────┐
             │     LLM      │
             └──────┬───────┘
                    │
             evaluate result
                    │
                    ▼
             another action
```

**O que isso significa:**
- A mesma caixa de LLM aparece duas vezes, uma para escolher a ação e outra para julgar o resultado, e isso é a origem das chamadas extras por etapa.
- As setas mostram que cada julgamento fica no caminho crítico entre duas ações de ferramenta, então sua latência é paga a cada etapa.
- O diagrama não tem um papel de decisão separado; o LLM desempenha o papel de decisão dentro de cada geração.
- É a linha de base que o próximo diagrama do autor altera.

## Uma arquitetura diferente: Jev + LLM
O autor propõe separar as responsabilidades. Uma camada de decisão (Jev) fica entre o estado do agente e as ações: ela escolhe uma ferramenta, encaminha para escalonamento ou verifica se a tarefa está concluída. Só quando é preciso gerar texto um LLM generativo produz a resposta final. O código cuida das regras de negócio determinísticas, e as ferramentas executam as ações.

```javascript
                      USER
                        │
                        ▼
                   Agent State
                        │
                        ▼
                ┌─────────────┐
                │     Jev     │
                │  Decision   │
                │    Layer    │
                └──────┬──────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
      Tool A          Tool B        Escalate
        │              │
        └───────┬──────┘
                ▼
           New State
                │
                ▼
              Jev
                │
         Is task complete?
           /          \
         No            Yes
         │              │
     Continue           ▼
                  Generative LLM
                        │
                        ▼
                  Final Response
```

**O que isso significa:**
- A primeira caixa do Jev é a decisão de roteamento após a requisição do usuário, escolhendo entre ferramentas ou escalonamento.
- A segunda caixa do Jev é uma verificação de conclusão sobre o novo estado, que decide se o fluxo continua ou se é gerada a resposta final.
- O LLM generativo aparece apenas uma vez, no fim, onde o texto de fato é necessário.
- A conclusão do autor é que isso se parece mais com a arquitetura de software tradicional do que com um grande prompt envolvendo um LLM.

## Pense no Jev como um "if difuso"
O modelo mental mais útil do artigo é um if semântico. O código comum lida bem com regras determinísticas:

```py
if transaction_amount > 10000:
    request_review()
```

Muitas decisões reais não têm uma regra limpa:

```py
if customer_message_indicates_fraud:
    send_to_fraud_team()
```

**O que isso significa:**
- O primeiro bloco é uma regra determinística: um limiar numérico sobre um campo que o código pode ler diretamente.
- O segundo bloco é o caso difícil: `customer_message_indicates_fraud` é um julgamento semântico, sem uma implementação óbvia em código comum.
- O autor observa que centenas de regras de palavras-chave ainda seriam frágeis, porque a linguagem é confusa.

Os LLMs lidam bem com essa ambiguidade, e por isso empresas passaram a usá-los como classificadores. A proposta do Jev é manter o julgamento semântico difuso, mas expô-lo por meio de uma interface que se comporta como software.

```py
if fraud_probability > 0.90:
    route_to_fraud()

elif fraud_probability > 0.65:
    request_human_review()
else:
    continue_normal_flow()
```

**O que isso significa:**
- A condição é uma probabilidade, então o julgamento difuso vira um valor que o código comum pode comparar.
- O limiar de 0.90 encaminha para o caminho de fraude, que é a ação de alta confiança.
- O limiar de 0.65 envia a faixa intermediária para revisão humana, e não para uma ação automática.
- O ramo `else` segue o fluxo normal, então o software mantém o controle do que acontece em cada faixa.

A síntese do autor: a IA lida com a ambiguidade, e o software mantém o controle.

> 🔎 Contexto adicionado: os limiares 0.90 e 0.65 são valores ilustrativos no exemplo do artigo, não valores que a TypeSafe recomende.

## A camada de confiança pode ser mais importante que a decisão
O Jev não devolve apenas uma resposta; ele também expõe probabilidades e confiança para os tipos de decisão que oferece. Isso permite arquiteturas construídas em torno da incerteza. O autor mostra um caso claro, em que cobrança tem 0.97, técnico tem 0.02 e fraude tem 0.01, e um menos claro, em que cobrança tem 0.42, técnico tem 0.36 e fraude tem 0.22. O primeiro pode ser encaminhado com segurança. O segundo deve se comportar de forma diferente, talvez com um modelo mais forte, uma pergunta de esclarecimento ao usuário ou um humano.

```javascript
billing issue: 0.97
technical issue: 0.02
fraud: 0.01
```

```javascript
billing issue: 0.42
technical issue: 0.36
fraud: 0.22
```

**O que isso significa:**
- O primeiro bloco é uma decisão confiante: um rótulo concentra quase toda a probabilidade, então o roteamento é seguro.
- O segundo bloco é um empate apertado entre cobrança e técnico, com fraude não muito atrás.
- A mesma saída do classificador gera duas ações diferentes dependendo da margem, e esse é o ponto do exemplo.
- A regra do autor: a autonomia deve depender da incerteza, do risco e da reversibilidade, e não de "o LLM disse algo".

```javascript
High confidence
      ↓
Autonomous action
      ↓
Medium confidence
      ↓
Stronger model / more context
Low confidence
      ↓
Human review
```

**O que isso significa:**
- O fluxo associa faixas de confiança a ações, indo de autônoma para modelo mais forte e depois humano.
- Cada faixa tem um custo de erro diferente, então cada uma tem uma ação diferente.
- Torna a incerteza explícita, o que uma resposta de texto simples não faz.
- O autor chama isso de "muito mais próximo de como sistemas de IA em produção precisam operar".

> ❌ **Anti-padrão — executar o que o LLM disse:** tratar a saída de um modelo como autoridade e executá-la, sem considerar a confiança. **Por que as pessoas fazem isso:** uma única resposta é mais simples de programar e parece decisiva. **Consequência:** decisões de baixa confiança e alto risco são executadas com a mesma autonomia que as fáceis. **Correção:** condicione as ações à probabilidade, ao risco e à reversibilidade, e envie os casos incertos para um modelo mais forte ou para uma pessoa.

## Mas saída estruturada não resolve isso?
O autor antecipa essa objeção e a considera razoável. Uma classe no estilo Pydantic, com uma rota `Literal`, uma prioridade inteira e um booleano de escalonamento, dá saídas estruturadas confiáveis de um LLM. A resposta do autor é que "saída estruturada" esconde vários problemas diferentes, e o Jev trata quatro deles.

```py
class Decision(BaseModel):
    route: Literal["billing", "technical", "fraud"]
    priority: int
    escalate: bool
```

**O que isso significa:**
- `route` é um `Literal`, então o schema restringe a resposta a três rótulos.
- `priority` é um `int`, o que fixa o tipo, mas não a escala nem o significado do número.
- `escalate` é um `bool`, o que esconde quão certo o modelo estava ao escolher verdadeiro ou falso.
- O argumento do autor é que um schema fixa a forma da resposta, mas não sua velocidade, seu custo ou sua calibração.

### 1. Confiabilidade do schema
A saída estruturada ajuda a garantir `route = billing`, e não uma frase como "acredito que esta requisição provavelmente deve ir para a equipe de cobrança". O Jev trata isso nativamente, restringindo o espaço de saída às escolhas declaradas.

### 2. Latência
Modelos generativos produzem sequências, um token após o outro, então o tempo cresce com o tamanho da resposta. O Jev avalia várias perguntas em paralelo. A TypeSafe informa atualmente uma latência de ponta a ponta de cerca de 70 a 500 milissegundos para o Jev.

### 3. Custo
A TypeSafe atualmente cobra do Jev US$ 0,042 por milhão de tokens de entrada, sem cobrança separada pela saída gerada. Para decisões ocasionais essa diferença pode ser irrelevante; para milhões ou bilhões de decisões de agentes, diz o autor, ela se torna uma questão de arquitetura.

> 🔎 Contexto adicionado: "por milhão de tokens de entrada" é uma unidade de preço, então o custo de uma decisão cresce com a quantidade de estado enviada ao modelo, e não com a quantidade de texto que ele escreve.

### 4. Calibração
Uma resposta estruturada de LLM pode dizer `{"fraud": true}`. A pergunta mais difícil é quão certo o sistema deve estar sobre essa resposta. O autor afirma que o Jev é explicitamente treinado em torno de decisões calibradas, e não de geração eloquente, segundo a TypeSafe. Isso não torna toda decisão correta, mas muda aquilo para o que o modelo é otimizado.

> 🔎 Contexto adicionado: um modelo calibrado é aquele cujas probabilidades declaradas correspondem à frequência com que esses resultados de fato ocorrem. O artigo não traz medições de calibração para o Jev.

## Espere, "zero alucinações"?
O marketing da TypeSafe afirma zero alucinações, e o autor separa dois significados. Como as saídas do Jev são predefinidas, ele não pode inventar um novo tipo de saída: se as escolhas forem cobrança, técnico e fraude, ele não pode devolver `super-premium-customer-success-department`. Nesse sentido, a alucinação de schema ou de tipo desaparece.

Ainda assim, ele pode errar. Pode classificar com confiança uma requisição de fraude como cobrança. A própria TypeSafe separa sua garantia de segurança de tipos da correção mais ampla e reconhece incertezas em algumas de suas avaliações. A formulação do autor é que o Jev é "incapaz de produzir um tipo de saída inválido, e não incapaz de tomar uma decisão incorreta", e a segunda afirmação seria muito mais forte e muito mais difícil de provar.

> ❌ **Anti-padrão — confundir tipo válido com decisão correta:** ler "a saída sempre tem um tipo válido" como "a saída está certa". **Por que as pessoas fazem isso:** uma afirmação de marketing sobre alucinações é fácil de ler como uma afirmação sobre precisão. **Consequência:** os sistemas deixam de avaliar a precisão e confiam em rótulos errados com aparência de certeza. **Correção:** meça a precisão das decisões com os seus próprios dados, separadamente da validade do schema.

## As métricas de desempenho são impressionantes, mas são muito recentes
A TypeSafe relata ganhos grandes nos seus fluxos System One, com o Jev cerca de 193,6 vezes mais rápido e 444,6 vezes mais barato nos fluxos de comparação. O autor diz que os números devem ser lidos com cuidado. A própria TypeSafe afirma que os resultados provavelmente estão no extremo superior dos ganhos reais, e os fluxos foram criados por membros da sua própria equipe de capacidades de modelo, de modo que o viés de avaliação não pode ser totalmente descartado. Além disso, o Jev foi lançado apenas alguns dias antes do artigo.

O autor não conclui que os LLMs são "400 vezes caros demais". O ponto mais duradouro é que especializar um modelo para a tarefa computacional real pode produzir grandes ganhos de eficiência. O autor diz que esse princípio é maior do que o próprio Jev.

> 🔎 Contexto adicionado: o "400×" aparece na frase retórica do próprio autor e não é um dos números relatados pela TypeSafe; os números relatados são 193,6× e 444,6×.

## Isso pode mudar os sistemas multiagente
O autor considera o conceito mais interessante em sistemas multiagente. Imagine um sistema com um agente de Cliente, Produto, Pedido, Reembolso, Fraude, Conhecimento e Escalonamento Humano. Algo precisa decidir qual agente trata cada requisição. Hoje isso costuma ser outra chamada de LLM, depois outra para escolher uma ferramenta, outra para avaliar a resposta e outra para verificar a conclusão. A frase do autor: construímos uma organização em que todo funcionário é um redator de ensaios muito bem pago, inclusive a recepcionista que só precisava apontar a sala certa.

Um modelo de decisão poderia atuar como a malha de roteamento entre agentes, ficando na frente dos agentes e de novo depois de cada um deles para perguntar se a tarefa está concluída.

```javascript
                Incoming Request
                        │
                        ▼
                 Decision Layer
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Order Agent      Product Agent    Refund Agent
        │               │               │
        └───────────────┼───────────────┘
                        ▼
                 Decision Layer
                        │
                 Is task complete?
```

**O que isso significa:**
- A primeira Decision Layer encaminha a requisição recebida para um de vários agentes especialistas.
- Os três agentes mostrados são exemplos da lista; o mesmo padrão se aplica aos demais agentes que o autor menciona.
- A segunda Decision Layer reúne as saídas e pergunta se a tarefa está concluída.
- O ponto do autor: modelos diferentes realizam tipos diferentes de cognição, que é a inteligência em camadas que ele espera que a engenharia de agentes adote.

## Onde o Jev poderia ser útil de fato
O autor argumenta que os casos de uso óbvios não são glamourosos, e é por isso que importam. O artigo lista dez: roteamento de agentes, seleção de ferramentas, filtragem de recuperação (um documento é relevante o bastante para entrar no contexto?), guardrails (uma saída viola alguma política?), escalonamento (uma pessoa deve assumir?), roteamento de modelos (a requisição precisa de um modelo de raciocínio caro ou de um mais barato?), avaliação de qualidade (uma resposta gerada atende à requisição?), decisões de nova tentativa (uma chamada de ferramenta falhou de forma recuperável?), roteamento de atendimento ao cliente (fila, prioridade, sentimento ou fluxo de trabalho) e verificações de fraude e risco (o estado justifica mais verificação?). O ponto do autor é que essas são as decisões invisíveis repetidas centenas de vezes dentro de software em produção.

## Onde você provavelmente não deve usar o Jev
O autor é claro sobre o que o Jev não substitui. Escrever um e-mail, explicar por que uma consulta SQL falha, resumir dez documentos, gerar código e raciocinar de forma aberta sobre um problema ambíguo continuam pertencendo a um LLM ou a um modelo de raciocínio capaz. O Jev serve para perguntas em que a resposta é A ou B, quanto, sim ou não, qual rota e quão confiantes estamos. O autor conclui que "Jev vs LLM" é o enquadramento errado, e que a melhor arquitetura é Jev mais um LLM mais código determinístico mais ferramentas.

## Talvez o futuro não seja um único modelo fundacional
O autor observa que a indústria muitas vezes buscou um modelo fundacional cada vez mais poderoso que fizesse tudo, e isso produziu sistemas notáveis. Já a engenharia de produção tende a se especializar. Os bancos de dados não eliminaram os caches, a nuvem não eliminou as CDNs, as CPUs não eliminaram as GPUs e os microsserviços não tornaram todos os serviços idênticos. A IA pode seguir o mesmo caminho.

O autor lista as possíveis partes especializadas: modelos generativos para linguagem e conteúdo, modelos de raciocínio para problemas difíceis de múltiplas etapas, modelos de decisão para roteamento, pontuação e classificação, modelos de embedding para representação semântica, modelos de visão para percepção, código para lógica determinística e ferramentas para ações. Uma aplicação inteligente passa a ser então uma camada de orquestração sobre essas capacidades, e não "tudo → um LLM gigante".

## Por que o nome "Jev"?
A TypeSafe diz que o nome homenageia o economista William Stanley Jevons e o paradoxo de Jevons. A ideia simplificada é que tornar um recurso mais eficiente de usar pode aumentar o consumo total dele em vez de reduzi-lo. O autor aplica isso à inteligência: uma decisão inteligente que custa segundos e um dinheiro considerável é usada ocasionalmente, enquanto uma que custa milissegundos e quase nada pode ser usada uma vez por requisição do usuário, uma vez por etapa do agente, várias vezes por etapa ou continuamente ao longo de toda a aplicação.

> 🔎 Contexto adicionado: o paradoxo de Jevons foi descrito pela primeira vez para o uso de carvão, na Inglaterra do século XIX, onde motores mais baratos e eficientes levaram a um consumo total de carvão maior. O próprio artigo traz apenas o enunciado simplificado acima.

O autor descreve isso como a aposta maior: não apenas tornar a IA existente mais barata, mas tornar a inteligência barata o suficiente para que o software a use onde antes a IA não fazia sentido.

## Minha opinião: a ideia importante não é o Jev. É a separação da inteligência.
O autor diz que o Jev é extremamente novo, que suas afirmações mais fortes de benchmark precisam de validação independente mais ampla, e que "System One Model" é terminologia da TypeSafe, e não um padrão estabelecido da indústria. O Jev pode se tornar uma primitiva de infraestrutura, provedores de LLM podem incorporar capacidades semelhantes em seus modelos, alternativas de código aberto podem surgir, ou outra arquitetura pode vencer.

A pergunta real do autor é por que um único modelo deveria executar todas as operações cognitivas de um sistema de IA. Escrever e decidir são coisas diferentes, assim como raciocinar e rotear, e gerar uma resposta e verificá-la. O artigo termina com uma pilha futura em que um evento ou fluxo do usuário entra em uma camada de decisão, que encaminha para código, ferramentas e agentes; os agentes podem usar um LLM de raciocínio, uma etapa de decisão ou verificação e um LLM generativo antes da saída. A afirmação final do autor é que os LLMs não estão desaparecendo, mas podem deixar de ser solicitados a fazer todo o trabalho, e que a contribuição mais útil do Jev pode ser levar os engenheiros de IA a perguntar: esta etapa realmente precisa de um modelo de linguagem, ou meu software só precisa de uma decisão?

```javascript
                ┌──────────────────┐
                │   User / Event   │
                └────────┬─────────┘
                         ▼
                ┌──────────────────┐
                │  Decision Layer  │
                │      Jev         │
                └────────┬─────────┘
                         ▼
            ┌────────────┼────────────┐
            ▼            ▼            ▼
          Code         Tools        Agents
                                       │
                                       ▼
                              ┌────────────────┐
                              │ Reasoning LLM  │
                              └───────┬────────┘
                                      ▼
                              Decision / Verify
                                      │
                                      ▼
                              Generative LLM
                                      │
                                      ▼
                                   Output
```

**O que isso significa:**
- O topo do diagrama é o gatilho, que pode ser uma mensagem do usuário ou um evento do sistema.
- A Decision Layer fica em primeiro lugar, decidindo se o evento vai para código, para ferramentas ou para agentes.
- Os agentes podem chamar um LLM de raciocínio, depois uma etapa de decisão ou verificação, e depois um LLM generativo para produzir a saída.
- O ponto do autor é que o LLM generativo é a última etapa, e não a primeira, então a maior parte da pilha é lógica comum de decisão e de ferramentas.

## 🧠 Modelos mentais
### If difuso
**O modelo:** o Jev age como um comando `if` cuja condição é um julgamento semântico, devolvendo uma probabilidade que o código comum pode comparar com limiares.
**Como aplicar:** substitua uma etapa de regra de palavras-chave ou de prompt e interpretação por uma pergunta tipada, e depois defina limiares para cada faixa de ação, com um caminho humano para a faixa intermediária.

### Geração versus julgamento
**O modelo:** um LLM é otimizado para produzir texto, enquanto um modelo de decisão é otimizado para produzir uma resposta limitada, de modo que os dois são trabalhos diferentes mesmo quando ambos devolvem dados estruturados.
**Como aplicar:** para cada etapa de um agente, pergunte se a saída é um texto que o usuário lê ou uma escolha em que o software age, e encaminhe de acordo.

### Autonomia condicionada à confiança
**O modelo:** a ação correta depende da confiança do modelo, do risco de uma ação errada e de a ação poder ser revertida.
**Como aplicar:** associe faixas de probabilidade a ações, por exemplo ação autônoma acima de um limiar alto, um modelo mais forte na faixa intermediária e revisão humana abaixo de um limiar baixo.

### Especialização
**O modelo:** a infraestrutura tende a se especializar com o tempo, com cada carga de trabalho encontrando uma arquitetura construída para ela.
**Como aplicar:** trate o modelo fundacional como um componente entre vários, e atribua cada operação cognitiva ao componente mais barato que consegue executá-la com confiabilidade.

## 🔑 Principais conclusões
- A maioria das etapas de agentes, como seleção de ferramentas, verificações de relevância, novas tentativas e roteamento, são julgamentos de conjunto fechado que não precisam de texto gerado.
- A interface do Jev devolve respostas Choice, Score e Noul sobre um estado compartilhado, avaliadas em paralelo, de modo que o software recebe decisões em vez de strings para interpretar.
- O exemplo do artigo relata uma latência de cerca de 70 a 500 milissegundos e um preço de US$ 0,042 por milhão de tokens de entrada, ambos informados pela TypeSafe.
- As probabilidades permitem ações diferentes conforme a confiança, então uma decisão de roteamento com 0.97 e um empate apertado com 0.42 não devem disparar o mesmo comportamento.
- Uma saída de tipo válido exclui rótulos inventados, mas não exclui decisões erradas, então a precisão precisa ser medida separadamente.
- Os números de 193,6× de velocidade e 444,6× de custo da TypeSafe vêm de fluxos criados pela própria equipe da empresa e foram lançados apenas alguns dias antes do artigo, então precisam de validação independente.
- A pergunta prática do artigo para qualquer desenho de agente é se uma etapa precisa de um modelo de linguagem ou apenas de uma decisão.

## 🏛️ Para o Kenji
Seu trabalho de nível staff com sistemas de agentes e governança de IA é onde isso se aplica com mais diretamente: uma camada de decisão com probabilidades calibradas é um lugar natural para associar limiares de risco, regras de escalonamento e registros de auditoria. Esta é a sua interpretação, não a do autor. A pergunta que ela levanta para você: quais das decisões nos seus desenhos atuais de agentes você se sentiria confortável em condicionar a uma probabilidade, e como mediria se essa probabilidade é calibrada com os seus próprios dados?

## 📚 Plano de leitura
### Do artigo
- O artigo não cita links. Suas fontes são mencionadas apenas no texto: TypeSafe AI (o anúncio e a documentação do Jev), LangChain (uma publicação sobre loops de agentes e chamadas de modelo depois do lançamento do Jev) e William Stanley Jevons (o paradoxo de Jevons).
### Para aprofundar
- Saídas estruturadas e tool calling em APIs de LLM — para comparar a abordagem com restrição de schema, que o autor trata como linha de base.
- Calibração de classificadores probabilísticos, incluindo diagramas de confiabilidade — para testar se uma probabilidade no estilo Noul significa o que diz.
- Avaliação com LLM-as-a-judge — para as etapas de qualidade e verificação que, segundo o autor, muitas vezes são feitas por outra chamada de LLM.
- Padrões de orquestração multiagente — para roteamento e verificações de conclusão entre agentes especialistas.
- Paradoxo de Jevons — para o argumento econômico de que inteligência mais barata aumenta o seu uso.
