---
type: Article Summary
title: Testei o Jev, um modelo de decisão 478× mais barato da TypeSafe, contra Claude, GPT-6, Kimi, MiniMax e DeepSeek
description: Um modelo de decisão especialista iguala LLMs de fronteira na maioria das decisões tipadas, por uma fração do custo, mas erra na avaliação de ambiguidade.
resource: https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
- calibration
- structured-decision-model
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
  title: I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek
  author: Manjunath Janardhan
status: stable
source_url: https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
source: Data Science Collective
source_type: medium
published: '2026-09-19'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
- calibration
- structured-decision-model
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81cc90d8e9c155504851
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-i-tested-typesafe-s-jev-a-478-cheaper-decision-model-against.md
---

# Testei o Jev, um Modelo de Decisão 478× Mais Barato da TypeSafe, Contra Claude, GPT-6, Kimi, MiniMax e DeepSeek

**Fonte:** https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
**Autor:** Manjunath Janardhan
**Data:** 2026-09-19
**Conceitos:** [Throughput/latência/custo de inferência](/pt-br/concepts/inference-tradeoffs.md) · [Calibração](/pt-br/concepts/calibration.md) · [Modelo de decisão estruturado](/pt-br/concepts/structured-decision-model.md)

---

## 🎯 Tese central
Um modelo pequeno e de finalidade única, o "System One", consegue responder perguntas de decisão tipadas quase tão bem quanto LLMs de fronteira, por uma fração do custo e da latência. Mas só o modelo mais caro o supera com clareza, e só as probabilidades desse modelo acompanham a discordância humana genuína.

## 🧭 Por que o autor escreveu isto
Grande parte do que uma aplicação de IA faz é decidir (rotear um ticket, marcar spam, avaliar uma resposta, julgar o tom), e cada decisão é uma pequena questão de múltipla escolha que as equipes normalmente enviam a um LLM de chat generalista. O autor testa se um modelo feito apenas para essas perguntas é bom o suficiente, e se suas probabilidades significam algo nos itens em que as próprias pessoas discordam. Se o Jev se sustentar, a escolha padrão para decisões de alto volume muda de um modelo topo de linha para um especialista rápido.

## O que é o Jev, em uma chamada
O Jev não é um modelo de chat e não gera texto. Você envia uma entrada (o "state") e uma ou mais perguntas tipadas: um `choice` a partir de uma lista, um `score` em uma escala ordenada ou um `noul` (a probabilidade de algo ser verdadeiro). Ele devolve uma probabilidade para cada opção. A TypeSafe lançou o modelo em 15 de setembro de 2026 e o chama de modelo "System One", em referência à metade rápida e intuitiva do modelo de pensamento de Kahneman. No OpenRouter, custa $0.042 por milhão de tokens de entrada, e a saída é gratuita. O autor pagou $0.025 por mil decisões no teste, e o teste inteiro custou $4.83.

```py
import json, os, urllib.request

body = {
    "model": "typesafe/jev-1.13",
    "state": {"message": "I was charged twice for the same coffee"},
    "questions": {
        "team": {
            "type": "choice",
            "instructions": "Which team should handle `message`?",
            "criteria": {"payments": "Charges, refunds, payouts",
                         "cards": "Card delivery, activation, PINs",
                         "other": None},
        }
    },
}
req = urllib.request.Request(
    "https://openrouter.ai/api/alpha/decisions",
    data=json.dumps(body).encode(),
    headers={"Authorization": f"Bearer {os.environ['OPENROUTER_API_KEY']}",
             "Content-Type": "application/json"},
)
print(json.load(urllib.request.urlopen(req))["answers"]["team"])
# {"type": "choice", "choice": "payments",
#  "probabilities": {"cards": 0, "payments": 1, "other": 0}, "confidence": 1}
```

**O que isso faz:**
- O `state` contém a entrada a ser julgada, e `questions` contém uma ou mais perguntas tipadas sobre ela, de modo que a requisição é uma decisão, e não um prompt.
- A pergunta `choice` lista suas opções como `criteria`, com `other` como alternativa; o modelo só pode responder a partir dessa lista.
- A chamada vai para o endpoint de decisões do próprio OpenRouter, e não para o endpoint de chat completions, porque o Jev não é um modelo de chat.
- A resposta devolve `probabilities` para cada opção, de modo que quem chama pode aplicar limiares ou escalar com base nelas; a probabilidade de 1 para `payments` no exemplo é a confiança do próprio modelo.
- A requisição usa apenas a biblioteca padrão, o que mantém o exemplo sem dependência de cliente.

