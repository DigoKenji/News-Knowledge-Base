---
type: Article Summary
title: 10 principais casos de uso do JEV em aplicações agênticas, ranqueados pelo ganho teórico sobre LLMs
description: Separe os agentes para que decisões tipadas e baratas tratem julgamentos de conjunto fechado, enquanto LLMs mantêm a geração aberta e o planejamento.
resource: https://medium.com/@agentnativedev/top-10-jev-use-cases-in-agentic-apps-ranked-by-theoretical-gain-over-llms-5be9e293ea19
tags:
- ai-ml
- llm-engineering
- rag
- inference-tradeoffs
- structured-decision-model
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@agentnativedev/top-10-jev-use-cases-in-agentic-apps-ranked-by-theoretical-gain-over-llms-5be9e293ea19
  title: Top 10 JEV Use Cases in Agentic Apps. Ranked by Theoretical Gain Over LLMs.
  author: Agent Native
status: stable
source_url: https://medium.com/@agentnativedev/top-10-jev-use-cases-in-agentic-apps-ranked-by-theoretical-gain-over-llms-5be9e293ea19
source: Medium (agentnativedev)
source_type: medium
published: '2026-09-19'
domain: ai-ml
subdomain: llm-engineering
concepts:
- rag
- inference-tradeoffs
- structured-decision-model
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d8121a217fb1a2c7081f3
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-top-10-jev-use-cases-in-agentic-apps-ranked-by-theoretical-g.md
---

# 10 principais casos de uso do JEV em aplicações agênticas, ranqueados pelo ganho teórico sobre LLMs

**Fonte:** https://medium.com/@agentnativedev/top-10-jev-use-cases-in-agentic-apps-ranked-by-theoretical-gain-over-llms-5be9e293ea19
**Autor:** Agent Native
**Data:** 2026-09-19
**Conceitos:** [Geração aumentada por recuperação](/pt-br/concepts/ai-ml/rag.md) · [Vazão/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md) · [Modelo estruturado de decisão](/pt-br/concepts/ai-ml/structured-decision-model.md)

---

## 🎯 Tese central
Agentes não deveriam chamar um LLM de fronteira para cada pequena decisão: julgamentos de conjunto fechado (rotear, bloquear, pontuar, selecionar, parar, manter, escalar) podem migrar para uma camada rápida de decisão tipada, como o Jev, enquanto o LLM continua responsável pela geração, pelo planejamento e pelo raciocínio difícil.

## 🧭 Por que o autor escreveu o texto
Os laços dos agentes vivem fazendo perguntas de controle, como "estamos travados?" ou "qual ferramenta deve rodar?", e responder cada uma com uma chamada completa de LLM faz com que custo e latência se acumulem a cada iteração. O artigo argumenta que, se esses julgamentos forem atendidos por um modelo tipado com resposta em menos de um segundo, é possível se dar ao luxo de tomar centenas de decisões que antes eram ignoradas, e a arquitetura dos agentes muda.

## Abertura: o que é o Jev e por que a divisão importa
O Jev é descrito como um modelo "Sistema Um": você envia o estado da aplicação junto com perguntas tipadas, e ele devolve decisões tipadas e probabilísticas. Ele não gera prosa e não precisa de um prompt JSON seguido de parsing e validação, porque o seu software define primeiro o espaço de respostas.

> 🔎 Contexto adicionado: "Sistema Um" vem da divisão entre o modo rápido e intuitivo e o modo lento e deliberado da psicologia de processos duais. O artigo usa o termo de forma livre, para se referir a um modelo rápido que responde perguntas fechadas.

