---
type: Article Summary
title: Jev explicado com clareza
description: 'Decisões limitadas devem ser pontuadas em vez de geradas: uma única passagem direta produz probabilidades calibradas que o código pode comparar com limiares, embora a correção ainda exija avaliação independente.'
resource: https://medium.com/gitconnected/jev-clearly-explained-3203d6d48de3
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://medium.com/gitconnected/jev-clearly-explained-3203d6d48de3
  title: Jev Clearly Explained
  author: Level Up Coding (Gitconnected)
status: stable
source_url: https://medium.com/gitconnected/jev-clearly-explained-3203d6d48de3
source: Level Up Coding (Gitconnected)
source_type: medium
published: '2026-09-21'
domain: ai-ml
concepts:
- structured-decision-model
- calibration
- tokenization
- large-language-model
- ai-agent
- llm-as-a-judge
- inference-tradeoffs
- model-evaluation
- logit
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d81139ef4f54799137a98
lang: pt-br
translation_of: /notes/ai-ml/2026-11-25-jev-clearly-explained.md
---

## 🎯 Tese central
Quando o código já conhece as respostas possíveis para uma pergunta, um modelo deve pontuar essas respostas em uma única passagem direta em vez de escrevê-las, e as probabilidades resultantes devem guiar a lógica de ramificação.

## 🧭 Por que o autor escreveu
Fareed Khan parte de um padrão comum: uma aplicação pede a um modelo generativo completo um julgamento de uma palavra, depois converte a prosa de volta em um rótulo, pagando pela geração token a token em cada chamada. Ele apresenta o Jev, um modelo de decisão fechado da TypeSafe AI que retorna respostas tipadas com probabilidades e nunca escreve texto. Se a tese se sustentar, agentes e pipelines podem tomar muitas pequenas decisões de forma mais rápida e barata, com os limiares de decisão mantidos em código comum.

## Abertura: pontuar uma decisão em vez de escrevê-la
O artigo começa com uma verificação de fraude em uma fatura. Um modelo generativo responderia com uma frase como "esta fatura parece legítima", que a aplicação precisaria interpretar, torcendo para que a redação nunca quebre seu parser. Um modelo de pontuação retorna três números para três desfechos declarados, fraude 0.07, limpa 0.88 e revisar 0.05, em uma única passagem, sem nenhuma frase para interpretar. O autor usa esse único exemplo para mostrar toda a ideia, do primeiro estado até a linha de código que faz a ramificação.

### Decisão tipada
Uma decisão tipada é uma resposta restrita a uma lista que o desenvolvedor declarou antecipadamente, como fraude, limpa ou revisar. Como a lista é fixa, a saída pode ser uma probabilidade por opção em vez de texto livre.

> 🔎 Contexto adicionado: "Closed weights" (pesos fechados) significa que os parâmetros do modelo não são publicados, então leitores externos não podem inspecioná-los diretamente.

## O problema pelo qual continuamos pagando preço cheio
A maior parte do que o software precisa de um modelo de linguagem é um julgamento, não um parágrafo: se um chamado é urgente, qual modelo deve atender uma requisição, se um comando de shell é perigoso, se um trecho responde a uma pergunta. Cada um desses julgamentos tem uma de algumas respostas conhecidas. As equipes ainda enviam cada pergunta para um modelo generativo completo e convertem o resultado de volta no rótulo que queriam. Dentro de um loop de agente o custo se acumula, porque o agente faz essas pequenas chamadas constantemente.

```javascript
while not task_done:
    action = llm(context)          # full generation, just to pick a tool
    result = run_tool(action)
    context += result
    task_done = llm(context)       # full generation again, just to check "done or not"
```

**O que isso faz:**
- Executa um loop no qual cada iteração chama o modelo duas vezes para decisões que precisam de apenas alguns bits.
- Chama `llm` uma vez para escolher a próxima ferramenta e de novo para testar se a tarefa está concluída.
- Mostra que o custo de cada decisão é uma geração completa, token por token, mesmo que a resposta seja um de poucos rótulos.

A diferença se resume a quantas vezes o modelo precisa rodar. Escrever T tokens custa T passagens sequenciais pelo modelo, porque cada passagem depende do token que a anterior escreveu. Uma resposta limitada precisa de apenas uma passagem para ler as pontuações das opções declaradas.

> ❌ **Anti-padrão — gerar e depois analisar:** Pedir a um modelo generativo uma frase ou um objeto JSON apenas para recuperar um rótulo que o código poderia ter pontuado. **Por que as pessoas fazem isso:** É a interface padrão, e funciona para tarefas abertas. **Consequência:** Geração sequencial repetida em loops de agente, mais um parser que pode falhar quando a redação muda. **Correção:** Quando o conjunto de respostas é conhecido, peça uma pontuação sobre essas opções.

A aposta do Jev é que a geração é a interface errada quando os desfechos podem ser listados antecipadamente. Para esse caso, o autor argumenta que o modelo deve pontuar, não escrever.