### Pergunta de decisão tipada
Uma pergunta tipada diz ao modelo que tipo de resposta devolver, então a saída é um conjunto fechado, e não texto livre. Os três tipos são `choice`, `score` e `noul`. É isso que separa o modelo de um LLM de chat: o espaço de respostas é fixado antecipadamente, então o modelo pode devolver uma probabilidade para cada opção em vez de uma frase.

### Saída de probabilidade
Cada resposta traz um mapa `probabilities` sobre as opções, e o maior valor é a confiança do próprio modelo em sua escolha. O artigo deixa claro que o campo `confidence` separado do Jev é uma estatística resumida, e não uma probabilidade, então o autor usa a maior probabilidade. Várias perguntas sobre a mesma entrada podem ser feitas em uma única chamada e são respondidas em paralelo.

> ❌ **Anti-padrão — enviar decisões a um LLM de chat por padrão:** usar um modelo generalista, feito para escrever ensaios, em portões de sim/não, roteamento e classificação. **Por que as pessoas fazem isso:** é a ferramenta que já está à mão e funciona, então ninguém mede a alternativa. **Consequência:** alto custo por chamada e esperas de vários segundos em decisões que um especialista responde em menos de meio segundo. **Correção:** faça um benchmark de um modelo System One com o seu próprio conjunto de decisões antes de adotar um topo de linha como padrão.

### Calibração
Calibração pergunta se as probabilidades declaradas por um modelo correspondem à frequência com que ele realmente acerta. O autor relata um erro de calibração do Jev de 0.13 nas três tarefas padrão e de 0.25 na tarefa ambígua, contra 0.07 nas tarefas padrão em outro benchmark independente. Um teste fora da distribuição também constatou que as respostas de escolha do Jev são excessivamente confiantes. Trate a calibração como algo a ser medido nos seus próprios dados, e não como algo a ser presumido.

## O teste
Seis modelos foram comparados: Jev, Claude Fable 5.1, GPT-6 Astra, Kimi K3, MiniMax M3 e DeepSeek V4.1 Flash. Havia quatro tarefas de 50 itens cada, extraídas de conjuntos de dados públicos com respostas conhecidas: perguntas de sim/não sobre um trecho, roteamento entre 77 intenções bancárias mais "other" (78 opções), avaliações por estrelas e a tarefa de ambiguidade ChaosNLI. Isso dá 200 itens no total.

As regras de equidade importam. Todos os modelos viram a mesma entrada, a mesma pergunta e as mesmas opções. Os LLMs tinham de responder em JSON estrito, que permitia apenas as opções listadas e dava uma probabilidade para cada uma. Na tarefa de roteamento com 78 opções, os LLMs devolveram uma resposta e uma confiança, porque a API da Anthropic recusou o formato maior e ele custaria seis vezes mais. Os LLMs rodaram com esforço de raciocínio "medium". O Jev seguiu a orientação da própria TypeSafe (a lista completa de intenções e perguntas que nomeiam o campo ao qual se referem). Cada modelo de pesos abertos foi fixado a um único host, para que a velocidade não dependesse de qual servidor o OpenRouter escolhesse. Cada chamada foi registrada, e todos os números vêm desses registros.

### Ambiguidade com rótulos humanos
Alguns itens não têm uma única resposta certa. O ChaosNLI rotula cada par de sentenças com 100 julgamentos humanos, e em muitos pares esses julgamentos se dividem. Um bom sistema de decisão nessa situação deveria expressar a divisão, e não escolher um rótulo com alta confiança.

## Resultado 1: só os dois modelos mais caros o superam com clareza
Comparados item a item, o Claude Fable 5.1 supera o Jev em 11.5 pontos e o GPT-6 Astra em 6.5 pontos, e ambas as diferenças se sustentam estatisticamente. O Kimi K3, o MiniMax M3 e o DeepSeek V4.1 Flash ficam de 2 a 3.5 pontos à frente, o que está dentro do ruído em 200 itens. Esses três custam de 6 a 150 vezes mais que o Jev e levam de 2 a 15 vezes mais tempo. O Claude Fable 5.1 compra 11.5 pontos a mais de precisão por 478 vezes o preço e cerca de 10 vezes a espera. Se isso vale a pena depende do custo de uma resposta errada.

