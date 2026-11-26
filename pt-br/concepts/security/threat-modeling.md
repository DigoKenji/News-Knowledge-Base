---
type: Concept
title: Modelagem de ameaças
description: 'Descobrir de forma sistemática o que pode dar errado em um sistema - modelando fluxos de dados e fronteiras de confiança, listando ameaças (por exemplo, com STRIDE) e decidindo mitigações.'
tags:
- security
- appsec
domain: security
subdomain: appsec
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§1.5 Attack Trees, pp. 32-34'
- title: 'Course handout (n.d.). Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)'
  resource: 'FIAP-MBA/Materiais/Referencias (local copy, no public URL)'
  kind: handout
  locator: 'p. 3 (threat modelling and attack trees in gap analysis)'
lang: pt-br
translation_of: /concepts/security/threat-modeling.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição curta acrescentada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
---

# Modelagem de ameaças

**Definição.** A modelagem de ameaças pergunta de forma sistemática o que pode dar errado em um sistema: modelá-lo (fluxos de dados, fronteiras de confiança), enumerar ameaças (por exemplo, com STRIDE ou árvores de ataque) e decidir mitigações. Uma árvore de ataque, como Stallings e Brown a apresentam, decompõe o objetivo de um atacante em subobjetivos e passos alternativos, mostrando quais caminhos são mais baratos de bloquear.

Domínio: [security](/pt-br/domains/security.md) · subdomínio `appsec`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §1.5 Attack Trees, pp. 32-34: Árvores de ataque como forma estruturada de enumerar como um objetivo pode ser atacado.
- **Course handout (n.d.)**, *Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)*, p. 3 (threat modelling and attack trees in gap analysis): Usa modelos de ameaça para decidir se cada lacuna de controle é aceita ou mitigada.

## Fontes

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §1.5 Attack Trees, pp. 32-34. Árvores de ataque como forma estruturada de enumerar como um objetivo pode ser atacado.
2. Course handout (n.d.). *Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)*. — p. 3 (threat modelling and attack trees in gap analysis). Usa modelos de ameaça para decidir se cada lacuna de controle é aceita ou mitigada.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
