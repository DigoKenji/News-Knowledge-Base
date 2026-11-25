---
type: Concept
title: Superfície de ataque
description: A soma de todos os pontos em que um atacante poderia tentar entrar, extrair dados ou afetar um sistema - interfaces, entradas, serviços, dependências, pessoas - por isso reduzi-la é uma das medidas de segurança mais eficazes.
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
- title: Manadhata, P. K. and Wing, J. M. (2011). An Attack Surface Metric. IEEE Transactions on Software Engineering 37(3)
  resource: 'https://doi.org/10.1109/TSE.2010.60'
  kind: paper
- title: NIST CSRC. Glossary - attack surface
  resource: 'https://csrc.nist.gov/glossary/term/attack_surface'
  kind: standard
- title: OWASP. Attack Surface Analysis Cheat Sheet
  resource: 'https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html'
  kind: practitioner
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§1.5 Attack Surfaces and Attack Trees, pp. 31-34'
- title: 'Stuttard, D. and Pinto, M. (2008). The Web Application Hacker''s Handbook. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9780470170779'
  kind: book
  locator: 'ch. 4 Mapping the Application, pp. 61-93'
lang: pt-br
translation_of: /concepts/security/attack-surface.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Superfície de ataque

**Definição.** Manadhata e Wing formalizaram a superfície de ataque de um sistema como o conjunto de pontos de entrada e saída, canais e itens de dados não confiáveis que um atacante pode usar. O NIST a define como o conjunto de pontos na fronteira de um sistema em que um atacante pode tentar entrar, causar um efeito ou extrair dados. Superfícies maiores significam mais lugares a defender.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); certificado por Kenji em 2026-11-24.

## Como se encaixa

Reduzir a superfície de ataque significa remover o que não é necessário: fechar portas, desativar funcionalidades não usadas, apagar contas e chaves de API antigas, enxugar dependências. A análise de superfície de ataque faz parte da modelagem de ameaças e do projeto seguro. Tecnologias novas costumam aumentar a superfície em silêncio: cada servidor MCP, plugin ou ferramenta de IA que um agente pode chamar, e cada documento que ele lê, é um novo ponto de entrada.

## Exemplo

Um app web expõe o site público, um painel administrativo, uma API REST, um formulário de upload e 300 dependências npm; cada um faz parte da superfície de ataque, e colocar o painel atrás de single sign-on remove uma parte grande.

## Conceitos mais específicos

- [Ataque à cadeia de suprimentos](/pt-br/concepts/security/supply-chain-attack.md)
- [Model Context Protocol (MCP)](/pt-br/concepts/ai-ml/model-context-protocol.md)
- [Prompt injection indireta](/pt-br/concepts/security/indirect-prompt-injection.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §1.5 Attack Surfaces and Attack Trees, pp. 31-34: Superfícies de ataque de rede, software e humana, e árvores de ataque.
- **Stuttard, D. and Pinto, M. (2008)**, *The Web Application Hacker's Handbook*, ch. 4 Mapping the Application, pp. 61-93: Enumerar a superfície de ataque de uma aplicação web (conteúdo, parâmetros, tecnologias).

## Fontes

1. Manadhata, P. K. and Wing, J. M. (2011). *An Attack Surface Metric*. IEEE Transactions on Software Engineering 37(3). <https://doi.org/10.1109/TSE.2010.60> — Definição formal e medição da superfície de ataque.
2. NIST CSRC. *Glossary - attack surface*. <https://csrc.nist.gov/glossary/term/attack_surface> — Definição padronizada.
3. OWASP. *Attack Surface Analysis Cheat Sheet*. <https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html> — Como mapear e reduzir a superfície de ataque de uma aplicação.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §1.5 Attack Surfaces and Attack Trees, pp. 31-34. Superfícies de ataque de rede, software e humana, e árvores de ataque.
5. Stuttard, D. and Pinto, M. (2008). *The Web Application Hacker's Handbook*. Wiley. <https://books.google.com/books?vid=ISBN9780470170779> — ch. 4 Mapping the Application, pp. 61-93. Enumerar a superfície de ataque de uma aplicação web (conteúdo, parâmetros, tecnologias).

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
