---
type: Concept
title: Versionamento de API
description: 'Mudar uma API sem quebrar clientes existentes - mantendo as mudanças compatíveis com versões anteriores ou mantendo versões explícitas em paralelo até que os clientes migrem.'
tags:
- backend-systems
- api-design
domain: backend-systems
subdomain: api-design
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 4 Encoding and Evolution: backward and forward compatibility; Data encoding and evolution for RPC'
- title: 'Google. AIP-185: API Versioning'
  resource: 'https://google.aip.dev/185'
  kind: standard
- title: 'Preston-Werner, T. Semantic Versioning 2.0.0'
  resource: 'https://semver.org/'
  kind: standard
lang: pt-br
translation_of: /concepts/backend-systems/api-versioning.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Versionamento de API

**Definição.** O versionamento de API é a forma como um provedor evolui uma interface sem quebrar seus consumidores. A primeira ferramenta é a compatibilidade: mudanças aditivas (novos campos opcionais, novos endpoints) mantêm clientes antigos funcionando. Quando uma mudança incompatível é inevitável, uma nova versão principal é publicada - no caminho da URL, em um cabeçalho ou no nome do pacote - e a antiga roda em paralelo até os clientes migrarem e ela ser descontinuada.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `api-design`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Aplica a evolução de schema aos contratos de serviço: servidores precisam continuar compatíveis com clientes antigos, que não podem ser atualizados no ritmo do servidor. O versionamento semântico dá o vocabulário (major significa incompatível), e as diretrizes de API do Google colocam a versão principal no caminho e proíbem mudanças incompatíveis dentro de uma versão.

## Exemplo

Renomear customer_name para full_name quebraria apps já instalados, então /v1 mantém o campo antigo enquanto /v2 introduz o novo; a v1 é desligada um ano depois, quando o uso cai a zero.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 4 Encoding and Evolution: backward and forward compatibility; Data encoding and evolution for RPC: As regras de compatibilidade por baixo do versionamento de APIs: servidores precisam continuar compatíveis com clientes antigos.

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 4 Encoding and Evolution: backward and forward compatibility; Data encoding and evolution for RPC. As regras de compatibilidade por baixo do versionamento de APIs: servidores precisam continuar compatíveis com clientes antigos.
2. Google. AIP-185: API Versioning. <https://google.aip.dev/185> — Versão principal no caminho; nada incompatível dentro de uma versão.
3. Preston-Werner, T. Semantic Versioning 2.0.0. <https://semver.org/> — MAJOR.MINOR.PATCH e o que conta como incompatível.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