A TypeSafe informa tempos de resposta de cerca de 70 a 500 ms, acelerações de 40 a 200 vezes em consultas comparáveis de formato Sistema Um, e preço de US$ 0,042 por milhão de tokens de entrada, com a saída praticamente gratuita. A avaliação própria da empresa, com quatro fluxos de trabalho, relatou 193,6 vezes mais rápido e 444,6 vezes mais barato do que a configuração de LLM de referência. A conclusão do autor é que o LLM fica para a geração, o planejamento e o raciocínio difícil, enquanto os julgamentos repetidos de conjunto fechado migram para uma camada de decisão mais barata. O resultado é um sistema híbrido em que o modelo rápido reage e o modelo lento planeja. O artigo classifica dez lugares em que essa divisão deve gerar o maior ganho teórico, e linka um artigo anterior sobre o modelo de decisão de US$ 0,0001.

## Primeiro, o que o Jev realmente devolve
O Jev tem três primitivas, e elas formam o vocabulário de tudo o que vem depois.

### Choice
Escolhe uma opção dentro de um conjunto fixo e devolve a opção selecionada, suas probabilidades e um valor de confiança.

### Score
Posiciona algo em uma rubrica ordenada e devolve a pontuação, as probabilidades e a confiança.

### Noul
Estima se uma afirmação é verdadeira e devolve uma probabilidade de 0 a 1.

### Perguntas em lote sobre um mesmo estado
Muitas perguntas sobre o mesmo estado podem ser feitas em uma única chamada. A TypeSafe diz que cada pergunta é avaliada de forma independente, então acrescentar perguntas quase não altera o tempo de resposta. O exemplo abaixo faz, em uma só requisição, uma pergunta de roteamento, uma pontuação de severidade e uma verificação de urgência sobre um incidente.

```py
from typesafe_sdk import Choice, Noul, Score, TypeSafeClient

client = TypeSafeClient()

response = client.system_one(
    state="The deploy failed twice and customers are seeing 500s.",
    questions={
        "route": Choice(
            instructions="Which team should handle this?",
            criteria={
                "platform": "Infrastructure, deploys, runtime failures",
                "product": "Application behavior and product defects",
                "support": "Customer communication and follow-up",
            },
        ),
        "severity": Score(
            instructions="How severe is the incident?",
            criteria=["low", "medium", "high"],
        ),
        "urgent": Noul(
            instructions="This requires immediate attention",
        ),
    },
)

print(response.answers["route"].choice)
print(response.answers["route"].confidence)
print(response.answers["urgent"].noul)
```

Instale o SDK e defina a chave primeiro, com `pip install typesafe-sdk` e `TYPESAFE_API_KEY` exportada:

```javascript
pip install typesafe-sdk
export TYPESAFE_API_KEY=...
```

**O que isso faz:**
- Instala o SDK Python da TypeSafe e define a chave de API a partir do ambiente, para que o cliente possa se autenticar.
- Mostra que a chave é lida do ambiente, e não fixada no programa.
- O bloco de instalação aparece rotulado como javascript no artigo, mas contém comandos de shell; é reproduzido como está.

**O que isso faz (requisição):**
- `TypeSafeClient()` cria o cliente que envia uma requisição por estado.
- `state` são os dados da aplicação sobre os quais as decisões são tomadas; aqui, uma descrição de incidente em texto simples.
- `questions` associa nomes a objetos de pergunta tipados, então cada resposta volta sob uma chave escolhida pelo seu código.
- `Choice` recebe um conjunto fixo de critérios rotulados, `Score` recebe uma lista ordenada e `Noul` recebe uma afirmação de sim ou não.
- Ler `.choice`, `.confidence` e `.noul` separadamente permite que quem chama o código ramifique a lógica com base em cada resultado.

A frase final do artigo sobre este bloco: o que importa é que o seu código é dono da ramificação.

## 1. Laços em tempo real: jogos, robótica, controle interativo
Este é o maior ganho teórico, porque um LLM de fronteira é lento demais para participar de um laço rápido. A demo de Doom da TypeSafe roda a cerca de 10 decisões por segundo, a um custo informado de cerca de US$ 7 por hora, e exemplos da comunidade em Minecraft usam a mesma divisão: o Jev cuida das reações imediatas enquanto um modelo de fronteira planeja à frente.

