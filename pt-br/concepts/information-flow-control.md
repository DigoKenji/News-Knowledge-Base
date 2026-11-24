---
type: Concept
title: Controle de fluxo de informação
description: Uma abordagem de segurança que rotula dados por confidencialidade ou integridade (por exemplo "secreto" ou "não confiável") e acompanha esses rótulos enquanto os dados se movem por um programa, bloqueando fluxos que violariam uma política - como uma entrada não confiável decidir uma ação privilegiada.
tags:
- security
- appsec
domain: security
subdomain: appsec
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Denning, D. E. (1976). A Lattice Model of Secure Information Flow. Communications of the ACM 19(5)
  resource: 'https://dl.acm.org/doi/10.1145/360051.360056'
  kind: paper
- title: Sabelfeld, A. and Myers, A. C. (2003). Language-Based Information-Flow Security. IEEE JSAC 21(1)
  resource: 'https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf'
  kind: paper
- title: 'Debenedetti, E. et al. (2025). Defeating Prompt Injections by Design (CaMeL). arXiv:2503.18813'
  resource: 'https://arxiv.org/abs/2503.18813'
  kind: paper
- title: 'Costa, M. et al. (2025). Securing AI Agents with Information-Flow Control (FIDES). arXiv:2505.23643'
  resource: 'https://arxiv.org/abs/2505.23643'
  kind: paper
relations:
- type: mitigates
  target: indirect-prompt-injection
lang: pt-br
translation_of: /concepts/information-flow-control.md
---

# Controle de fluxo de informação

**Definição.** Controle de acesso pergunta "este sujeito pode ler este objeto?". Controle de fluxo de informação (IFC) faz a pergunta seguinte: "para onde a informação pode ir depois de lida?". Cada valor carrega um rótulo, os rótulos se combinam quando os valores se combinam, e o sistema aplica regras como "dados secretos não podem chegar a uma saída pública" (confidencialidade) ou "dados não confiáveis não podem influenciar uma decisão confiável" (integridade).

Domínio: [security](/pt-br/domains/security.md) · subdomínio `appsec`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

Denning (1976) modelou os rótulos como um reticulado; Sabelfeld e Myers revisaram o IFC baseado em linguagem, em que sistemas de tipos provam em tempo de compilação que um programa não tem fluxos ilegais (não interferência), tratando fluxos explícitos (atribuições) e implícitos (desvios condicionados a um segredo). IFC dinâmico, ou taint tracking, marca valores em tempo de execução. Para agentes de LLM, IFC é uma defesa fundamentada contra prompt injection: dados de e-mails ou páginas web são rotulados como não confiáveis, e o harness garante que possam ser passados como argumento mas nunca escolham qual ferramenta roda ou para onde os dados vão. O CaMeL do Google DeepMind e o FIDES da Microsoft aplicam isso a agentes.

## Por que importa e trade-offs

O IFC dá garantias que filtros e prompts não dão: uma política vale independentemente do que o texto injetado disser. É restritivo (tarefas legítimas podem precisar de desclassificação), os rótulos se espalham e podem contaminar tudo ("label creep"), e exige redesenhar como o agente planeja e executa.

## Exemplo

Um assistente lê um e-mail não confiável pedindo para encaminhar os contatos do usuário. Com IFC, o corpo do e-mail é rotulado como não confiável, então não pode definir o destinatário de uma chamada de envio de e-mail; a ação é bloqueada ou enviada ao usuário para aprovação.

## Fontes

1. Denning, D. E. (1976). *A Lattice Model of Secure Information Flow*. Communications of the ACM 19(5). <https://dl.acm.org/doi/10.1145/360051.360056> — Modelo original de reticulado para fluxo de informação.
2. Sabelfeld, A. and Myers, A. C. (2003). *Language-Based Information-Flow Security*. IEEE JSAC 21(1). <https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf> — Não interferência, fluxos explícitos e implícitos, aplicação estática.
3. Debenedetti, E. et al. (2025). *Defeating Prompt Injections by Design (CaMeL)*. arXiv:2503.18813. <https://arxiv.org/abs/2503.18813> — Capacidades no estilo IFC aplicadas a agentes de LLM.
4. Costa, M. et al. (2025). *Securing AI Agents with Information-Flow Control (FIDES)*. arXiv:2505.23643. <https://arxiv.org/abs/2505.23643> — Planejador IFC independente (Microsoft) para agentes com rótulos de confidencialidade e integridade.

## Relações
- **mitiga** [indirect-prompt-injection](/pt-br/concepts/indirect-prompt-injection.md)

## Notas
- [Kaer: Security-Aware Replanning for Multi-Agent Systems](/pt-br/notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md)