### Custo sequencial versus passagem única
A geração é sequencial porque cada token depende do anterior. A pontuação não tem essa dependência, então o trabalho do modelo para uma decisão limitada acontece em uma única passagem cuja saída é lida diretamente.

## O que é o Jev
O artigo descreve o Jev como um motor de decisão semântica. O chamador envia o estado, ou seja, a situação atual, mais uma ou mais perguntas sobre ela. O Jev retorna uma resposta tipada com uma probabilidade, e nunca uma frase. A imagem no topo do post mostra esse fluxo: estado e perguntas entram, cada pergunta é avaliada na mesma passagem, um limiar de confiança decide o ramo, e código comum o executa.

Cada pergunta declara seu formato de resposta antecipadamente. O Jev oferece três primitivas:
- **Choice** escolhe uma opção de uma lista definida pelo desenvolvedor e retorna uma probabilidade para cada opção, não apenas para a vencedora.
- **Score** posiciona a entrada em uma escala ordenada definida pelo desenvolvedor, como uma escala de não resolvido a resolvido, e retorna um valor como 1.74.
- **Noul** responde a uma pergunta de sim ou não com a probabilidade de ela ser verdadeira. O artigo observa que este é o nome próprio da TypeSafe para a primitiva de tipo booleano.

Toda resposta é um número entre 0 e 1 que o código pode usar diretamente.

### Score como média ponderada por probabilidade
Score não é um artefato de arredondamento. É a média ponderada pelas probabilidades de cada degrau da escala, não apenas da escolha principal. Não resolvido, parcial e resolvido carregam cada um uma probabilidade, e sua soma ponderada é 1.74. Um "resolvido" certo cairia perto de 2.0, então 1.74 mostra uma forte inclinação para resolvido com peso real em parcial, uma informação que um único rótulo descarta.

Aqui está uma requisição para o chamado de suporte que o autor acompanha ao longo do post.

```json
{
  "model": "jev-latest",
  "state": "I was charged twice for the same subscription.",
  "questions": {
    "urgent": {
      "type": "noul",
      "instructions": "Does this need attention right now?"
    },
    "team": {
      "type": "choice",
      "instructions": "Which team should own this ticket?",
      "criteria": {
        "billing": "Charges, invoices, refunds",
        "technical": "Product bugs and errors",
        "account": "Login and access problems"
      }
    }
  }
}
```

**O que isso faz:**
- Envia uma string de estado e duas perguntas em uma única requisição.
- Declara `urgent` como uma pergunta de sim ou não e `team` como uma escolha entre três critérios nomeados.
- Mostra que o formato da resposta é fixado antes de o modelo rodar.

A resposta não contém prosa, apenas rótulos e números.

```json
{
  "urgent": {"probability": 0.61},
  "team": {
    "choice": "billing",
    "probabilities": {"billing": 0.91, "technical": 0.06, "account": 0.03}
  }
}
```

**O que isso faz:**
- Retorna uma probabilidade de 0.61 de que o chamado seja urgente.
- Retorna billing como escolha, com probabilidades 0.91, 0.06 e 0.03 entre as três equipes.
- Mostra que `team` só pode ser um dos três nomes declarados, então o modelo não pode inventar uma quarta equipe.

Isso difere de saída estruturada. Uma chamada de saída estruturada ainda decodifica um token por vez, e o schema apenas restringe a forma desses tokens. O caminho de pontuação do Jev nunca entra no loop de decodificação e lê a resposta a partir de uma única passagem direta. O artigo alinha as duas abordagens em saída, amostragem, adequação, controle e modo de falha, e elas diferem em todas as linhas. A saída estruturada permanece autorregressiva sob seu schema, enquanto a pontuação de respostas fixas lê a resposta antes de a decodificação começar.

## De onde vem essa probabilidade
Um tokenizador transforma o prompt em IDs de token. O modelo então produz um vetor para a próxima posição, contendo um logit, uma pontuação bruta, para cada token do vocabulário, ou seja, dezenas de milhares de números. A geração comum transforma esses logits em probabilidades com softmax sobre todo o vocabulário, depois de dividi-los por uma temperatura que afia ou achata a distribuição. A geração repete esse ciclo, acrescentando um token por vez, até que um token de parada ou um limite de tamanho a encerre.

> 🔎 Contexto adicionado: Softmax converte uma lista de pontuações brutas em números positivos que somam 1. Uma temperatura abaixo de 1 afia a distribuição, e uma temperatura acima de 1 a achata.

A pontuação restrita precisa apenas do primeiro vetor, e apenas três de suas entradas. O autor rotula as respostas como A, B e C no prompt e interrompe o prompt onde o modelo começaria a escrever o rótulo. O modelo ainda produz seu vetor habitual, mas o código mantém apenas os logits de A, B e C e aplica softmax a esses três. O denominador encolhe do vocabulário inteiro para os três rótulos declarados.