### Laço de controle com orçamento de tempo
Um laço de controle observa o estado, decide, age e observa de novo. Seu orçamento de tempo é o tempo que ele tem para responder antes que o mundo mude. Uma resposta de um LLM de fronteira pode exceder esse orçamento por completo, então o laço perderia o prazo por melhor que o modelo fosse. Uma decisão tipada e rápida mantém o laço dentro do orçamento, e o planejador lento roda apenas quando necessário.

_(Diagrama: o Jev fica dentro do laço de controle; o planejador LLM o guia de fora.)_

```javascript
observe state -> fast decision -> action -> observe again
                     |
                     +-> slow planner only when needed
```

O ganho é um laço de controle que, de outra forma, perderia seu orçamento de tempo por completo. Para robótica, simulações, agentes multijogador, controle de interface local e outros sistemas reativos, o artigo chama isso de mudança categórica de capacidade.

## 2. Seleção de ações em navegador e uso de computador
Agentes de navegador gastam dinheiro porque cada passo pergunta a um modelo o que fazer em seguida. O artigo aponta o repositório jev-ultrafast, da Browser Use, como um formato melhor, em que cada observação da página vira um espaço de ações indexado.

### Espaço de ações indexado
O código lê a página e produz uma lista numerada de controles, então as ações candidatas são geradas de forma determinística, e não inventadas por um modelo. A decisão passa a ser escolher dessa lista.

### Seleção de operação e alvo
Uma única requisição ao Jev escolhe tanto a operação (por exemplo, clicar ou digitar) quanto um alvo compatível entre os controles indexados. É uma única decisão tipada por passo.

### Geração apenas onde ela é irredutível
Um pequeno modelo de texto só é acionado quando a operação escolhida é `TYPE_TEXT` e é preciso escrever texto de fato. A demo relatada no artigo, no Google Flights, conclui uma busca de Zurique para Londres em cerca de 7,1 segundos, já incluindo as esperas reais de carregamento.

_(Diagrama: o código indexa a página, e uma requisição ao Jev escolhe tanto a operação quanto o alvo.)_

```javascript
DOM -> indexed controls -> Jev(operation + target) -> browser action
                                      |
                                      +-> small LLM only for TYPE_TEXT
```

**O que isso faz:**
- Mostra o pipeline: a página vira controles indexados, o Jev escolhe uma operação e um alvo, e o navegador executa a ação.
- Coloca o pequeno LLM em um único ramo, `TYPE_TEXT`, de modo que as chamadas generativas ficam limitadas ao passo que precisa de texto livre.
- O trecho é um diagrama em forma de bloco de código, não código executável.

A recomendação do artigo: gere os candidatos no código, deixe o Jev escolher e use geração apenas na parte que é irredutivelmente generativa.

Início rápido a partir do repositório:

```javascript
git clone https://github.com/browser-use/jev-ultrafast.git
cd jev-ultrafast
uv sync
cp .env.example .env
# Add TYPESAFE_API_KEY and TEXT_MODEL_API_KEY
uv run jev
```

**O que isso faz:**
- Clona o repositório, instala as dependências com `uv` e copia o arquivo de ambiente de exemplo.
- Exige duas chaves: uma para o serviço de decisão tipada e outra para o pequeno modelo de texto usado por `TYPE_TEXT`.
- Inicia o agente com `uv run jev`.

Ou use a biblioteca diretamente:

```py
from jev_ultrafast import Agent

with Agent(
    "https://www.google.com/travel/flights?hl=en",
    "Find one-way flights from Zurich to London on September 20, 2026, "
    "for one adult in economy. Stop when matching flight options are visible.",
) as agent:
    for state in agent.run():
        print(state["elapsed_ms"], state["status"])
```

