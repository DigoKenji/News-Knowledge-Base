---
type: Article Summary
title: Modelo de Decisão de $0.0001 que Vai Substituir Milhões de Chamadas de LLM
description: Pequenos modelos de decisão tipados, com confiança calibrada, devem ficar à frente dos fluxos de agentes, escalando para LLMs de fronteira apenas os casos incertos.
resource: https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
- llm-as-a-judge
- multi-agent-orchestration
- rag
- calibration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
  title: $0.0001 Decision Model That Will Replace Millions of LLM Calls
  author: Agent Native
status: stable
source_url: https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
source: Medium (agentnativedev)
source_type: medium
published: '2026-09-18'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
- llm-as-a-judge
- multi-agent-orchestration
- rag
- calibration
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81d0b5f9edc20efc83c4
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-0-0001-decision-model-that-will-replace-millions-of-llm-call.md
---

# $0.0001 Decision Model That Will Replace Millions of LLM Calls

**Fonte:** https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
**Autor:** Agent Native
**Data:** 2026-09-18
**Conceitos:** [Vazão, latência e custo de inferência](/pt-br/concepts/inference-tradeoffs.md) · [LLM-as-a-judge](/pt-br/concepts/llm-as-a-judge.md) · [Orquestração multiagente](/pt-br/concepts/multi-agent-orchestration.md) · [Geração aumentada por recuperação (RAG)](/pt-br/concepts/rag.md) · [Calibração](/pt-br/concepts/calibration.md)

---

## 🎯 Tese central
Muitas decisões de agentes são frequentes, restritas e já rotuladas, então um modelo pequeno treinado com esses desfechos deve respondê-las diretamente, com uma confiança calibrada, e o LLM de fronteira deve tratar apenas os casos em que o modelo pequeno está em dúvida.

## 🧭 Por que o autor escreveu
Sistemas de agentes enviam cada pequena decisão (roteamento, escolha de ferramenta, filtragem, guardrails, extração) por um LLM de fronteira porque ele já está lá e funciona, mas isso é lento e caro. Se a tese se sustentar, o modelo de fronteira deixa de ser o ramo padrão do fluxo e passa a ser o caminho de escalonamento, e a escolha de design real passa a ser o limiar de confiança no qual um modelo barato pode agir.

## O custo oculto das pequenas decisões
Fluxos de agentes contêm muitas pequenas decisões: em qual fila um chamado entra, se uma nota fiscal é claramente segura, qual ferramenta deve ser carregada, quais trechos recuperados devem permanecer, se um prompt é suspeito e se o agente deve continuar, tentar de novo, pedir a um humano ou parar. As equipes encaminham todas elas por um LLM. O ponto do autor é que isso é conveniente, mas lento e caro, e que a maioria dessas decisões é repetida e restrita.

O autor argumenta que uma grande parte do valor virá de colocar modelos pequenos, especializados em uma decisão, à frente de fluxos agênticos, deixando o modelo de fronteira ver apenas os casos incertos. O artigo usa o Jev, da TypeSafe AI, recém-anunciado, como exemplo dessa direção. O Jev abre mão da geração livre de strings e devolve decisões tipadas com probabilidades, o que a TypeSafe chama de "System One Model" (modelo de Sistema 1): estado não estruturado na entrada, decisões estruturadas e probabilísticas na saída.

O artigo cita dois exemplos, ambos de posts incorporados. O Jev consegue encontrar voos usando um navegador em cerca de 7 segundos, a uma fração do custo atual. Ele também consegue pontuar milhares de mensagens de prospecção personalizadas em 40 segundos, por menos de US$ 0,1, para ver quais terão bom desempenho.

A ideia mais ampla não está presa a um fornecedor. Se você tem desfechos históricos de uma decisão repetida, pode treinar um modelo muito mais barato para respondê-la, anexar uma estimativa de confiança e se abster, encaminhando ao modelo de fronteira quando estiver em dúvida. Isso muda a arquitetura do agente.

> 🔎 Contexto adicionado: a predição seletiva significa que um classificador pode se recusar a responder. A abstenção troca a cobertura (a fração das entradas que ele responde) pela taxa de erro nas entradas respondidas.

## O modelo de fronteira passa a ser o caminho de escalonamento
Considere um fluxo de suporte que processa 10 milhões de chamados por mês, em que cada chamado chega a um modelo de fronteira para triagem. O modelo escolhe entre algumas ações: encaminhar para cobrança, encaminhar para suporte técnico, encaminhar para vendas ou escalar para um humano. A maioria dos chamados não é ambígua, e a empresa provavelmente tem anos de chamados com a decisão final de roteamento anexada.

