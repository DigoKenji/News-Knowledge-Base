---
type: Concept
title: Destilação
description: Treinar um modelo aluno menor ou mais barato para reproduzir as saídas de um modelo professor maior, de modo que o aluno mantenha boa parte da capacidade do professor a uma fração do custo.
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
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Hinton, G., Vinyals, O. and Dean, J. (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531'
  resource: 'https://arxiv.org/abs/1503.02531'
  kind: paper
- title: Bucilă, C., Caruana, R. and Niculescu-Mizil, A. (2006). Model Compression. KDD 2006
  resource: 'https://dl.acm.org/doi/10.1145/1150402.1150464'
  kind: paper
- title: 'Sanh, V. et al. (2019). DistilBERT, a distilled version of BERT. arXiv:1910.01108'
  resource: 'https://arxiv.org/abs/1910.01108'
  kind: paper
- title: 'Xu, X. et al. (2024). A Survey on Knowledge Distillation of Large Language Models. arXiv:2402.13116'
  resource: 'https://arxiv.org/abs/2402.13116'
  kind: paper
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 8 Making Models Smaller via Knowledge Distillation, pp. 217-229'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 8 Model Distillation, p. 395'
lang: pt-br
translation_of: /concepts/ai-ml/distillation.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Destilação

**Definição.** Destilação de conhecimento transfere o que um modelo professor aprendeu para um modelo aluno, treinando o aluno nas saídas do professor em vez dos rótulos originais (ou além deles). Hinton et al. mostraram que a distribuição completa de probabilidades do professor ("soft targets") carrega mais informação que o único rótulo correto, por exemplo que a imagem de um 2 se parece um pouco com um 3.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

A destilação clássica minimiza a diferença entre as distribuições de saída do aluno e do professor, suavizadas com uma temperatura, mais a perda normal nos rótulos verdadeiros. Em LLMs as formas comuns são destilação por logits (igualar as probabilidades dos tokens, o que exige acesso aos logits do professor) e destilação em nível de sequência ou "caixa-preta" (fazer fine-tuning do aluno em texto gerado pelo professor, o que exige apenas acesso à API). O DistilBERT manteve cerca de 97% do desempenho de compreensão de linguagem do BERT com 40% menos parâmetros e inferência 60% mais rápida.

## Por que importa e trade-offs

Destilação é como fornecedores lançam modelos pequenos e rápidos (as linhas "mini" e "flash") e como times criam modelos baratos para tarefas específicas. Alunos raramente igualam os professores em raciocínio difícil, herdam os erros e vieses do professor e ficam limitados à qualidade dos dados gerados. Destilar o modelo de um concorrente pela API normalmente viola os termos de uso; os provedores chamam isso de destilação adversarial e tentam detectá-la.

## Exemplo

Gerar 50.000 tickets de suporte rotulados com um modelo de fronteira e depois fazer fine-tuning de um modelo de 3 bilhões de parâmetros com eles para classificar tickets em produção a uma fração do custo.

## Notas de revisão para Kenji

O rascunho ("extrair capacidades") descrevia só o caso adversarial; a definição agora é a técnica geral, com o uso adversarial como ressalva.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Tunstall, L., von Werra, L. and Wolf, T. (2022)**, *Natural Language Processing with Transformers, revised edition*, ch. 8 Making Models Smaller via Knowledge Distillation, pp. 217-229: Destila um transformer com fine-tuning e avalia o modelo aluno.
- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 8 Model Distillation, p. 395: Destilação como técnica de síntese de dados para treinar modelos menores.

## Fontes

1. Hinton, G., Vinyals, O. and Dean, J. (2015). *Distilling the Knowledge in a Neural Network*. arXiv:1503.02531. <https://arxiv.org/abs/1503.02531> — Soft targets, temperatura, o método professor-aluno.
2. Bucilă, C., Caruana, R. and Niculescu-Mizil, A. (2006). *Model Compression*. KDD 2006. <https://dl.acm.org/doi/10.1145/1150402.1150464> — A ideia anterior de comprimir um ensemble num único modelo pequeno treinado nas suas saídas.
3. Sanh, V. et al. (2019). *DistilBERT, a distilled version of BERT*. arXiv:1910.01108. <https://arxiv.org/abs/1910.01108> — Números concretos de tamanho, velocidade e qualidade de um modelo de linguagem destilado.
4. Xu, X. et al. (2024). *A Survey on Knowledge Distillation of Large Language Models*. arXiv:2402.13116. <https://arxiv.org/abs/2402.13116> — Destilação caixa-branca vs caixa-preta para LLMs.
5. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 8 Making Models Smaller via Knowledge Distillation, pp. 217-229. Destila um transformer com fine-tuning e avalia o modelo aluno.
6. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 8 Model Distillation, p. 395. Destilação como técnica de síntese de dados para treinar modelos menores.

## Relações
- (nenhuma ainda)

## Notas
- [Apresentando Claude Fable 5.2 e Claude Mythos 5.2](/pt-br/notes/ai-ml/2026-11-18-introducing-claude-fable-5-2-and-claude-mythos-5-2.md)
