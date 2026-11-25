---
type: Concept
title: Segurança de IA e machine learning adversarial
description: Proteger sistemas de IA contra ataques que miram seus dados, modelos e comportamento - envenenamento de dados de treino, evasão ou extração de modelos, prompt injection, vazamento de dados - e os novos riscos criados quando sistemas de IA recebem acesso a ferramentas e dados.
tags:
- security
- core
domain: security
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2025). AI 100-2 E2025 - Adversarial Machine Learning - A Taxonomy and Terminology of Attacks and Mitigations
  resource: 'https://csrc.nist.gov/pubs/ai/100/2/e2025/final'
  kind: standard
- title: OWASP. OWASP Top 10 for LLM Applications 2025
  resource: 'https://genai.owasp.org/llm-top-10/'
  kind: standard
- title: MITRE. ATLAS - Adversarial Threat Landscape for Artificial-Intelligence Systems
  resource: 'https://atlas.mitre.org/'
  kind: standard
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 5 Defensive Prompt Engineering, pp. 235-251 (jailbreaking and prompt injection p. 238; information extraction p. 243; defenses p. 248)'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/security/ai-security.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Segurança de IA e machine learning adversarial

**Definição.** A taxonomia de machine learning adversarial do NIST (AI 100-2) classifica os ataques pela fase do ciclo de vida do sistema e pelo objetivo do atacante: evasão (entradas criadas para causar saídas erradas), envenenamento (corromper dados de treino ou modelos), ataques de privacidade (extrair dados de treino ou o próprio modelo) e, para IA generativa, abusos como prompt injection e jailbreaks. Segurança de IA também cobre a segurança comum do sistema em volta, já que os modelos rodam dentro de aplicações com dados, credenciais e ferramentas.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

O OWASP Top 10 para aplicações de LLM e o MITRE ATLAS (uma base de conhecimento no estilo ATT&CK com ataques reais a IA) são as principais referências práticas. Agentes elevam o risco: um LLM que navega, lê e-mail e chama ferramentas transforma uma resposta enganosa numa ação prejudicial, por isso privilégio mínimo, aprovação humana e controle de fluxo de informação importam tanto ali.

## Exemplo

Evasão - adesivos numa placa de pare fazem um modelo de visão lê-la como placa de limite de velocidade. Prompt injection - texto oculto numa página faz um agente de navegação vazar os dados do usuário.

## Conceitos mais específicos

- [Prompt injection indireta](/pt-br/concepts/security/indirect-prompt-injection.md)
- [Envenenamento de memória](/pt-br/concepts/security/memory-poisoning.md)
- [Controle de fluxo de informação](/pt-br/concepts/security/information-flow-control.md)
- [Model Context Protocol (MCP)](/pt-br/concepts/ai-ml/model-context-protocol.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 5 Defensive Prompt Engineering, pp. 235-251 (jailbreaking and prompt injection p. 238; information extraction p. 243; defenses p. 248): Ataques a aplicações com LLM e defesas em camadas; os guardrails do cap. 10 (p. 451) trazem a visão de sistema.

## Fontes

1. NIST (2025). *AI 100-2 E2025 - Adversarial Machine Learning - A Taxonomy and Terminology of Attacks and Mitigations*. <https://csrc.nist.gov/pubs/ai/100/2/e2025/final> — Taxonomia de ataques a IA preditiva e generativa.
2. OWASP. *OWASP Top 10 for LLM Applications 2025*. <https://genai.owasp.org/llm-top-10/> — Riscos classificados para aplicações de LLM.
3. MITRE. *ATLAS - Adversarial Threat Landscape for Artificial-Intelligence Systems*. <https://atlas.mitre.org/> — Base de conhecimento de táticas e técnicas reais contra sistemas de IA.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 5 Defensive Prompt Engineering, pp. 235-251 (jailbreaking and prompt injection p. 238; information extraction p. 243; defenses p. 248). Ataques a aplicações com LLM e defesas em camadas; os guardrails do cap. 10 (p. 451) trazem a visão de sistema.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