**O que isso faz:**
- O bloco `with` abre o agente em uma URL inicial com um objetivo em linguagem simples e o fecha corretamente depois.
- A string do objetivo está dividida em dois literais, o que é concatenação de strings comum em Python.
- O laço imprime o tempo decorrido e o status de cada estado que o agente produz, o que permite conferir as afirmações de tempo.

## 3. Controle de risco de ferramentas
Todo agente autônomo sério acaba precisando de um checkpoint de política antes que uma ferramenta seja executada. Os exemplos do artigo: este comando de shell é destrutivo, esta requisição exfiltra dados, esta chamada de ferramenta é permitida no modo automático, e um humano deve confirmar esta ação. A LangChain expõe esse padrão como `AutoModeMiddleware`.

```py
from langchain.agents import create_agent
from langchain_typesafe.experimental.middleware import AutoModeMiddleware

guardrail = AutoModeMiddleware(tools=["bash"])
agent = create_agent("openai:gpt-5.6-luna", middleware=[guardrail])
```

**O que isso faz:**
- Constrói um agente com `create_agent` e passa um objeto de middleware na lista de middlewares.
- `AutoModeMiddleware(tools=["bash"])` restringe a verificação à ferramenta nomeada, então apenas chamadas de shell passam pelo filtro.
- O guardrail fica entre a chamada de ferramenta do modelo e a execução, que é o lugar de uma verificação de risco.

A vantagem teórica é confiabilidade somada a latência: uma verificação de risco precisa ser barata o bastante para rodar antes de cada chamada de ferramenta.

### Execução condicionada à confiança
A ressalva do artigo: "saída tipada não pode alucinar" não significa "a decisão não pode estar errada". O Jev pode garantir que a resposta é uma das opções válidas, mas não que a opção selecionada está correta. Por isso, o padrão de produção é a execução condicionada à confiança, em que um limite de confiança escolhe entre executar, confirmar e bloquear.

> ❌ **Anti-padrão — confiança cega na saída tipada:** tratar qualquer valor que passe na verificação de tipo como uma decisão correta e executá-lo diretamente. **Por que as pessoas fazem isso:** um valor válido de enum parece uma resposta verificada, e o sistema de tipos dá a sensação de garantia. **Consequência:** decisões erradas, mas bem formadas, são executadas sem nenhum checkpoint. **Correção:** condicione a execução à confiança e envie decisões de baixa confiança ou de alto risco para confirmação ou para um plano alternativo.

_(Diagrama: um tipo válido não é uma decisão correta, então limiares de confiança escolhem entre executar, confirmar e bloquear.)_

## 4. Roteamento de modelos
Agentes desperdiçam tokens de modelos de fronteira em trabalho que não exige raciocínio de fronteira. O artigo coloca o Jev na frente da frota de modelos e torna o roteamento um middleware explícito: um modelo rápido para consultas diretas, extração e mudanças localizadas, e um modelo potente para arquitetura e decisões de alto risco.

### Roteamento antes da invocação do modelo
O roteamento fica em uma fronteira arquitetural limpa, antes de o modelo ser invocado, então uma decisão de roteamento custa quase nada em comparação com a chamada que ela evita. A economia se acumula em sessões longas, e o artigo considera o roteamento uma das integrações do Jev mais fáceis de justificar.

```py
from langchain_typesafe.experimental.middleware import (
    ModelChoice,
    ModelRouterMiddleware,
)

router = ModelRouterMiddleware(
    choices={
        "fast": ModelChoice(
            model="openai:luna",
            criteria="Direct lookups, extraction, and localized changes.",
        ),
        "powerful": ModelChoice(
            model="openai:sol",
            criteria="Architecture and high-stakes decisions.",
        ),
    },
    instructions="Choose the least costly model that can complete the task.",
)
```

**O que isso faz:**
- Declara duas escolhas de modelo nomeadas, cada uma com uma string de critérios em linguagem simples com a qual o roteador pode comparar.
- A escolha `fast` cobre o trabalho de rotina e a escolha `powerful` cobre o trabalho de nível arquitetural, então o roteador escolhe a opção mais barata que serve.
- A string `instructions` enuncia, em uma frase, a regra de custo para o roteador.

