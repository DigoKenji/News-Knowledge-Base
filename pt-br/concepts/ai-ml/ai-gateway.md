---
type: Concept
title: Gateway de IA
description: Um proxy compartilhado entre aplicações e provedores de LLM que centraliza roteamento, autenticação, controle de custos, limites de taxa, cache, logs e políticas para toda chamada a modelo.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Cloudflare. AI Gateway documentation
  resource: 'https://developers.cloudflare.com/ai-gateway/'
  kind: docs
- title: Databricks. AI Gateway documentation
  resource: 'https://docs.databricks.com/aws/en/ai-gateway/'
  kind: docs
- title: LiteLLM. Proxy Server (LLM Gateway) documentation
  resource: 'https://docs.litellm.ai/docs/simple_proxy'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 10 Step 3. Add Model Router and Gateway, pp. 456-460'
lang: pt-br
translation_of: /concepts/ai-ml/ai-gateway.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Gateway de IA

**Definição.** Um AI gateway é um API gateway especializado em tráfego de modelos. As aplicações chamam um único endpoint; o gateway encaminha cada requisição a um provedor de modelo (OpenAI, Anthropic, um modelo próprio) e aplica controles da organização no caminho: quem pode usar qual modelo, quanto pode gastar, o que pode ser registrado e o que fazer quando um provedor falha.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Recursos comuns: uma API unificada entre provedores; gestão de credenciais para que times nunca tenham as chaves dos provedores; cotas por time, limites de taxa por tokens e atribuição de custos (chargeback); fallback e balanceamento entre modelos ou regiões; cache de respostas, inclusive cache semântico; log de requisições e respostas para auditoria e avaliação; e guardrails como remoção de dados pessoais ou filtros de prompt injection. Produtos incluem Cloudflare AI Gateway, Kong AI Gateway, Databricks Mosaic AI Gateway e o proxy open source LiteLLM.

## Por que importa e trade-offs

Sem um gateway cada time integra provedores separadamente, então gastos, exposição de dados e escolha de modelos ficam invisíveis para a organização. Um gateway dá um lugar único de governança e torna barato trocar de provedor. Ele adiciona um salto de latência, vira uma dependência crítica (precisa ser altamente disponível) e concentra prompts sensíveis num único repositório de logs que também precisa ser protegido.

## Exemplo

A AUMOVIO (da nota de 2026-11-19) roteia todo o uso interno de LLM pelo Databricks Unity AI Gateway para aplicar permissões e devolver os custos às unidades de negócio.

## Notas de revisão para Kenji

O termo vem de fornecedores e ainda não tem definição de órgão de padronização; as três fontes são fornecedores independentes que concordam no conjunto de recursos.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 10 Step 3. Add Model Router and Gateway, pp. 456-460: Roteadores que escolhem um modelo por consulta e gateways que unificam acesso, chaves, fallbacks e controle de custo.

## Fontes

1. Cloudflare. *AI Gateway documentation*. <https://developers.cloudflare.com/ai-gateway/> — Cache, limite de taxa, análise, log e fallback para requisições a modelos.
2. Databricks. *AI Gateway documentation*. <https://docs.databricks.com/aws/en/ai-gateway/> — Recursos de governança - permissões, rastreio de uso, guardrails, roteamento de tráfego.
3. LiteLLM. *Proxy Server (LLM Gateway) documentation*. <https://docs.litellm.ai/docs/simple_proxy> — Implementação open source - API unificada, chaves virtuais, orçamentos, balanceamento.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 10 Step 3. Add Model Router and Gateway, pp. 456-460. Roteadores que escolhem um modelo por consulta e gateways que unificam acesso, chaves, fallbacks e controle de custo.

## Relações
- (nenhuma ainda)

## Notas
- [How AUMOVIO centralized AI access with Databricks Unity Gateway](/pt-br/notes/architecture/2026-11-19-how-aumovio-centralized-ai-access-with-databricks-unity-gate.md)