### Comparação item a item
Comparar modelos item a item, nos mesmos 200 itens, é mais sensível do que comparar a precisão geral, porque remove a dificuldade dos itens da comparação. É por isso que o autor pode considerar sólidas as duas grandes diferenças, enquanto as diferenças de 2 a 3.5 pontos não são. O desenho de 50 itens por tarefa ainda limita as conclusões por tarefa.

## Resultado 2: onde ele vence e onde não vence
Em perguntas de sim/não sobre um trecho, o Jev iguala todos os outros modelos do teste. O roteamento entre 77 intenções bancárias de nomes parecidos é mais difícil: o Jev obtém 76%, em linha com os resultados publicados para ele nesse conjunto de dados (78 a 83%, com barras de erro largas). As avaliações por estrelas são sua tarefa mais fraca. Ele costuma ficar a uma estrela da avaliação correta, mas acerta a contagem exata com menos frequência. A leitura prática é que o Jev é mais forte onde as opções são claramente separadas e mais fraco onde elas se sobrepõem.

## Resultado 3: responde em menos de meio segundo
A latência mediana do Jev foi de 0.43 segundo a partir da Índia, via OpenRouter, rede incluída, e seus 5% mais lentos ainda responderam em até 0.6 segundo. O LLM mais rápido, o MiniMax M3, levou cerca de um segundo. Os dois topos de linha levaram 2.2 segundos (Claude Fable 5.1) e 4.3 segundos (GPT-6 Astra). Os 5% mais lentos do DeepSeek passaram de 23 segundos. Para um widget de suporte ou um formulário que se roteia enquanto o usuário digita, essa diferença importa mais do que alguns pontos de precisão.

### Percentis de latência
O autor relata a mediana e os 5% mais lentos (o percentil 95). A latência de cauda é o que um usuário que espera sente nos piores momentos, então a extremidade lenta da distribuição costuma importar mais que a média. Um modelo com mediana rápida e cauda longa, como o DeepSeek aqui, ainda pode parecer pouco confiável em um fluxo interativo.

## Resultado 4: só um modelo sabe quando as pessoas discordam
Este foi o resultado mais surpreendente para o autor, que não encontrou nenhuma medição anterior disso para o Jev. No exemplo do ChaosNLI, os 100 rótulos humanos de um mesmo par de sentenças se dividiram entre as opções, em vez de concordar. O Claude Fable 5.1 ficou em dúvida em proporções praticamente iguais às das pessoas. O Jev e o GPT-6 Astra estavam quase certos, e escolheram a visão minoritária.

### Distância da distribuição de rótulos humanos
Para pontuar isso, o autor mede o quanto o vetor de probabilidades de cada modelo está distante da distribuição de rótulos humanos, em todos os 50 pares. O Claude Fable 5.1 fica cerca de 2.5 vezes mais próximo da divisão humana do que o segundo melhor, o MiniMax M3, e 3.5 vezes mais próximo que o Jev. O ponto desconfortável é que um modelo que responde um terço para cada opção em todos os pares supera o Jev, o GPT-6 Astra e o DeepSeek, e empata com o Kimi. Só o Claude Fable 5.1 e o MiniMax M3 se saem melhor que um palpite cego.

> ❌ **Anti-padrão — um rótulo único e confiante para uma questão de julgamento:** devolver um rótulo com alta confiança quando a pergunta subjacente é disputada. **Por que as pessoas fazem isso:** uma resposta única é mais simples de consumir e de aplicar limiares. **Consequência:** o modelo tem 96% de certeza de um cara ou coroa e falha exatamente onde você gostaria que ele pedisse ajuda. O Jev escolheu "neither" em 29 dos 50 pares, onde a maioria humana escolheu em 19, com confiança média de 83%. **Correção:** mantenha a distribuição de probabilidades completa e encaminhe os itens de alta discordância para revisão ou para um modelo maior.

Se suas decisões incluem julgamentos reais, como casos limítrofes de moderação, "esta resposta é boa o suficiente?" ou tom, esse resultado importa mais do que a precisão de destaque.