_(Diagrama: roteie antes de gastar: tarefas de rotina vão para o modelo rápido, e só as difíceis chegam à fronteira.)_

## 5. Verificações de objetivo e de travamento
Os laços dos agentes vivem fazendo variações das mesmas perguntas de controle: atingimos o objetivo, estamos progredindo, devemos tentar de novo, estamos travados, devemos parar e perguntar ao usuário. O artigo chama essas perguntas de decisões típicas de Noul ou de Choice pequeno. Muitos harnesses ainda gastam uma chamada completa de LLM com elas, o que faz com que o controle do laço seja mais lento do que a execução da ferramenta que ele supervisiona.

### Controle contínuo do laço
Um modelo de decisão com resposta em menos de um segundo torna essas verificações baratas o bastante para rodar a cada passo, e não apenas depois que as falhas ficam evidentes. Isso melhora a confiabilidade: cada verificação é um Noul que decide se continua, para ou escala.

_(Diagrama: barato o bastante para rodar a cada passo: cada verificação é um Noul que decide se continua, para ou escala.)_

## 6. Compactação de contexto
A maioria dos agentes de código compacta o contexto pedindo a um LLM que resuma os turnos antigos, e o artigo chama isso de com perda por definição. O projeto fast-jev-compaction segue outro caminho: pontua chamadas de ferramentas antigas e resultados de ferramentas, mantém o que ainda importa, trunca alguns resultados e descarta o material obsoleto. O texto do usuário e do assistente permanece literal.

### Pontuar, manter, descartar
Cada chamada de ferramenta ou resultado antigo recebe uma decisão de relevância. A saída obsoleta é apagada, alguns resultados são truncados e tudo o que sobrevive é mantido exatamente como foi escrito. O ponto mais profundo do artigo é que apagar pode preservar melhor a verdade do que reescrever.

> ❌ **Anti-padrão — reescrever o histórico para economizar espaço:** substituir turnos antigos por um resumo escrito por um LLM. **Por que as pessoas fazem isso:** é o método padrão de compactação e sempre produz um resultado curto. **Consequência:** o resumo é com perda por definição, então detalhes de que o agente precisará depois podem desaparecer sem aviso. **Correção:** apague ou trunque o material obsoleto, mantenha o texto sobrevivente literal e tenha um plano alternativo para quando a redução for pequena demais para importar.

_(Diagrama: pontuar, manter, descartar: a saída obsoleta de ferramentas desaparece, e tudo o que sobrevive permanece literal.)_

Instale e defina sua chave:

```javascript
npm install fast-jev-compaction
export TYPESAFE_API_KEY=...
```

**O que isso faz:**
- Instala o pacote npm e define a mesma chave de API que as outras ferramentas do Jev usam.

Depois, compacte uma transcrição, com um plano alternativo para quando a redução for pequena demais para valer a pena:

```ts
import {
  compactMessages,
  reductionRatio,
  type Message,
} from 'fast-jev-compaction';

const result = await compactMessages(transcript, {
  preserveRecentMessages: 4,
});

if (reductionRatio(result) < 0.25) {
  // Keep the original transcript or fall back to summarization.
}
```

**O que isso faz:**
- Importa a função de compactação, a medida de redução e um tipo `Message`.
- `preserveRecentMessages: 4` protege as quatro mensagens mais recentes contra remoção.
- `reductionRatio(result) < 0.25` verifica se a compactação economizou menos de um quarto; nesse caso, quem chama mantém o original ou recorre à sumarização.

O resultado da comunidade que chamou atenção: quase 1 milhão de tokens reduzidos para 86 mil em cerca de um segundo.

