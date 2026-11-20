---
type: Article Summary
title: Apresentando o Tool Retrieval na API do Gemini
description: O Tool Retrieval carrega os schemas de parâmetros das ferramentas sob demanda, reduzindo tokens de entrada e latência para agentes com grandes bibliotecas de ferramentas.
resource: https://blog.google/innovation-and-ai/technology/developers-tools/gemini-api-tool-retrieval/
tags:
- ai-ml
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://blog.google/innovation-and-ai/technology/developers-tools/gemini-api-tool-retrieval/
  title: Introducing Tool Retrieval in the Gemini API
  author: Google
status: stable
source_url: https://blog.google/innovation-and-ai/technology/developers-tools/gemini-api-tool-retrieval/
source: Google AI blog
source_type: blog
published: '2026-11-19'
domain: ai-ml
concepts:
- model-context-protocol
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81f4a22bf388a91653d4
lang: pt-br
translation_of: /notes/ai-ml/2026-11-20-introducing-tool-retrieval-in-the-gemini-api.md
---

## 🎯 Tese central
Carregar apenas os schemas de parâmetros que a tarefa precisa, em vez de enviar todas as definições de ferramentas em cada requisição, reduz os tokens de entrada e a latência, e mantém a seleção de ferramentas precisa à medida que a biblioteca do agente cresce.

## 🧭 Por que o autor escreveu
Quando um agente se conecta a dezenas ou centenas de ferramentas, enviar todos os schemas completos de parâmetros em cada requisição infla a contagem de tokens de entrada, aumenta a latência e reduz a precisão da seleção de ferramentas conforme a lista de candidatas cresce. Se a tese se sustenta, o agente pode manter sua biblioteca completa de ferramentas em espera e carregar um schema apenas quando a tarefa atual precisar dele.