A correção é inserir um modelo de decisão antes do LLM. Se o modelo mais barato está confiante em 60% do tráfego e atinge a taxa de erro exigida nesse conjunto coberto, o modelo de fronteira recebe apenas 40% das chamadas originais. O artigo tem o cuidado de dizer que 60% é uma ilustração, não um número universal, e que varia conforme o fluxo. O ponto importante é que a cobertura se torna um parâmetro de sistema ajustável. Você não está mais escolhendo entre "modelo barato" e "modelo inteligente". Está escolhendo o limiar de confiança no qual o modelo barato pode agir.

Este é o problema clássico da predição seletiva: preveja quando você sabe, abstenha-se quando não sabe. A pesquisa em classificação seletiva trata isso há anos como um trade-off entre risco e cobertura. Quanto mais tráfego você trata automaticamente, mais erro absorve. A infraestrutura de agentes dá a essa ideia antiga um lugar novo e valioso para ser aplicada.

## Por que modelos no estilo Jev se encaixam em fluxos de agentes
LLMs tradicionais são otimizados para gerar sequências de tokens. Essa é a ferramenta certa quando a saída é código, prosa, análise ou um plano. Muitas decisões dentro de um agente não são problemas de geração de sequência. Se as ações válidas são:

```javascript
["approve", "hold", "reject"]
```

então gerar um objeto JSON token por token é uma forma cara de selecionar um elemento de um conjunto que a aplicação já conhece.

> ❌ **Anti-padrão: gerar uma escolha token por token:** pedir ao LLM que escreva `{"decision": "approve"}` para um conjunto fixo de opções. **Por que as pessoas fazem isso:** o LLM já está na stack e a saída é fácil de interpretar. **Consequência:** cada decisão paga pela decodificação autorregressiva e por uma chamada completa ao modelo de fronteira apenas para selecionar um item de um conjunto conhecido. **Correção:** pontuar as opções conhecidas em uma única passada e devolver uma probabilidade para cada uma.

O Jev avalia perguntas estruturadas em paralelo e devolve probabilidades diretamente. O artigo relata grandes melhorias de latência e custo nas avaliações dos próprios fluxos, mas esses números são informados pelo fornecedor e devem ser validados com a sua própria carga de trabalho. A arquitetura é interessante mesmo se você não aceitar os benchmarks. Um modelo de decisão pode usar uma interface diferente:

```javascript
context + available actions -> probability distribution
```

Essa interface se encaixa bem no software de agentes. A aplicação já conhece as ações permitidas. O modelo não precisa inventar uma string; ele só precisa pontuar as ações. O ponto de partida mais simples do artigo, se você tiver acesso à API, é instalar a skill da TypeSafe com `npx skills add typesafe-ai/skills --skill typesafe-ai` e depois pedir ao seu assistente de código para usar `/typesafe-ai`. Você também pode ler o repositório para entender seus internos.

```javascript
npx skills add typesafe-ai/skills --skill typesafe-ai
```

## Um pequeno experimento de código aberto: jevlike
O repositório jevlike, de Vinny LaRouge, torna a ideia concreta. O autor é preciso ao dizer que o jevlike não é uma implementação do Jev, porque a TypeSafe não publicou a arquitetura nem o método de treinamento do Jev. O jevlike é um modelo inicial independente, com um formato de entrada e saída semelhante: contexto em texto e uma lista variável de opções em texto como entrada, e uma probabilidade por opção como saída.

Internamente, cada opção é transformada em um vetor de consulta. Cada opção faz atenção sobre o contexto, produzindo uma representação de contexto específica para ela. Uma cabeça de pontuação compartilhada emite um logit por opção, seguido de um softmax entre as escolhas disponíveis. Assim, em vez de escrever autorregressivamente:

```javascript
{"route": "billing"}
```

ele calcula uma distribuição sobre as opções em uma única passada de pontuação:

```javascript
[
  {"option": "billing",   "probability": 0.94},
  {"option": "technical", "probability": 0.04},
  {"option": "sales",     "probability": 0.02}
]
```

O repositório oferece dois modos de encoder: um encoder minúsculo, em nível de bytes, treinado do zero, ou um encoder Hugging Face congelado, com uma pequena cabeça de atenção treinável. O segundo modo serve às equipes que têm dados do domínio, mas pouca infraestrutura de treinamento.

### Início rápido
O artigo traz os passos de configuração. Clone o repositório, crie um ambiente virtual e instale em modo editável:

```javascript
git clone https://github.com/vinnylarouge/jevlike.git
cd jevlike

uv venv
source .venv/bin/activate
uv pip install -e '.[dev]'
```

O repositório inclui um gerador de dados sintéticos, uma CLI de treinamento, um avaliador e um preditor:

```javascript
jevlike-data synthetic --output data/synthetic

jevlike-train data/synthetic/train.jsonl \
  --validation data/synthetic/validation.jsonl \
  --output runs/synthetic.pt

jevlike-eval runs/synthetic.pt data/synthetic/test.jsonl

jevlike-predict runs/synthetic.pt \
  --context "Choose the exact badge amber badger. Badge: amber badger." \
  --option "azure crane" \
  --option "amber badger" \
  --option "gold heron"
```

Seus próprios dados de treinamento são um JSONL simples, uma linha por decisão:

```javascript
{"context":"The customer needs a refund.","options":["refund","sales","technical support"],"label":0}
```

Cada linha pode conter um número diferente de opções. Isso importa para agentes, porque catálogos de ferramentas, subagentes, rotas e ações de fluxo mudam ao longo do tempo.

Para representações semânticas mais fortes, o repositório pode congelar um encoder pré-treinado e treinar apenas a cabeça de pontuação:

```javascript
uv pip install -e '.[transformers]'

jevlike-train data/synthetic/train.jsonl \
  --validation data/synthetic/validation.jsonl \
  --output runs/qwen-head.pt \
  --encoder hf \
  --hf-model Qwen/Qwen2.5-0.5B \
  --rank 256 \
  --batch-size 8
```

O checkpoint armazena a cabeça de pontuação treinada e o nome do encoder. O encoder pré-treinado grande permanece congelado.

## Transformando o preditor em um gate de agente
A CLI do repositório carrega um checkpoint, agrupa um contexto com suas opções, executa o modelo e aplica softmax aos logits. As mesmas peças podem se tornar uma pequena função de roteamento dentro de um serviço de agente. A função abaixo age apenas quando a maior probabilidade atinge um limiar de 0,90, e caso contrário devolve `escalate`:

```py
import torch

from jevlike.data import ChoiceExample
from jevlike.model import load_checkpoint, select_device
from jevlike.train import move

DEVICE = select_device("auto")
MODEL, COLLATOR, _ = load_checkpoint("runs/router.pt", DEVICE)
MODEL.eval()


def decide(context: str, options: list[str], threshold: float = 0.90):
    batch = COLLATOR([ChoiceExample(context, tuple(options), 0)])
    batch = move(batch, DEVICE)

    with torch.no_grad():
        logits = MODEL(batch)[0, : len(options)]
        probs = logits.softmax(-1).cpu()

    idx = int(probs.argmax())
    confidence = float(probs[idx])

    if confidence < threshold:
        return {"action": "escalate", "confidence": confidence}

    return {
        "action": options[idx],
        "confidence": confidence,
        "probabilities": dict(zip(options, probs.tolist())),
    }
```

O modelo caro passa então a ser o fallback:

```py
result = decide(ticket_text, ["billing", "technical", "sales", "human"])

if result["action"] == "escalate":
    result = frontier_llm_triage(ticket_text)

return result
```

O padrão central é proteger o LLM atrás de um gate de confiança.

## Onde eu implantaria isto primeiro
Os alvos óbvios são decisões frequentes, restritas e já registradas em log.

### 1. Roteamento de modelo e de requisição
Preveja se uma requisição precisa de um modelo barato, de um modelo de fronteira ou de um humano. O artigo observa que isso já é uma área ativa de pesquisa, e que o RouteLLM mostrou que roteadores aprendidos podem reduzir substancialmente o custo do modelo, mantendo a qualidade nas configurações avaliadas. Um pontuador no estilo Jev leva esse padrão mais fundo na stack.

### 2. Seleção de ferramenta ou de subagente
Muitos frameworks de agentes colocam a descrição de todas as ferramentas no prompt. Isso aumenta o contexto e torna a seleção de ferramentas mais difícil. Em vez disso, pontue primeiro as ferramentas disponíveis, carregue apenas os principais candidatos e deixe o modelo de fronteira raciocinar sobre um espaço de ações menor.

> ❌ **Anti-padrão: enfiar todas as ferramentas no prompt:** incluir a descrição completa de cada ferramenta disponível em todas as chamadas. **Por que as pessoas fazem isso:** é o padrão de muitos frameworks e não exige nenhum componente extra. **Consequência:** o contexto cresce e a seleção de ferramentas fica mais difícil para o modelo. **Correção:** pontue as ferramentas primeiro e carregue apenas os principais candidatos.