### Softmax restrito
O softmax restrito é a mesma fórmula de softmax com o denominador limitado às opções declaradas. Ele responde como o modelo divide sua preferência apenas entre as escolhas que o código permitiu, não qual é a probabilidade em todo o vocabulário. Com logits A = 6.8, B = 4.3 e C = 3.1, o softmax restrito dá billing 0.90, technical 0.07 e account 0.02. O artigo descreve isso como trocar uma soma sobre cerca de 150.000 termos por uma soma sobre 3.

### Rótulos de uma letra
Os rótulos são de uma letra porque uma palavra pode ocupar vários tokens. A palavra "billing" pode ser um token para um tokenizador e vários para outro, e "technical support" normalmente ocupará mais de uma posição. Uma única letra sempre ocupa exatamente uma posição de saída, e seu significado está no texto do prompt ao lado dela.

> ❌ **Anti-padrão — rótulos de múltiplos tokens:** Escrever os nomes completos das opções como resposta pontuada. **Por que as pessoas fazem isso:** As palavras soam naturais. **Consequência:** A contagem de tokens muda conforme o tokenizador, então a posição pontuada não é fixa. **Correção:** Use rótulos de um único token, como letras, e descreva cada um no prompt.

### Armadilha do espaço inicial
Tokenizadores costumam incorporar um espaço inicial a um token, então `A` e ` A` podem mapear para dois IDs de token diferentes. Um template de chat pode inserir esse espaço em branco sem que o desenvolvedor perceba. A correção é renderizar o prompt completo pelo próprio template de chat do modelo, verificar a continuação exata que ele espera e confirmar que cada rótulo é um único token antes de confiar em uma chamada de pontuação.

> ❌ **Anti-padrão — tokens de rótulo não verificados:** Presumir o mapeamento rótulo-para-token sem checá-lo contra o template de chat real. **Por que as pessoas fazem isso:** O mapeamento parece óbvio. **Consequência:** As pontuações são lidas nas posições erradas e parecem plausíveis mesmo assim. **Correção:** Tokenize cada rótulo pelo modelo alvo e falhe se algum rótulo não for um único token.

### Escolhas exaustivas
Se as escolhas declaradas não forem exaustivas, o softmax restrito ainda empurra toda a massa de probabilidade para a resposta declarada mais próxima. Um incidente de segurança enviado a um roteador que conhece apenas billing, technical e account é arquivado com confiança em uma dessas três. Adicionar uma opção OTHER ou ESCALATE é um seguro barato.

> ❌ **Anti-padrão — lista fechada de escolhas sem saída:** Oferecer apenas as respostas que você espera. **Por que as pessoas fazem isso:** Adicionar uma opção de escape parece trabalho extra. **Consequência:** Entradas fora do escopo são atribuídas com confiança à categoria errada. **Correção:** Sempre inclua uma opção OTHER ou ESCALATE.

## Vamos construir o mesmo truque por conta própria
O Jev é fechado, mas a mesma técnica de pontuação já roda em stacks de serving abertos. O autor a reproduz com SGLang como servidor e Qwen2.5-0.5B-Instruct como modelo, construindo um cliente chamado `decide.py` um bloco por vez. O servidor do modelo faz todo o trabalho da GPU, e o cliente se comunica com ele via HTTP.

### Iniciando o servidor do modelo
Crie um ambiente limpo com dois pacotes e depois inicie o servidor do modelo. A primeira execução baixa o modelo do Hugging Face e o mantém em memória na porta 30000 para requisições posteriores.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install "sglang[all]==0.5.10.post1" "requests==2.34.2"
```

**O que isso faz:**
- Cria e ativa um ambiente Python isolado.
- Fixa SGLang em 0.5.10.post1 e requests em 2.34.2, para que a execução seja reproduzível.

```bash
python -m sglang.launch_server \
    --model-path Qwen/Qwen2.5-0.5B-Instruct \
    --host 127.0.0.1 \
    --port 30000
```

**O que isso faz:**
- Inicia o servidor SGLang, o único processo que faz trabalho de GPU.
- Serve o modelo Qwen2.5-0.5B-Instruct em 127.0.0.1, porta 30000.

### Definindo as escolhas e construindo o prompt
O primeiro bloco do cliente define os rótulos e o chamado. A ordem do dicionário precisa sobreviver à tokenização, à pontuação e ao mapeamento de volta, então o prompt é construído a partir do dicionário em vez de redigitar os rótulos.

```py
# decide.py
import json
import requests

BASE_URL = "http://127.0.0.1:30000"
MODEL = "Qwen/Qwen2.5-0.5B-Instruct"