## Introdução (sem título no original)
O Google está lançando o Tool Retrieval na API do Gemini, em prévia na [Interactions API](https://aistudio.google.com/docs/interactions-overview). A proposta do artigo é que um agente com uma grande biblioteca não deve pagar o custo total dos schemas em cada turno.

> 🔎 Contexto adicionado: a Interactions API é a superfície da API do Gemini em que este recurso está em prévia. O artigo linka a página de visão geral, que é o lugar para verificar a disponibilidade atual.

## Como funciona
O mecanismo do artigo é uma ferramenta `tool_search` mais uma flag `defer_loading` em cada função que você quer segurar. Tudo acontece dentro de uma única requisição à API.

### Tool Retrieval (carregamento adiado)
O Tool Retrieval é o padrão de manter uma biblioteca de ferramentas em espera e carregar somente os schemas necessários. O modelo ainda vê o nome e a descrição de cada ferramenta adiada, então sabe que ela existe. Os schemas de parâmetros ficam fora do contexto até que o modelo peça um deles pelo nome, e então a API carrega o schema completo para que o modelo possa chamar a função. O resultado é que uma grande biblioteca custa pouco por requisição, até que uma ferramenta seja de fato usada.

> ❌ **Antipadrão — biblioteca completa em toda requisição:** enviar todos os schemas completos de parâmetros em cada prompt. **Por que as pessoas fazem isso:** é o formato padrão de uma lista de ferramentas e não exige nenhum desenho extra. **Consequência:** mais tokens de entrada, mais latência e menor precisão na seleção de ferramentas conforme a lista de candidatas cresce. **Correção:** adicione `tool_search`, defina `defer_loading: true` na cauda longa e mantenha as ferramentas mais usadas sem adiamento.

### Ferramentas adiadas e defer_loading
Para ativar, adicione uma ferramenta `tool_search` à requisição e defina `defer_loading: true` nas ferramentas que deseja segurar. O artigo chama o restante da biblioteca de cauda longa: você pode manter as ferramentas mais usadas carregadas desde o início e adiar as demais. Só a flag muda na própria declaração da função.

```python
from google import genai

client = genai.Client()

tools = [
   {"type": "tool_search"},
   {
       "type": "function",
       "name": "issue_refund",
       "description": "Refund an order to the original payment method.",
       "defer_loading": True,
       "parameters": {
           "type": "object",
           "properties": {
               "order_id": {"type": "string"},
               "amount": {"type": "number"},
           },
           "required": ["order_id", "amount"],
       },
   },
   # ... 50+ other deferred functions (leave frequent tools undeferred)
]

interaction = client.interactions.create(
   model="gemini-4-hydrogen",
   input="Order A-1042 arrived damaged. Please refund 40 dollars.",
   tools=tools,
)
```

**O que isso faz:**
- `{"type": "tool_search"}` é a única entrada nova. Ela dá ao modelo uma forma de buscar entre as funções adiadas.
- `"defer_loading": True` mantém o schema de parâmetros de `issue_refund` fora do prompt, enquanto o nome e a descrição continuam visíveis para o modelo.
- O comentário no último item da lista mostra a escolha de design: as ferramentas frequentes ficam sem adiamento, e a cauda longa é adiada.
- A entrada do usuário é uma requisição comum. O modelo precisa encontrar sozinho a ferramenta de reembolso, dentro dessa mesma chamada.
- Nada mais na declaração da função muda, então as definições de ferramentas existentes só ganham uma flag.

### Uma requisição, três etapas visíveis
Dentro dessa única chamada, o modelo busca por `issue_refund`, a API carrega o schema e o modelo emite o `function_call`. O artigo observa que as três etapas aparecem em `interaction.steps`, o que ajuda a depurar nomes ou descrições de ferramentas ao verificar quais ferramentas o modelo considerou.

```json
"steps": [
 {
   "type": "tool_search_call",
   "id": "tsc_7f2a",
   "arguments": {"function_names": ["issue_refund"]},
   "signature": "..."
 },
 {
   "type": "tool_search_result",
   "call_id": "tsc_7f2a",
   "result": [{"type": "function", "name": "issue_refund", "parameters": {...}}],
   "signature": "..."
 },
 {
   "type": "function_call",
   "id": "fc_91c3",
   "name": "issue_refund",
   "arguments": {"order_id": "A-1042", "amount": 40}
 }
]
```

**O que isso faz:**
- O rastro tem três entradas em ordem: `tool_search_call` (o modelo pede `issue_refund` pelo nome), `tool_search_result` (a API devolve o schema completo) e `function_call` (o modelo emite a chamada com os argumentos).
- `call_id` liga o resultado da busca à sua chamada, então o caminho continua rastreável ao ser inspecionado.
- O artigo omite os valores de `signature` como `"..."`, então o exemplo não mostra o formato deles.
- O argumento `amount` chega como o número `40`, o que corresponde ao tipo `"number"` do schema.

Daí em diante, segundo o artigo, é uma chamada de função normal. O seu código executa o reembolso e envia de volta um `function_result`.

### Busca no servidor, sem índice
Não há índice para construir ou hospedar. O Gemini executa a busca no servidor, então as únicas mudanças no seu código são a nova ferramenta e a flag em cada ferramenta adiada.

### Prefix caching
O artigo diz que o Tool Retrieval funciona bem com prefix caching. Os schemas adiados ficam fora do início do prompt, e os schemas que o modelo carrega são anexados depois, na conversa. Assim, o prefixo em cache permanece o mesmo de turno para turno. O efeito prático é que carregar schemas não invalida o cache dos turnos anteriores.

### Servidores MCP remotos
O Tool Retrieval também funciona com servidores MCP remotos na API do Gemini. Defina `defer_loading: true` em uma ferramenta `mcp_server` e cada ferramenta exposta por esse servidor é carregada sob demanda. Você pode conectar um servidor grande sem listar as ferramentas uma a uma. Esse é o caminho de integração do [Model Context Protocol](https://ai.google.dev/gemini-api/docs/function-calling#mcp) descrito no artigo.

### Modelo mental: nome primeiro, schema depois
**O modelo:** o modelo raciocina a partir do nome e da descrição de cada ferramenta, e pede o schema apenas depois de escolher uma. Selecionar é barato, e carregar é a etapa cara.
**Como aplicar:** escreva o nome e a descrição de cada ferramenta adiada como se fossem a única coisa que o modelo vai ler antes de decidir. Guarde o schema para o momento em que o modelo se compromete com a ferramenta.

## Como funciona a recuperação no lado do cliente
Se você mantém seu próprio registro de ferramentas, um índice de embeddings ou um filtro de permissões por usuário, declare `tool_search` com `"execution": "client"` e defina seus parâmetros de busca. Sua aplicação executa então a consulta. O artigo observa duas consequências: isso adiciona uma ida e volta por busca, e as ferramentas não precisam ser declaradas antecipadamente, então você pode expor novas ferramentas no meio da conversa, quando um usuário conecta uma integração.

### Recuperação no lado do cliente
O modelo emite um `function_call` para a sua ferramenta de busca, seu código executa a busca no próprio registro e devolve as declarações `{"type": "function", ...}` correspondentes em um `function_result`. Você controla a lógica de busca, e é isso que torna possível o filtro de permissões por usuário.

```python
tools = [{
    "type": "tool_search",
    "execution": "client",
    "name": "search_tools",
    "description": "Search the internal tool registry for functions.",
    "parameters": {
        "type": "object",
        "properties": {"query": {"type": "string"}},
        "required": ["query"],
    },
}]

# When the model emits a function_call for "search_tools",
# return matching {"type": "function", ...} schemas in function_result:
followup = client.interactions.create(
    model="gemini-4-hydrogen",
    previous_interaction_id=interaction.id,
    tools=tools,
    input=[{
        "type": "function_result",
        "name": "search_tools",
        "call_id": step.id,
        "result": my_registry.search(**step.arguments),
    }],
)
```

**O que isso faz:**
- `"execution": "client"` indica à API que o seu código executa a busca, e o bloco `parameters` define a consulta que o modelo envia a ela.
- As ferramentas não precisam ser declaradas antecipadamente, então novas integrações podem aparecer no meio da conversa.
- `previous_interaction_id` continua a mesma conversa, então o resultado da busca alimenta o próximo turno do modelo.
- O `function_result` devolve as declarações `{"type": "function", ...}` correspondentes, no mesmo formato dos resultados do lado do servidor.
- `my_registry.search(**step.arguments)` repassa diretamente os argumentos do modelo. 🔎 Contexto adicionado: o artigo cita o filtro de permissões por usuário como motivo para usar este modo, então uma implementação real aplicaria esse filtro antes de devolver qualquer schema.

## Como funcionam os preços e os modelos suportados
### Preços
O Tool Retrieval não tem cobrança separada. Os tokens de entrada e saída são cobrados pelas tarifas padrão do modelo que você chama. Ativar `tool_search` adiciona um pequeno bloco fixo de tokens de entrada. Cada schema que o modelo carrega é cobrado como entrada pelo restante da conversa, do mesmo modo que uma ferramenta declarada desde o início. Você economiza tokens quando os schemas que nunca são carregados superam os que são, o que o artigo chama de caso comum para bibliotecas grandes. A recomendação é medir com o seu próprio tráfego.

### Modelos suportados
O Tool Retrieval funciona com o Gemini 3.6 e modelos posteriores, incluindo todos os modelos Gemini 4, na Interactions API. O artigo linka a lista atual de modelos para verificar quais se aplicam.

## Como começar
O artigo termina com três recursos. O guia de configuração traz exemplos completos. A página de comparação de modelos ajuda a escolher um modelo para o seu caso de uso. A discussão no Discord é onde a equipe reúne feedback sobre o recurso.

## 🔑 Principais conclusões
- O Tool Retrieval está em prévia na Interactions API. Você o ativa adicionando uma ferramenta `tool_search` e definindo `defer_loading: true` nas ferramentas que quer segurar.
- As ferramentas adiadas mantêm o nome e a descrição visíveis para o modelo. Só o schema de parâmetros fica fora do contexto até que o modelo o peça.
- A busca, o carregamento do schema e a chamada de função acontecem dentro de uma única requisição. Elas aparecem como etapas em `interaction.steps`, o que as torna inspecionáveis.
- Adiar compensa quando os schemas que nunca são carregados superam os que são. O artigo recomenda medir isso com o seu próprio tráfego.
- Não há cobrança separada. Valem as tarifas padrão de entrada e saída, e cada schema carregado é cobrado como entrada pelo restante da conversa.
- A recuperação no lado do cliente permite que você controle o registro, o índice de embeddings ou o filtro de permissões, ao custo de uma ida e volta extra por busca.
- Os schemas adiados ficam fora do início do prompt, então o prefixo em cache se mantém de turno para turno.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. O carregamento de ferramentas é uma decisão de orçamento de contexto, e transfere a autorização para o seu código quando você escolhe a recuperação no lado do cliente, já que o filtro de permissões fica na função de busca. Para um arquiteto, o registro de ferramentas passa a ser um ativo governado, com regras próprias de acesso. A pergunta que isso levanta: quais ferramentas são frequentes o bastante para ficar sem adiamento, e como você testa se a descrição, sozinha, basta para o modelo escolher a ferramenta certa?

## 📚 Plano de leitura
### No artigo
- [Visão geral da Interactions API](https://aistudio.google.com/docs/interactions-overview) — a superfície da API onde o Tool Retrieval está em prévia.
- [Function calling, seção MCP](https://ai.google.dev/gemini-api/docs/function-calling#mcp) — como servidores MCP remotos são expostos como ferramentas adiadas.
- [Lista atual de modelos](https://aistudio.google.com/docs/latest-model) — quais modelos aceitam o Tool Retrieval.
- [Documentação de tool retrieval](https://aistudio.google.com/docs/tool-retrieval) — o guia completo de configuração, com exemplos.
- [Comparar modelos Gemini](https://aistudio.google.com/docs/choosing-a-model?e=0) — como escolher um modelo para o seu caso de uso.
- [Discussão no Discord](https://discuss.ai.google.dev/) — onde compartilhar feedback com a equipe.
### Para aprofundar
- Prefix caching — por que um prefixo estável do prompt mantém a computação em cache válida entre turnos.
- Model Context Protocol (MCP) — o padrão aberto por trás da integração com servidores MCP remotos.
- Precisão na seleção de ferramentas com catálogos grandes — como o tamanho da lista de candidatas afeta a escolha do modelo.