### 3. Reranqueamento na recuperação
A recuperação costuma enviar contexto demais para as etapas seguintes. Um pontuador barato pode ranquear os trechos candidatos em relação ao estado atual e descartar as falhas óbvias antes que elas cheguem à janela de contexto cara.

### 4. Guardrails em cada turno
Prompt injection, violações de política, verificações de contradição, chamadas de ferramenta inseguras e a pergunta "esta saída satisfaz o invariante do fluxo?" são decisões repetidas. Executar um juiz de fronteira após cada turno do agente pode custar mais do que o próprio agente. Um modelo de decisão especializado pode atuar como o verificador de primeira linha e escalar apenas os casos ambíguos.

> ❌ **Anti-padrão: um juiz de fronteira em cada turno:** chamar o modelo mais capaz para verificar cada etapa do agente. **Por que as pessoas fazem isso:** é o verificador mais confiável disponível. **Consequência:** o custo da verificação pode superar o custo do trabalho verificado. **Correção:** colocar um pontuador especializado primeiro e escalar apenas os casos ambíguos.

### 5. Extração tipada antes do raciocínio
E-mails, PDFs, transcrições, sinistros, notas fiscais e chamados costumam conter um pequeno conjunto de campos de que a lógica posterior precisa. Extraia ou classifique primeiro as partes previsíveis e reserve o raciocínio generativo para os casos que realmente o exigem.

## A confiança é o produto
A parte difícil não é a acurácia top-1. É a calibração. Se um modelo diz 0,95, esse número precisa significar algo útil operacionalmente: em muitas previsões pontuadas perto de 95%, cerca de 95% devem estar corretas. O artigo observa que redes neurais costumam estar mal calibradas.

> 🔎 Contexto adicionado: o erro de calibração esperado (ECE) mede a diferença entre a confiança declarada e a acurácia observada, com média sobre faixas de confiança. Um ECE baixo significa que um limiar como 0,90 corresponde a aproximadamente 90% de acerto.

Para um gate de agente, o artigo diz para avaliar pelo menos seis coisas:

> 📋 **Checklist — avaliação de um gate de agente**
> - [ ] Acurácia top-1: qualidade básica da decisão.
> - [ ] Erro de calibração esperado: se a confiança corresponde à correção observada.
> - [ ] Cobertura: fração do tráfego tratada sem escalonamento.
> - [ ] Risco seletivo: taxa de erro no tráfego que você trata automaticamente.
> - [ ] Taxa de escalonamento: quanto tráfego de modelo de fronteira permanece.
> - [ ] Custo por fluxo concluído: a métrica que as finanças acabam observando.

Não ajuste o limiar no conjunto de teste. Escolha-o nos dados de validação, congele-o e meça o sistema final em uma divisão separada que reflita as entidades e os períodos de produção. Agrupe registros relacionados na mesma divisão para evitar vazamento. Isso importa em fluxos de negócio, em que o mesmo cliente, comerciante, modelo de documento ou página-alvo pode aparecer muitas vezes.

> ❌ **Anti-padrão: ajustar no conjunto de teste:** escolher o limiar olhando os resultados de teste. **Por que as pessoas fazem isso:** é o caminho mais rápido para atingir um número-alvo. **Consequência:** o desempenho reportado é otimista e não se sustenta em produção. **Correção:** escolher o limiar nos dados de validação e congelá-lo antes do teste final.

> ❌ **Anti-padrão: divisões aleatórias entre registros relacionados:** dividir as linhas ao acaso quando muitas delas compartilham um cliente, comerciante ou modelo. **Por que as pessoas fazem isso:** a divisão aleatória é o padrão. **Consequência:** o modelo vê quase-duplicatas no teste, então a acurácia e a calibração parecem melhores do que são. **Correção:** agrupar registros relacionados na mesma divisão.

## O modelo econômico é simples
Se o modelo de decisão trata uma fração c das requisições, o novo custo é aproximadamente o custo do modelo de decisão em todas as requisições, mais o custo do modelo de fronteira nas (1 - c) requisições restantes, mais a sobrecarga de escalonamento e de novas tentativas:

```javascript
new_cost
  = decision_model_cost_on_all_requests
  + frontier_model_cost_on_(1 - c)_requests
  + escalation/retry overhead
```