## Uma surpresa: os modelos de fronteira quase não raciocinaram
Os LLMs foram instruídos a raciocinar com esforço medium, mas, nessas perguntas curtas, em sua maioria não o fizeram. O Claude Fable 5.1 usou cerca de 2 tokens de raciocínio por pergunta, o GPT-6 Astra cerca de 20, o Kimi K3 cerca de 89 e o DeepSeek V4.1 Flash cerca de 236. Assim, um modelo topo de linha com raciocínio habilitado, em uma pergunta de roteamento, está em grande parte pagando pelo modelo base, e não por pensamento extra. Para decisões curtas, os modelos "System Two" se comportam como modelos System One, apenas mais lentos e mais caros.

> ❌ **Anti-padrão — pagar por raciocínio em decisões curtas:** habilitar um orçamento de raciocínio para uma pergunta de roteamento com um único rótulo e presumir que isso acrescenta qualidade. **Por que as pessoas fazem isso:** o raciocínio é o recurso anunciado de um modelo de fronteira. **Consequência:** custo e latência sobem enquanto o modelo gasta quase nenhum token pensando. **Correção:** meça o uso de tokens de raciocínio nos seus próprios itens e habilite o raciocínio apenas para decisões que exigem trabalho em várias etapas.

## Usando os dois: Jev primeiro, depois escalar
As probabilidades do Jev permitem uma cascata: aceitar a resposta do Jev quando sua maior probabilidade ultrapassa um limiar, e enviar o restante a um modelo maior. Com um limiar de 90%, a cascata chega a 1.5 ponto do Claude Fable 5.1 sozinho por 37% do custo, e 62% das requisições ainda recebem a resposta de meio segundo do Jev. Com um limiar de 70%, ela supera o Kimi K3 em precisão por menos da metade do preço do Kimi. O autor alerta que os limiares foram escolhidos depois de ver os dados, então, no seu próprio tráfego, você deve definir o limiar em uma amostra e verificá-lo em outra.

### Roteamento em cascata
Uma cascata é um pipeline de duas etapas em que um modelo barato e rápido trata os casos fáceis e um modelo mais forte trata os incertos. Sua qualidade depende de o sinal de confiança ser útil para decidir o que escalar, mesmo que os valores absolutos sejam excessivamente confiantes. O argumento do autor é que o ranking que o Jev faz de suas próprias respostas é informativo, mesmo quando suas probabilidades brutas são extremas demais.

> ❌ **Anti-padrão — ajustar o limiar nos dados reportados:** escolher o corte que parece melhor nos mesmos itens sobre os quais você reporta os resultados. **Por que as pessoas fazem isso:** produz um número limpo para publicar. **Consequência:** a economia reportada vai superestimar o que você obtém em produção. **Correção:** fixe o limiar em uma amostra reservada antes de medir a economia.

## Então, qual você deve usar?
- Portões e filtros de sim/não: Jev. Ele igualou todos os modelos de fronteira do teste, por uma fração de centavo por decisão.
- Roteamento e classificação em volume: Jev, com os itens de baixa confiança escalados a um modelo maior. Sua confiança continua útil para escolher o que escalar.
- Julgamentos em que "depende" é a resposta certa: o Claude Fable 5.1 foi o único modelo cujas probabilidades acompanharam a discordância humana.
- Qualquer coisa pela qual um usuário espera: os 0.43 segundo do Jev estão em uma classe diferente de 1 a 6 segundos.

## Ressalvas
O autor sinaliza várias limitações. São 50 itens por tarefa, então diferenças por tarefa abaixo de cerca de 10 pontos estão dentro do ruído, enquanto o ranking geral é mais sólido. Toda a execução foi feita de um único local, a Índia, via OpenRouter, então a latência inclui o tempo de rede e seria menor mais perto dos servidores, para todos os modelos. A calibração é pior do que outras medições independentes encontraram, e os itens do autor eram mais difíceis e em menor número, então o número de calibração deles é um ponto de dados isolado. As respostas de roteamento dos LLMs foram compactas (uma resposta e uma confiança), e não o vetor completo de probabilidades. O Jev está em acesso antecipado (versão 1.13, build datado de 17 de setembro de 2026), e todos os modelos aqui são atualizados com o tempo, então os números descrevem uma semana de setembro de 2026.