### Irrelevante agora não é reproduzível depois
A compactação é onde a confiança mais importa. O artigo diz que sistemas de produção devem fixar saídas não reproduzíveis, restrições recentes e estado de ferramentas não resolvido antes de permitir a exclusão baseada em relevância, porque material que parece irrelevante agora pode ser necessário para reproduzir um resultado depois.

## 7. Seleção de ferramentas e skills
Os prompts dos agentes continuam crescendo porque os desenvolvedores colocam no contexto do modelo a descrição de cada ferramenta e cada skill. O artigo chama isso de contraproducente e propõe um fluxo em que as ferramentas ou skills candidatas passam por uma única Choice do Jev, e só os vencedores são ativados.

### Uma Choice sobre o catálogo inteiro
Parte do ganho é de custo, mas o ganho mais interessante é a qualidade do prompt: o modelo generativo vê uma superfície de ações menor e mais relevante, em vez de dezenas de especificações de ferramentas irrelevantes. Isso importa mais para agentes de plataforma cujos catálogos mudam dinamicamente e podem ter centenas de capacidades. O artigo observa que a Choice suporta até 255 opções, o que cobre muitos registros reais sem precisar pedir antes a um LLM que faça uma pré-seleção.

```javascript
request -> candidate tools/skills -> Jev Choice -> activate only the winner(s)
```

**O que isso faz:**
- Mostra o fluxo em quatro passos: a requisição, as ferramentas ou skills candidatas, uma única decisão Choice e a ativação apenas dos itens selecionados.
- O passo Choice substitui uma etapa de pré-seleção por LLM, e é daí que vem a economia de custo.
- É um esboço de fluxo, não código executável.

_(Diagrama: uma Choice sobre o catálogo inteiro; só os vencedores chegam ao contexto do modelo.)_

## 8. Guardrails de saída e de rastreamento
Guardrails costumam ser outro prompt de LLM que roda depois que o LLM caro já terminou, o que pode ser lento demais para ser aplicado em todo lugar. O Jev torna o "verificar tudo" viável: pontua prompts, rastros, saídas de ferramentas ou respostas finais quanto a risco de política, assinaturas de jailbreak ou restrições de domínio. A arquitetura mais forte faz várias perguntas estreitas e combina as respostas no código.

### Várias perguntas estreitas, combinadas por código de política
Cada pergunta estreita tem uma saída e um limite definidos, o que é mais fácil de testar do que um único prompt gigante de moderação em linguagem natural. O exemplo do artigo combina três pontuações e um valor de confiança em uma única decisão de bloquear e auditar.

```javascript
jailbreak? 0.04
pii leak?  0.11
unsafe?    0.87
confidence 0.93

=> block + audit
```

**O que isso faz:**
- Lista três pontuações de risco estreitas e um valor de confiança para uma única saída.
- A última linha é o resultado de política que o código deriva desses valores.
- Os números mostram por que o desenho é testável: cada sinal tem o seu próprio limite.

_(Diagrama: várias perguntas estreitas, combinadas por código de política, em vez de um único prompt gigante de moderação.)_

> ❌ **Anti-padrão — um único prompt gigante de moderação:** pedir a um único prompt em linguagem natural que julgue todos os riscos de uma vez. **Por que as pessoas fazem isso:** é uma chamada e um prompt para manter. **Consequência:** cada decisão não tem saída nem limite definidos, então é difícil testar e ajustar. **Correção:** divida a pergunta em decisões tipadas e estreitas e combine-as no código.

## 9. Triagem de tickets e e-mails
O roteamento de alto volume é onde economizar pequenos custos por item se torna uma economia real de infraestrutura. O quickstart da TypeSafe é essencialmente um pipeline de triagem de tickets de suporte: escolher um departamento, pontuar a frustração e estimar a urgência em uma única requisição. Construtores da comunidade aplicaram o mesmo padrão a lotes de e-mails.