# A is the token I actually score, "billing and payments" is what my
# application does with the answer once it comes back
choices = {
    "A": "billing and payments",
    "B": "technical support",
    "C": "account access",
}
ticket = "I was charged twice for the same subscription."
```

**O que isso faz:**
- Mantém o mapeamento rótulo-significado em um único dicionário que todas as etapas seguintes leem.
- Separa o rótulo pontuado (`A`) do significado de aplicação ao qual ele corresponde.

```py
# decide.py (continued)
choice_lines = "\n".join(
    f"{label} = {meaning}" for label, meaning in choices.items()
)
prompt = f"""Ticket:
{ticket}
Question:
Which category matches the ticket?
Allowed labels:
{choice_lines}
Return only the label.
Label:
"""
print(prompt)
```

**O que isso faz:**
- Gera as linhas de rótulos permitidos a partir do dicionário, para que os rótulos nunca sejam digitados duas vezes.
- Termina o prompt em `Label:`, a posição cujo vetor do vocabulário será lido. O modelo nunca é solicitado a gerar o token que vem depois.

```javascript
#### OUTPUT ####
Ticket:
I was charged twice for the same subscription.
Question:
Which category matches the ticket?
Allowed labels:
A = billing and payments
B = technical support
C = account access
Return only the label.
Label:
```

**O que isso faz:**
- Mostra o prompt exato que o modelo recebe.
- Confirma que o prompt para em `Label:`.

### Resolvendo os IDs de token dos rótulos
Antes de pontuar, o cliente busca o ID de token real de cada rótulo sob este tokenizador, em vez de presumi-lo a partir de outro modelo.

```py
# decide.py (continued)
label_token_ids = []

for label in choices:
    response = requests.post(
        f"{BASE_URL}/tokenize",
        json={
            "model": MODEL,
            "prompt": label,
            "add_special_tokens": False,
        },
        timeout=30,
    )
    response.raise_for_status()
    token_ids = response.json()["tokens"]
    if len(token_ids) != 1:
        raise ValueError(f"{label!r} is not a single token: {token_ids}")
    print(f"{label!r} -> {token_ids}")
    label_token_ids.append(token_ids[0])
```

**O que isso faz:**
- Tokeniza cada rótulo pelo servidor e mantém os IDs resultantes na mesma ordem.
- Lança um erro se algum rótulo tiver mais de um token, para que um rótulo ruim interrompa a execução antes da pontuação.

```javascript
#### OUTPUT ####
'A' -> [32]
'B' -> [33]
'C' -> [34]
```

**O que isso faz:**
- Mostra que cada rótulo mapeia para exatamente um ID de token: 32, 33 e 34.
- Confirma que cada opção ocupa uma única posição do vocabulário.

### Pontuando as três posições
Com os IDs confirmados, a decisão é uma única chamada HTTP.

```py
# decide.py (continued)
response = requests.post(
    f"{BASE_URL}/v1/score",
    json={
        "model": MODEL,
        "query": prompt,
        "items": [""],
        "label_token_ids": label_token_ids,
        "apply_softmax": True,
    },
    timeout=120,
)

response.raise_for_status()
score_response = response.json()

print(json.dumps(score_response, indent=2))

scores = score_response["scores"][0]
```

**O que isso faz:**
- Envia o prompt completo como `query`, com uma string vazia em `items` para que a posição logo após o prompt seja pontuada.
- Nomeia as três posições de rótulo a serem lidas e define `apply_softmax` para que as leituras se tornem probabilidades.

```javascript
#### OUTPUT ####
{
  "scores": [
    [
      0.67776233,
      0.310878605,
      0.011359035
    ]
  ]
}
```

**O que isso faz:**
- Retorna três probabilidades na mesma ordem em que os IDs de token foram passados.
- Mostra que nenhum token foi gerado para produzi-las.

Os logits brutos antes do softmax foram 25.2776, 24.4982 e 21.1888 para A, B e C. Aplicando o softmax restrito à mão, obtém-se o mesmo 0.678 que o SGLang imprimiu. O cálculo é a fórmula da seção de mecanismo, executada uma vez sobre três números.

### Mapeando as pontuações de volta a uma decisão
A última etapa é Python simples que transforma três floats de volta nos rótulos que a aplicação usa.

```py
# decide.py (continued)
probabilities = {
    choices[label]: float(score)
    for label, score in zip(choices, scores, strict=True)
}

decision = max(probabilities, key=probabilities.get)

print(json.dumps({"decision": decision, "probabilities": probabilities}, indent=2))
```

**O que isso faz:**
- Emparelha cada pontuação com seu significado de aplicação, usando `strict=True` para que uma incompatibilidade de tamanho gere um erro.
- Escolhe a opção de maior probabilidade como decisão e imprime tanto a decisão quanto a distribuição completa.

```javascript
#### OUTPUT ####
{
  "decision": "billing and payments",
  "probabilities": {
    "billing and payments": 0.6777623295783997,
    "technical support": 0.31087860465049744,
    "account access": 0.011359035037457943
  }
}
```

**O que isso faz:**
- Mostra "billing and payments" como decisão, com 0.678.
- Mostra technical support com 0.311 e account access com 0.011, que juntos formam o restante da distribuição.

A cobrança vence, mas com 0.678, e não com os 0.99 ou mais que o autor talvez esperasse. Esse é o tipo de caso que um limiar de 0.70 deve encaminhar para uma segunda análise em vez de automatizá-lo.

### Velocidade medida
O autor executou o caminho de pontuação e um caminho padrão de chat completion no mesmo servidor SGLang, com o mesmo modelo e os mesmos 100 casos de chamados de suporte, iniciados no mesmo instante. Quando a pontuação terminou todos os 100 casos, a geração padrão, que decodifica até 32 tokens por resposta, tinha concluído 54. A pontuação teve média de 463 milissegundos por caso contra 848 milissegundos para a geração, uma diferença medida de 1.83x.

## Por que o número ao lado da resposta importa mais do que a resposta
Uma resposta tipada diz o que venceu, e a probabilidade diz quão apertada foi a disputa. O segundo número é aquele no qual o código deve ramificar.

```json
{
  "choice": "billing",
  "probabilities": {"billing": 0.52, "technical": 0.46, "account": 0.02},
  "confidence": 0.06
}
```

**O que isso faz:**
- Informa billing como escolha, com sua probabilidade de 0.52.
- Acrescenta uma confiança de 0.06, que é a diferença entre billing e technical.

O artigo define confiança como a margem entre a resposta principal e seu concorrente mais próximo, não apenas a probabilidade vencedora. Billing com 0.52 contra technical com 0.46 é, na prática, cara ou coroa, então este chamado não deve seguir o mesmo ramo de um vencedor com 0.91.

```py
if probabilities["billing"] > 0.9:
    route_automatically(ticket, team="billing")