A arquitetura compensa quando o primeiro termo é muito menor do que a chamada de fronteira que ela evita, e a perda de qualidade no tráfego coberto fica dentro da sua tolerância. É por isso que pré-processadores minúsculos são mais interessantes do que "use um modelo de chat menor em todo lugar". Eles precisam resolver uma fronteira de decisão repetida de forma muito barata.

> ❌ **Anti-padrão: um modelo de chat menor em todo lugar:** substituir o modelo de fronteira por um modelo geral menor em todas as chamadas. **Por que as pessoas fazem isso:** é uma troca simples. **Consequência:** a qualidade cai em todo o tráfego, sem uma fronteira medida e sem como escolher um limiar. **Correção:** resolver uma fronteira de decisão repetida com um pontuador direcionado e manter o modelo de fronteira para os escalonamentos.

## O que eu construiria em produção
O autor começaria com um ponto de decisão de alto volume que já tenha rótulos limpos. Por exemplo, chega uma nota fiscal, campos determinísticos são extraídos e um pontuador personalizado decide: aprovar com 0,98 vai para o fluxo normal, reter com 0,94 vai para a fila de revisão, e qualquer caso incerto vai para o LLM de fronteira:

```javascript
invoice arrives
   |
   v
extract deterministic fields
   |
   v
custom decision scorer
   |
   +-- approve 0.98 ---> normal workflow
   |
   +-- hold 0.94 ------> review queue
   |
   +-- uncertain ------> frontier LLM
```

O autor recomenda executar primeiro em modo sombra:

> 📋 **Checklist — implantação em produção**
> - [ ] Registrar as probabilidades do pontuador ao lado da decisão existente do LLM.
> - [ ] Construir gráficos de calibração.
> - [ ] Medir risco versus cobertura.
> - [ ] Escolher o limiar a partir da tolerância real do negócio.
> - [ ] Só então ativar a automação para a fatia de maior confiança.

Quando isso funcionar, repita o padrão no próximo ponto de decisão. A visão de longo prazo do artigo é um sistema de software distribuído: código determinístico sempre que possível, pequenas funções de decisão aprendidas onde forem úteis, recuperação e regras para o estado, e raciocínio de fronteira apenas onde a incerteza ou a novidade justificarem o custo. O modelo de fronteira continua sendo o componente mais capaz, mas deixa de ser o ramo padrão.

## 🔑 Principais takeaways
- Decisões repetidas e restritas de agentes, com rótulos históricos limpos, são o primeiro alvo para um pontuador pequeno.
- Um pontuador que devolve uma probabilidade por opção conhecida evita gerar uma escolha token por token.
- O limiar de confiança, e não a escolha do modelo, define quanto tráfego o modelo barato pode tratar.
- A calibração importa mais do que a acurácia top-1, porque o limiar só tem significado se a confiança corresponder à correção.
- Os limiares devem ser escolhidos em dados de validação, e as divisões devem agrupar registros relacionados para evitar vazamento.
- A economia só funciona se o custo do modelo de decisão em todas as requisições for muito menor do que as chamadas de fronteira que ele evita.
- Os benchmarks do fornecedor (as alegações de latência e custo do Jev) devem ser validados na sua própria carga de trabalho antes que você confie neles.

## 🏛️ Para o Kenji
Muitas das decisões dos seus próprios projetos de governança de IA e de agentes (roteamento, guardrails, seleção de ferramentas) são repetidas e registradas em log, o que se encaixa bem no padrão de gate. Esta é a minha interpretação, não a do autor: a pergunta que isso levanta para um arquiteto é qual fronteira de decisão no seu sistema tem desfechos históricos limpos o suficiente para calibrar, e quem é o responsável pelo limiar quando ele começa a trocar erro por custo.

## 📚 Plano de leitura
### Do artigo
- [post incorporado sobre busca de voos](https://x.com/gregpr07/status/2100411066966749359) — o exemplo do fornecedor do Jev encontrando voos em cerca de 7 segundos.
- [post incorporado sobre pontuação de prospecção](https://x.com/romanbuildsaas/status/2100891604735099103) — o exemplo de pontuação de milhares de mensagens por menos de US$ 0,1.
- [repositório jevlike](https://github.com/vinnylarouge/jevlike.git) — o modelo inicial de código aberto e sua CLI de treinamento e de predição.
### Para ir além
- RouteLLM (roteamento de LLM aprendido) — a referência do artigo para a redução de custo com roteadores aprendidos.
- Classificação seletiva (trade-off entre risco e cobertura) — a base de pesquisa para se abster quando há dúvida.
- Calibração e erro de calibração esperado — como verificar se os scores de confiança significam o que dizem.