### Julgamentos com lote em primeiro lugar
Quando um fluxo de trabalho precisa de três ou dez julgamentos por item, perguntas tipadas em paralelo importam mais do que a velocidade bruta de uma chamada única. A implementação certa compartilha um estado, faz todas as perguntas independentes que possam ser necessárias e deixa o código a jusante decidir quais saídas importam.

_(Diagrama: triagem com lote em primeiro lugar: uma chamada ao Jev responde departamento, frustração e urgência juntos.)_

## 10. Reranqueamento em RAG
A recuperação devolve candidatos, mas não garante que eles pertençam ao contexto da resposta. A correção óbvia é um juiz de relevância para cada trecho, mas um LLM de fronteira por trecho é caro o suficiente para que muitas equipes aceitem uma recuperação ruidosa.

### Julgamento de relevância por trecho
Uma pergunta Noul por trecho recuperado fornece uma probabilidade barata de relevância, sim ou não, e os trechos acima do limite sobrevivem para o contexto da resposta.

```py
from typesafe_sdk import Noul, TypeSafeClient

client = TypeSafeClient()
kept = []

for chunk in retrieved_chunks:
    verdict = client.system_one(
        state={"question": user_question, "chunk": chunk.text},
        questions={
            "relevant": Noul(
                instructions="The chunk contains information needed to answer the question"
            )
        },
    )

    if verdict.answers["relevant"].noul > 0.5:
        kept.append(chunk)
```

**O que isso faz:**
- Cria um cliente e uma lista vazia `kept` que reúne os trechos sobreviventes.
- Para cada trecho, envia a pergunta e o trecho como estado, com uma pergunta Noul sobre relevância.
- Mantém o trecho apenas quando sua probabilidade Noul está acima de 0,5, o limite de exemplo do artigo.
- O artigo usa uma requisição por trecho aqui, o que é barato porque cada julgamento é uma chamada tipada pequena.

O pipeline resultante é:

```javascript
retrieve broadly -> judge cheaply -> generate from the survivors
```

**O que isso faz:**
- Mostra três etapas: recuperação ampla, um julgamento barato por trecho e, em seguida, a geração apenas a partir dos trechos sobreviventes.

Ele ocupa a décima posição porque o reranqueamento já pode ser resolvido com embeddings, cross-encoders ou rerankers dedicados. A afirmação do artigo é que a vantagem do Jev é mais forte quando a relevância depende de um estado de aplicação mais rico do que a similaridade vetorial consegue capturar.

_(Diagrama: recupere de forma ampla, julgue barato e gere apenas a partir dos trechos que sobrevivem.)_

## A arquitetura que eu usaria
Não substitua o modelo de agente pelo Jev. Divida o sistema pela forma computacional. Use o LLM quando o espaço de saída for aberto: planejamento, síntese, código, explicação, argumentos novos de ferramentas e raciocínio difícil de múltiplos passos. Use o Jev quando o espaço de saída puder ser definido antes da inferência: rotear, bloquear, pontuar, selecionar, parar, tentar de novo, manter, excluir, escalar.

### Limiares de confiança no caminho de controle
O caminho de controle usa a confiança para decidir o que acontece em seguida: baixa confiança vai para um humano ou para um plano alternativo, ações de alto risco com confiança média precisam de confirmação, e o restante é executado. O artigo diz que os limiares exatos devem vir do seu próprio conjunto de avaliação, e não de um post de blog.

```py
if decision.confidence < 0.5:
    ask_human_or_fallback()
elif high_risk_action and decision.confidence < 0.9:
    require_confirmation()
else:
    execute()
```

**O que isso faz:**
- O primeiro ramo envia decisões de baixa confiança para um humano ou para um caminho alternativo.
- O segundo ramo exige confirmação para ações de alto risco cuja confiança está abaixo de 0,9.
- O `else` final executa todo o restante, então quem chama define a política, e não o modelo.