elif confidence < 0.3:
    send_to_human_review(ticket)
else:
    escalate_to_larger_model(ticket)
```

**O que isso faz:**
- Automatiza apenas quando a probabilidade de billing está acima de 0.9.
- Envia casos de baixa confiança para um humano, e tudo o que fica no meio para um modelo maior.

A regra de roteamento tem dois limiares, e eles vivem no código, onde podem ser revisados, testados e alterados, não dentro dos pesos do modelo. Um rótulo de dashboard pode tolerar uma previsão instável, mas um comando que apaga dados de clientes exige uma barra muito mais alta.

> ❌ **Anti-padrão — automatizar apenas com base no rótulo vencedor:** Rotear um chamado porque seu rótulo venceu, independentemente da margem. **Por que as pessoas fazem isso:** O rótulo é a única saída que a maior parte do código lê. **Consequência:** Uma vitória de 0.52 é automatizada da mesma forma que uma vitória de 0.91. **Correção:** Ramifique com base em probabilidade e margem ao mesmo tempo.

### Calibração
A TypeSafe treina o Jev com o que chama de Reinforcement Learning for Calibrated Decisions, RLCD. O objetivo é a calibração, e não apenas a acurácia: previsões feitas com 90 por cento de confiança devem estar certas cerca de 90 por cento das vezes. Para verificar isso, agrupe cada previsão pela confiança declarada e compare a confiança média de cada grupo com a frequência com que o grupo acertou. Um modelo pode ser preciso em média e, ainda assim, estar confiantemente errado na faixa em que você mais confiaria. Verificar a calibração exige um conjunto de avaliação rotulado próprio.

## A alegação de alucinação, dita com mais cuidado
A TypeSafe diz que o Jev não pode alucinar. Isso vale sob uma definição estreita. Se as respostas declaradas são billing, technical e account, uma resposta jurídica não pode aparecer, porque essa opção não existe no schema. Prosa malformada também fica descartada. O schema, porém, não diz nada sobre se uma resposta válida está correta. Um bug de login classificado como billing é válido pelo schema e ainda assim errado, e um reembolso pode ser emitido sobre um problema que nunca foi uma cobrança.

> ❌ **Anti-padrão — equiparar validade de schema a correção:** Ler "não pode alucinar" como "sempre está certo". **Por que as pessoas fazem isso:** A garantia soa absoluta. **Consequência:** Decisões erradas, mas bem formadas, fluem direto para ações. **Correção:** Meça a correção em um conjunto rotulado e controle ações de alto impacto pela confiança.

Uma afirmação mais cuidadosa é que o Jev não pode quebrar seu schema de saída declarado, mas ainda pode errar. Garantia de tipo e julgamento correto são coisas separadas, e apenas a primeira vem de graça. O artigo sugere colocar pontos de verificação do Jev ao lado de um loop de agente comum em três momentos: escolher o modelo, aprovar uma ferramenta e verificar o resultado.

## Onde o Jev se justifica dentro de um agente
O Jev funciona ao lado de um modelo generativo, não no lugar dele. O modelo generativo planeja, escreve, explica e chama ferramentas. O Jev cuida das decisões pequenas e frequentes ao redor desse trabalho.

### Roteamento de modelos
Uma consulta simples não precisa do modelo usado para uma revisão de arquitetura difícil. O Jev pontua a requisição recebida e escolhe o modelo menos caro com probabilidade de concluí-la. O roteador decide qual modelo responde, não o que a resposta diz.

### Controle de risco de ferramentas
Antes de um agente executar um comando de shell, o Jev pode classificá-lo como somente leitura, reversível ou destrutivo, e também dizer se ele toca em produção ou sai do sandbox. Um comando somente leitura com alta confiança segue executando, enquanto um comando destrutivo ou incerto pausa para um humano. Um comando de shell gera uma chamada Choice, e as três classes de risco levam a três consequências diferentes.

### Verificação
Um agente pode afirmar que uma tarefa está pronta enquanto seus testes ainda falham. O Jev pode responder perguntas limitadas sobre o estado, como se os testes passaram, se o agente está repetindo uma ação que falhou ou se a saída segue a política. Testes rígidos continuam pegando o que foram feitos para pegar, e o Jev acrescenta uma verificação semântica para o que uma regra rígida não consegue expressar.

## Problemas que o Jev pode resolver hoje
O artigo afirma que cargas de trabalho adequadas ao Jev têm três propriedades: todas as respostas possíveis podem ser nomeadas antecipadamente, um revisor cuidadoso poderia julgar a entrada rapidamente, e a decisão acontece com frequência suficiente para que latência ou custo importem.

### Suporte e operações
Classifique intenção, urgência, departamento, spam e frustração em uma única passagem. Encaminhe reembolsos por meio de várias verificações pequenas em vez de um prompt grande. Ordene incidentes por severidade antes que um humano os leia.

### Busca e recuperação
Reordene trechos por responderem à consulta, e não apenas por proximidade semântica. Verifique se uma citação sustenta uma afirmação. Filtre chunks irrelevantes antes que cheguem a um modelo caro.

### Qualidade e segurança
Analise prompts em busca de tentativas de jailbreak ou injeção, verifique conteúdo gerado contra uma política escrita e sinalize uma mudança de código arriscada ou uma chamada de ferramenta antes que ela rode. Essas verificações ficam ao lado de verificações determinísticas e não as substituem.

### Classificação em alto volume
Rotule documentos ou mensagens de clientes em uma escala antes inviável financeiramente. Transforme texto livre em features para um modelo de ML tradicional. Pontue cada linha de um corpus com a mesma rubrica.

### Interfaces em tempo real
Escolha a próxima ação no navegador a partir de um conjunto conhecido de elementos da página, ou avalie o tom enquanto alguém ainda está digitando. O Jev é apenas de texto hoje, então o ambiente precisa primeiro ser transformado em texto.

## Isso se sustenta?
O artigo trata os números da própria TypeSafe com ceticismo. A TypeSafe informa latência de 70 a 500 milissegundos, $0.042 por milhão de tokens de entrada com a saída gratuita, e alega cerca de 200 vezes mais rápido e 400 vezes mais barato que fluxos comparáveis. O autor trata esses valores como um teto, não como promessa. O mecanismo é real, como a reprodução acima mostra, mas os múltiplos de marketing ficam no extremo favorável.

O autor prefere uma comparação independente. A LangChain testou o Jev contra três juízes LLM, GPT-5.6 Luna, GPT-5.6 Terra e Claude Sonnet 4.6, usando seu próprio harness Deep Agents e o LangSmith para o dataset.

Um exemplo mostra a diferença de mecanismo. Diante da mesma pergunta sobre política de reembolso, um juiz LLM raciocina por um parágrafo antes de produzir uma pontuação de 0.67. O juiz observou que a resposta dá uma janela de 30 dias, que corresponde à política, mas deixa de fora a exceção para itens abertos, então ela está correta, mas não está completa. Um juiz no estilo Jev pula o parágrafo e retorna três probabilidades calibradas diretamente: correta 0.98, fundamentada 0.92 e completa 0.11. Elas se combinam no mesmo 0.67.

### Acurácia contra um oráculo humano
Em 500 julgamentos repetidos de aprovado/reprovado contra um oráculo humano, o Jev concordou com o humano em 100.0 por cento dos casos, o GPT-5.6 Terra em 99.8 por cento, o GPT-5.6 Luna em 96.4 por cento e o Claude Sonnet 4.6 em 80.0 por cento.

### Variância
A acurácia sozinha conta só metade da história, já que um juiz pode acertar em média e ainda oscilar de uma execução para outra. Em 100 repetições sobre um único caso congelado, a variância média do Jev foi 0.0000149. A do Claude Sonnet 4.6 foi 92 vezes maior, a do GPT-5.6 Luna 433 vezes maior e a do GPT-5.6 Terra 913 vezes maior. Baixa variância não prova correção, já que um juiz pode estar consistentemente errado, mas um juiz preciso com baixa variância é algo sobre o qual vale construir automação.

### Custo e latência
O Jev teve média de $0.00035 e 0.44 segundos por chamada. O GPT-5.6 Luna teve média de $0.00039 e 2.50 segundos, o GPT-5.6 Terra $0.00289 e 2.83 segundos, e o Claude Sonnet 4.6 $0.02811 e 2.16 segundos. Projetando 10,000 traces por dia durante 30 dias, os totais são $103.59 para o Jev, $117.24 para o GPT-5.6 Luna, $866.61 para o GPT-5.6 Terra e $8,434.08 para o Claude Sonnet 4.6.

### Valor do sinal
O valor do sinal é a concordância com o oráculo multiplicada pela repetibilidade, ou seja, a chance de duas chamadas independentes sobre o mesmo trace congelado chegarem ao mesmo veredito. Um juiz só ganha crédito se for correto e repetível ao mesmo tempo. O Jev marcou 100.0 por cento, o GPT-5.6 Terra 99.4 por cento, o GPT-5.6 Luna 90.1 por cento e o Claude Sonnet 4.6 80.0 por cento.

O experimento usou um agente, cinco casos de teste e uma fatia de avaliação estreita, então o autor não o trata como a palavra final. É, porém, uma medição independente, e aponta na direção que o mecanismo prevê.

## Onde o Jev deixa a desejar
O Jev é menos útil quando o espaço de respostas não pode ser conhecido antecipadamente.
- Ele não consegue escrever uma resposta, resumir um documento, gerar código ou explicar seu raciocínio.
- É pouco confiável em aritmética, contagem, comparação de datas ou manipulação exata de strings, então mantenha isso em código comum.
- Tem dificuldades quando uma decisão exige várias etapas de raciocínio ocultas. Divida o julgamento em perguntas menores ou use um modelo de raciocínio.
- Não consegue extrair um valor desconhecido de um texto. Encontre candidatos com outro método primeiro, e então deixe o Jev escolher entre eles.
- Contexto irrelevante pode reduzir sua acurácia, então envie apenas o estado de que a decisão precisa.
- Pesos fechados, uma janela de acesso antecipado, entrada somente de texto e dados de calibração independentes escassos tornam cedo demais confiar nele cegamente em decisões com consequências reais.

A síntese do artigo: respostas limitadas, de alto volume, são o ponto forte do Jev, trabalho aberto ainda pertence a um LLM, e decisões limitadas e raras pertencem a um simples if. Se código determinístico já resolve o problema corretamente, mantenha o código. Um if simples é mais rápido, mais barato e mais fácil de testar do que qualquer modelo.

## Uma forma sensata de implantar isso
Um modelo barato ainda pode ficar caro se seus erros gerarem novas tentativas, revisão manual ou um incidente, então a implantação deve medir o fluxo de trabalho inteiro, não apenas o preço por token.

> 📋 **Checklist — Implantando uma decisão pontuada**
> - [ ] Escolha uma decisão limitada e de baixo risco, com um conjunto pequeno e claramente nomeado de respostas possíveis, não a etapa mais arriscada do pipeline.
> - [ ] Escreva a rubrica antes de chamar o modelo. Decida em palavras o que pertence a cada opção. A rubrica faz parte do sistema.
> - [ ] Colete primeiro exemplos representativos, incluindo casos deliberadamente ambíguos e adversariais.
> - [ ] Rode em modo sombra. Deixe o modelo responder a cada caso real sem alterar nada, e registre suas probabilidades ao lado do resultado atual.
> - [ ] Plote acurácia contra confiança e defina os limiares a partir desses dados. O corte deve vir dos números do modo sombra, não de um chute.
> - [ ] Automatize primeiro o ramo mais seguro. Mantenha um humano, ou um modelo mais forte, como fallback para tudo o que os limiares sinalizarem como incerto.
> - [ ] Versione tudo. Fixe a versão do modelo, as perguntas, os critérios e os limiares juntos, para que uma mudança possa ser reproduzida sobre o mesmo conjunto de avaliação.

O artigo diz que o passo cinco faz trabalho real. Mire no limiar mais alto que ainda automatize volume suficiente para valer a pena construir. Um limiar alto demais faz o sistema nunca decidir nada, o que o autor chama de log caro em vez de automação. A restrição de cobertura mantém o limiar honesto.

## O que está comprovado aqui
O artigo considera o mecanismo comprovado, já que o autor o construiu e o executou, com IDs de token reais e um softmax restrito real produzindo 0.678. Os números de avaliação independente também são reais, publicados por uma parte que não ganha nada ao fazer o Jev parecer melhor. Permanecem não comprovados:
- Os pesos do Jev são fechados, então o mecanismo verificado é uma reconstrução fiel em um modelo aberto, não uma inspeção do Jev.
- As alegações de latência e custo da TypeSafe ficam na borda favorável das próprias comparações.
- Dados de calibração independentes além da única avaliação citada ainda são escassos.
- O acesso é antecipado e a interface é somente de texto, então imagens, UIs e dados brutos de sensores precisam de uma etapa separada para virar texto.

Nada disso torna a ideia errada. Torna as alegações em torno dela precisas, e não resolvidas.

## Onde isso nos deixa
O artigo argumenta que o Jev é interessante não porque escreve melhor que um modelo de linguagem, mas porque não escreve de jeito nenhum. Seus tipos de resposta são fixos, a incerteza é explícita em cada resposta, cada pergunta é avaliada em paralelo, e a lógica de ramificação permanece no código. O modelo generativo ainda produz planos, explicações e código. O Jev roteia requisições, controla ações arriscadas, verifica resultados e sinaliza quando a incerteza é alta o bastante para que um humano olhe.

O conselho do autor: não comece reconstruindo um agente em torno de um modelo de decisão tipado. Encontre um lugar onde o código paga por uma chamada generativa completa apenas para obter uma resposta de uma palavra, dê a ela o estado mínimo de que precisa, e deixe-a ganhar um ramo antes que mais qualquer coisa lhe seja entregue.

## 🧠 Modelos mentais
### Softmax restrito
**O modelo:** Leia a preferência do modelo apenas entre as opções que seu código declarou, aplicando softmax aos logits dessas opções e ignorando o restante do vocabulário.
**Como aplicar:** Rotule cada opção com um rótulo de um token, verifique os IDs de token e sempre inclua uma opção OTHER para que a massa de probabilidade tenha para onde ir.

### Margem como confiança
**O modelo:** A confiança é a diferença entre a opção vencedora e seu concorrente mais próximo, não apenas a probabilidade vencedora.
**Como aplicar:** Ramifique com base na probabilidade e na margem juntas. Uma vitória de 0.52 sobre 0.46 deve escalar, não automatizar.

### Schema garante pertencimento, não correção
**O modelo:** Uma saída tipada só pode conter valores declarados, mas qualquer valor declarado pode ser o errado.
**Como aplicar:** Meça a acurácia em um conjunto de avaliação rotulado antes de automatizar, e mantenha ações de alto impacto atrás de um humano ou de uma verificação mais forte.

### Pontuação como média ponderada
**O modelo:** Uma pontuação em uma escala ordenada é a média ponderada pelas probabilidades de cada degrau, então 1.74 em uma escala de não resolvido a resolvido carrega informação que um único rótulo descarta.
**Como aplicar:** Mantenha a distribuição completa, não apenas o rótulo principal, quando uma regra posterior precisar saber quão apertada foi a decisão.

### If primeiro
**O modelo:** Quando código determinístico já resolve corretamente um problema limitado, esse código é mais barato, mais rápido e mais fácil de testar do que qualquer modelo.
**Como aplicar:** Antes de adicionar uma chamada de modelo, verifique se uma regra ou comparação comum dá a mesma resposta.

## 🔑 Principais conclusões
- Pontuar um conjunto conhecido de opções em uma única passagem direta evita a geração token a token que um julgamento de uma palavra não precisa. O autor mediu 463 ms por caso para a pontuação contra 848 ms para a geração, uma diferença de 1.83x, no mesmo hardware.
- A probabilidade reportada pelo softmax restrito é a divisão do modelo apenas entre os rótulos declarados, então cada rótulo deve ser um único token verificado e as escolhas devem incluir uma opção de escape.
- Limiares pertencem ao código. A regra de roteamento usa um limiar de automação de 0.9 e um limiar de confiança de 0.3, e ambos podem ser revisados e testados.
- O schema do Jev garante que suas respostas são membros do conjunto declarado, não que estão corretas, então a acurácia deve ser medida separadamente.
- No teste independente da LangChain, em uma fatia estreita de um agente e cinco casos de teste, o Jev concordou com o oráculo humano em 100.0 por cento de 500 julgamentos, contra 80.0 por cento do Claude Sonnet 4.6, a $0.00035 por chamada contra $0.02811.
- Antes de automatizar, defina limiares a partir de dados de modo sombra, mantenha humanos ou um modelo mais forte para casos incertos, e fixe modelo, perguntas, critérios e limiares juntos.

## 🏛️ Para o Kenji
> Sua interpretação, não a do autor: No trabalho de um arquiteto, este padrão se encaixa em qualquer lugar em que uma decisão de controle ou roteamento recorra a um modelo de linguagem para um rótulo limitado, como verificações de governança de IA, roteamento de requisições em um gateway de IA, ou o controle de risco de ferramentas em plataformas de agentes. A questão que isso levanta é como você governaria os limiares e o conjunto de avaliação rotulado que justificam a automação, e quem é dono dessa evidência quando o modelo é atualizado.

## 📚 Plano de leitura
### Do artigo
- [decide.py](http://decide.py) — O script cliente do autor, usado na reprodução. O link aponta para um nome de arquivo, não para uma referência externa.
### Para aprofundar
- A avaliação da LangChain sobre o Jev contra juízes LLM — a comparação independente que o artigo usa para seus números de acurácia, variância e custo.
- SGLang e seu endpoint `/v1/score` — a stack de serving aberta que o autor usou para reproduzir a pontuação restrita.
- Calibração e expected calibration error — o método de agrupar previsões por confiança e comparar cada grupo com sua acurácia.
- Saída estruturada versus pontuação de respostas fixas — o contraste entre decodificação restrita por schema e a leitura de uma única passagem direta.

## Conceitos
- [structured-decision-model](/pt-br/concepts/ai-ml/structured-decision-model.md)
- [calibration](/pt-br/concepts/ai-ml/calibration.md)
- [tokenization](/pt-br/concepts/ai-ml/tokenization.md)
- [large-language-model](/pt-br/concepts/ai-ml/large-language-model.md)
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [llm-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md)
- [inference-tradeoffs](/pt-br/concepts/ai-ml/inference-tradeoffs.md)
- [model-evaluation](/pt-br/concepts/ai-ml/model-evaluation.md)
- [logit](/pt-br/concepts/ai-ml/logit.md)
