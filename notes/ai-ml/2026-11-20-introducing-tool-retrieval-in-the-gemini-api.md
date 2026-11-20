---
type: Article Summary
title: Introducing Tool Retrieval in the Gemini API
description: Tool Retrieval loads tool parameter schemas on demand, cutting input tokens and latency for agents with large
  tool libraries.
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
---

## 🎯 Core thesis
Loading only the parameter schemas a task needs, instead of sending every tool definition on every request, reduces input tokens and latency and keeps tool selection accurate as an agent's tool library grows.

## 🧭 Why the author wrote it
When an agent connects to dozens or hundreds of tools, sending every full parameter schema on every request inflates input token counts, adds latency, and lowers tool-selection accuracy as the candidate list grows. If the thesis holds, the agent can keep its full tool library on standby and load a schema only when the current task needs it.

## Introduction (no heading in the original)
Google is launching Tool Retrieval in the Gemini API, in preview in the [Interactions API](https://aistudio.google.com/docs/interactions-overview). The article's pitch is that an agent with a large library should not pay the full schema cost on every turn.

> 🔎 Added context: the Interactions API is the Gemini API surface where this feature is in preview. The article links its overview page, which is the place to check current availability.

## How it works
The article's mechanism is a `tool_search` tool plus a `defer_loading` flag on each function you want to hold back. Everything runs inside one API request.

### Tool Retrieval (deferred loading)
Tool Retrieval is the pattern of keeping a tool library on standby and loading only the needed schemas. The model still sees the name and description of every deferred tool, so it knows what exists. The parameter schemas stay out of context until the model asks for one by name, and the API then loads the full schema so the model can call the function. The result is that a large library costs little per request until a tool is actually used.

> ❌ **Anti-pattern — full library on every request:** sending every full parameter schema in every prompt. **Why people do it:** it is the default shape of a tool list and needs no extra design. **Consequence:** more input tokens, more latency, and lower tool-selection accuracy as the candidate list grows. **Fix:** add `tool_search`, set `defer_loading: true` on the long tail, and keep frequently used tools undeferred.

### Deferred tools and defer_loading
To turn it on, add a `tool_search` tool to the request and set `defer_loading: true` on the tools to hold back. The article calls the rest of the library the long tail: you can keep your most-used tools loaded up front and defer the rest. Only the flag changes on the function declaration itself.

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

**What this does:**
- `{"type": "tool_search"}` is the one new entry. It gives the model a way to search the deferred functions.
- `"defer_loading": True` keeps the `issue_refund` parameter schema out of the prompt, while its name and description remain visible to the model.
- The comment on the last list item shows the design choice: frequent tools stay undeferred, and the long tail is deferred.
- The user input is an ordinary request. The model has to find the refund tool itself, within this single call.
- Nothing else in the function declaration changes, so existing tool definitions only gain a flag.

### One request, three visible steps
Inside that single call, the model searches for `issue_refund`, the API loads its schema, and the model emits the `function_call`. The article notes that all three steps show up in `interaction.steps`, which helps when you debug tool names or descriptions by checking which tools the model considered.

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

**What this does:**
- The trace has three entries in order: `tool_search_call` (the model asks for `issue_refund` by name), `tool_search_result` (the API returns the full schema), and `function_call` (the model emits the call with arguments).
- `call_id` links the search result to its search call, so the trail stays traceable when you inspect it.
- The article elides the `signature` values as `"..."`, so the sample does not show their format.
- The `amount` argument arrives as the number `40`, which matches the schema's `"number"` type.

From there, the article says, it is normal function calling. Your code runs the refund and sends back a `function_result`.

### Server-side search with no index
There is no index to build or host. Gemini runs the search on the server, so the only changes to your code are the new tool and the flag on each deferred tool.

### Prefix caching
The article says Tool Retrieval plays well with prefix caching. Deferred schemas stay out of the start of the prompt, and the schemas the model loads are appended later in the conversation. The cached prefix therefore stays the same from turn to turn. The practical effect is that loading schemas does not invalidate the cache for earlier turns.

### Remote MCP servers
Tool Retrieval also works with remote MCP servers in the Gemini API. Set `defer_loading: true` on an `mcp_server` tool and every tool that server exposes loads on demand. You can connect a large server without listing its tools one by one. This is the [Model Context Protocol](https://ai.google.dev/gemini-api/docs/function-calling#mcp) integration path described in the article.

### Mental model: name first, schema second
**The model:** The model reasons from the name and description of every tool, and it asks for the schema only when it has picked one. Selection is cheap and loading is the expensive step.
**How to apply it:** Write each deferred tool's name and description as if they are the only thing the model will read before deciding. Keep the schema for the moment the model commits to a tool.

## How client-side retrieval works
If you maintain your own tool registry, embedding index, or per-user permission filter, declare `tool_search` with `"execution": "client"` and define your search parameters. Your app then runs the lookup. The article notes two consequences: it adds one round trip per search, and tools do not need to be declared upfront, so you can expose new tools mid-conversation when a user connects an integration.

### Client-side retrieval
The model emits a `function_call` for your search tool, your code runs the search against its own registry, and it returns matching `{"type": "function", ...}` declarations in a `function_result`. You own the search logic, which is what makes per-user permission filtering possible.

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

**What this does:**
- `"execution": "client"` tells the API that your code runs the lookup, and the `parameters` block defines the query the model sends to it.
- Tools do not need to be declared upfront, so new integrations can appear mid-conversation.
- `previous_interaction_id` continues the same conversation, so the search result feeds the model's next turn.
- The `function_result` carries matching `{"type": "function", ...}` declarations back, in the same shape as server-side results.
- `my_registry.search(**step.arguments)` passes the model's arguments straight through. 🔎 Added context: the article names per-user permission filtering as a reason to use this mode, so a real implementation would apply that filter before returning any schema.

## How pricing and supported models work
### Pricing
Tool Retrieval has no separate charge. Input and output tokens are billed at the standard rates for the model you call. Turning on `tool_search` adds a small, fixed block of input tokens. Each schema the model loads is billed as input for the rest of the conversation, the same as a tool declared up front. You save tokens when the schemas you never load outweigh the ones you do, which the article calls the usual case for large libraries. Its advice is to measure against your own traffic.

### Supported models
Tool Retrieval works with Gemini 3.6 and later models, including all Gemini 4 models, in the Interactions API. The article links the current model list to check which models apply.

## How to get started
The article ends with three resources. The setup guide has full examples. The model comparison page helps you pick a model for your use case. The Discord discussion is where the team collects feedback on the feature.

## 🔑 Key takeaways
- Tool Retrieval is in preview in the Interactions API. You enable it by adding a `tool_search` tool and setting `defer_loading: true` on the tools to hold back.
- Deferred tools keep their name and description visible to the model. Only the parameter schema stays out of context until the model asks for it.
- The search, the schema load and the function call all happen inside one request. They appear as steps in `interaction.steps`, which makes them inspectable.
- Deferring pays off when the schemas you never load outweigh the ones you do. The article says to measure this against your own traffic.
- There is no separate charge. Standard input and output rates apply, and each loaded schema is billed as input for the rest of the conversation.
- Client-side retrieval lets you own the registry, embedding index or permission filter, at the cost of one extra round trip per search.
- Deferred schemas stay out of the start of the prompt, so the cached prefix holds from turn to turn.

## 🏛️ For Kenji
This is your interpretation, not the author's. Tool loading is a context-budget decision, and it moves authorization into your code when you choose client-side retrieval, since the permission filter sits in the search function. For an architect, the tool registry becomes a governed asset with its own access rules. The question it raises: which tools are frequent enough to stay undeferred, and how do you test that the description alone is enough for the model to pick the right one?

## 📚 Reading plan
### From the article
- [Interactions API overview](https://aistudio.google.com/docs/interactions-overview) — the API surface where Tool Retrieval is in preview.
- [Function calling, MCP section](https://ai.google.dev/gemini-api/docs/function-calling#mcp) — how remote MCP servers are exposed as deferred tools.
- [Latest model list](https://aistudio.google.com/docs/latest-model) — which models accept Tool Retrieval.
- [Tool retrieval documentation](https://aistudio.google.com/docs/tool-retrieval) — the full setup guide with examples.
- [Compare Gemini models](https://aistudio.google.com/docs/choosing-a-model?e=0) — how to pick a model for your use case.
- [Discord discussion](https://discuss.ai.google.dev/) — where to share feedback with the team.
### To go further
- Prefix caching — why a stable prompt prefix keeps cached computation valid across turns.
- Model Context Protocol (MCP) — the open standard behind the remote MCP server integration.
- Tool-selection accuracy with large tool catalogs — how the candidate list size affects the model's choice.