O padrão central do artigo: a IA gera, o código restringe e o Jev decide entre opções conhecidas. Quanto mais agêntica uma aplicação se torna, mais pequenos julgamentos ela acumula, e se cada um acordar um LLM de fronteira, custo e latência se acumulam a cada iteração do laço. A promessa do Jev é que você pode se dar ao luxo de tomar centenas de decisões que antes evitava tomar.

## 🧠 Modelos mentais
### A IA gera, o código restringe, o Jev decide
**O modelo:** Três papéis em um sistema: o LLM produz saída aberta, o código determinístico define o espaço de respostas e impõe os ramos, e a camada de decisão tipada escolhe entre opções conhecidas.
**Como aplicar:** Antes de adicionar uma chamada de LLM, pergunte se o espaço de saída pode ser enumerado primeiro; se puder, use uma decisão tipada e deixe o código ser dono da ramificação.

### O modelo rápido reage, o modelo lento planeja
**O modelo:** Duas velocidades em um mesmo agente. Um modelo rápido fica dentro do laço de controle, e um modelo lento o guia de fora.
**Como aplicar:** Coloque tudo o que precisa terminar dentro de um orçamento de tempo no caminho rápido, e reserve o planejador para decisões que podem esperar.

### Um tipo válido não é uma decisão correta
**O modelo:** A validade do tipo garante que a resposta é uma das opções, não que ela é a certa.
**Como aplicar:** Acompanhe cada decisão tipada com um valor de confiança e condicione a execução a ele, usando limiares do seu próprio conjunto de avaliação.

## 🔑 Principais conclusões
- O Jev devolve respostas tipadas Choice, Score e Noul com probabilidades e confiança, e muitas perguntas sobre um mesmo estado podem ser feitas em uma única chamada.
- Laços em tempo real são o maior ganho: a demo de Doom da TypeSafe roda a cerca de 10 decisões por segundo, algo que um LLM de fronteira não consegue igualar dentro de um laço de controle.
- Agentes de navegador podem indexar a página em controles, deixar uma única requisição tipada escolher operação e alvo, e chamar um pequeno modelo de texto apenas para TYPE_TEXT.
- A execução condicionada à confiança é necessária porque uma opção válida ainda pode ser a errada.
- Compactação de contexto por exclusão (pontuar, manter, descartar) preserva melhor o texto literal do que uma reescrita por LLM, mas fixe as saídas não reproduzíveis antes de excluir.
- A Choice suporta até 255 opções, então um catálogo inteiro de ferramentas pode ser filtrado em uma decisão antes que o modelo generativo veja qualquer especificação.
- Várias perguntas tipadas e estreitas, combinadas no código, são mais fáceis de testar e ajustar do que um grande prompt de moderação.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Sua experiência em governança de IA combina com o padrão de execução condicionada à confiança: os limiares, o registro de auditoria das decisões bloqueadas e confirmadas e o conjunto de avaliação que define esses limiares são, por si sós, artefatos de governança. A pergunta que isso levanta para o seu trabalho de arquitetura: quais decisões de controle do seu sistema poderiam ser movidas com segurança para uma camada tipada e auditável, e como você mostraria a um regulador que os limiares vieram de evidências?

## 📚 Plano de leitura
### A partir do artigo
- [Artigo anterior sobre o modelo de decisão de US$ 0,0001](https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab) — a introdução do autor ao Jev e à sua precificação, para ler primeiro como base que este artigo pressupõe.
- [Repositório jev-ultrafast da browser-use](https://github.com/browser-use/jev-ultrafast.git) — a implementação de referência do controle de navegador por ações indexadas, útil para ver o padrão de operação mais alvo no código.

### Para aprofundar
- LLM como juiz — a forma mais antiga de usar um modelo para avaliar saídas, útil para comparar com decisões tipadas dos mesmos julgamentos.
- Definir limiares de confiança a partir de um conjunto de avaliação — como calibrar os cortes de executar, confirmar e bloquear, que o artigo diz não deverem ser copiados de um blog.