## Atualização, 20 de setembro de 2026
O autor depois executou novamente 20 itens tanto no OpenRouter quanto na API própria da TypeSafe, de forma intercalada. O OpenRouter teve mediana de 384 ms e a API direta 874 ms, ambas a partir da Índia. As duas concordaram em 19 de 20 decisões, e a única discordância foi um empate apertado entre duas intenções de roteamento (0.25 e 0.24). A TypeSafe relata de 70 a 500 ms a partir da costa oeste dos EUA, então a diferença parece ser a distância até os servidores deles, e não a rota da API. Leitores dos EUA devem esperar que a rota direta seja mais rápida do que a medida pelo autor.

O harness de teste, todas as 1,200 respostas brutas, o manifesto de itens e o código dos gráficos são publicados em [manjunathshiva/jev-frontier-bench](https://github.com/manjunathshiva/jev-frontier-bench). A análise pode ser reexecutada a partir dos registros publicados, sem chave de API. O autor pagou por cada chamada, e nenhuma empresa patrocinou ou revisou o teste.

## 🔑 Principais conclusões
- O Jev igualou todos os modelos de fronteira em perguntas de sim/não e ficou atrás dos cinco LLMs na precisão geral em 2 a 11.5 pontos, a cerca de 1/478 do preço do modelo mais preciso testado.
- Só o Claude Fable 5.1 (11.5 pontos à frente do Jev) e o GPT-6 Astra (6.5 pontos à frente) o superam com clareza; os outros três LLMs ficaram dentro do ruído.
- A latência mediana do Jev foi de 0.43 segundo, e seus 5% mais lentos ficaram abaixo de 0.6 segundo, contra 1 a 6 segundos dos LLMs e uma cauda de 23 segundos do DeepSeek.
- O Jev é mais fraco no roteamento com intenções sobrepostas (76%) e nas avaliações exatas por estrelas, em que costuma ficar a uma estrela da correta.
- Em itens ambíguos, o Jev e o GPT-6 Astra deram respostas quase certas que escolheram a visão humana minoritária, e o Jev escolheu "neither" em 29 dos 50 pares, onde a maioria humana escolheu em 19.
- O Claude Fable 5.1 usou cerca de 2 tokens de raciocínio por pergunta, então o raciocínio de modelos topo de linha acrescenta pouco em decisões curtas, mas acrescenta custo.
- Uma cascata que envia as respostas de baixa confiança do Jev a um modelo maior mantém a maior parte da velocidade e da precisão por uma fração do custo, desde que o limiar seja validado em dados reservados.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Em um papel de arquitetura de nível Staff, o movimento útil é classificar a carga de decisões dos seus sistemas de IA em portões, roteamento de alto volume e julgamentos genuínos, e então associar cada um a um nível de modelo. A questão em aberto que isso levanta para o trabalho de governança de IA é como você evidenciaria a calibração e a consciência de discordância para um revisor, dado que o artigo mostra que um modelo pode ser rápido, barato e confiantemente errado em itens disputados.

## 📚 Plano de leitura
### Do artigo
- [Introducing System One Models & Jev (TypeSafe AI Blog)](http://typesafe.ai) — a descrição do próprio fornecedor sobre o modelo e seu objetivo de design.
- [jev-decision-bench](https://github.com/OmarMujahid/jev-decision-bench) — o benchmark independente mais completo, com 49 tarefas contra o GPT-5.6 Luna.
- [jev-ood-calibration](https://github.com/scienthoon/jev-ood-calibration) — o teste de calibração fora da distribuição que constatou que as respostas de escolha do Jev são excessivamente confiantes.
- [jev-frontier-bench](https://github.com/manjunathshiva/jev-frontier-bench) — o harness completo, as respostas brutas e o manifesto de itens desta comparação.
### Para ir além
- ChaosNLI (distribuições de rótulos humanos para inferência em pares de sentenças) — a origem da medição de discordância e o melhor modelo para testar suas próprias decisões ambíguas.
- Calibração de modelos e regras de pontuação adequadas — necessárias para julgar se as probabilidades de um modelo são confiáveis antes de construir uma cascata sobre elas.
- Padrões de cascata e roteamento de modelos para controle de custo de LLMs — o padrão geral por trás do resultado "Jev primeiro, depois escalar".
