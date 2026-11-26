---
type: Concept
title: 'Gradiente descendente e retropropagação'
description: 'Como redes neurais aprendem - o gradiente descendente ajusta os pesos repetidamente na direção contrária ao gradiente da perda, e a retropropagação calcula esse gradiente de forma eficiente com a regra da cadeia, camada por camada, da saída para trás.'
tags:
- ai-ml
- deep-learning
domain: ai-ml
subdomain: deep-learning
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning. MIT Press'
  resource: 'https://www.deeplearningbook.org/'
  kind: book
  locator: '§4.3 Gradient-Based Optimization, p. 82; §6.5 Back-Propagation, pp. 204-224; §8.3 Basic Algorithms (SGD, momentum), p. 294'
- title: 'Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). Mathematics for Machine Learning. Cambridge University Press'
  resource: 'https://mml-book.github.io/'
  kind: book
  locator: '§5.6 Backpropagation and Automatic Differentiation, p. 159; §7.1 Optimization Using Gradient Descent, p. 227'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 2 Minimizing loss functions with gradient descent, p. 37 (SGD p. 45); ch. 11 Training neural networks via backpropagation, pp. 362-366'
- title: 'Rumelhart, D. E., Hinton, G. E. and Williams, R. J. (1986). Learning representations by back-propagating errors. Nature 323'
  resource: 'https://doi.org/10.1038/323533a0'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/gradient-descent-backpropagation.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Gradiente descendente e retropropagação

**Definição.** Treinar um modelo significa minimizar uma função de perda sobre seus parâmetros. O gradiente descendente faz isso de forma iterativa: calcula o gradiente da perda em relação a cada parâmetro e dá um pequeno passo (a taxa de aprendizado) na direção oposta. O gradiente descendente estocástico (SGD) estima o gradiente em um pequeno mini-lote aleatório em vez do conjunto inteiro, o que torna viável treinar com muitos dados. A retropropagação é o algoritmo que calcula esses gradientes em uma rede: uma passagem para frente guarda os valores intermediários, e uma passagem para trás aplica a regra da cadeia da perda até a primeira camada, reaproveitando cálculos para que o custo seja próximo ao de uma passagem para frente. É um caso particular da diferenciação automática em modo reverso, que o PyTorch e frameworks semelhantes implementam.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `deep-learning`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Todo modelo de deep learning, de um classificador pequeno a um LLM, é treinado assim; fine-tuning e LoRA são o mesmo laço sobre menos parâmetros ou parâmetros diferentes. A maior parte dos problemas práticos está aqui: uma taxa de aprendizado alta demais diverge, uma baixa demais trava; redes muito profundas ou recorrentes sofrem com gradientes que desaparecem ou explodem, o que motivou ativações ReLU, inicialização cuidadosa, camadas de normalização, conexões residuais e otimizadores adaptativos como o Adam. A memória do treino é dominada pelo que a retropropagação precisa guardar: ativações, gradientes e estado do otimizador, por isso o fine-tuning exige muito mais memória que a inferência (Huyen, cap. 7).

## Exemplo

Um classificador de spam prevê 0,9 para um e-mail legítimo. A perda é alta, a retropropagação diz a cada peso quanto ele empurrou a previsão para "spam", e o SGD move cada peso um pouco no sentido contrário; após muitos mini-lotes os erros diminuem.

## Conceitos mais amplos

- [Redes neurais e deep learning](/pt-br/concepts/ai-ml/deep-learning.md)

## Conceitos mais específicos

- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/ai-ml/lora.md)

## Conceitos relacionados

- [Fine-tuning (ajuste fino)](/pt-br/concepts/ai-ml/fine-tuning.md)

## Fontes

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*. MIT Press. <https://www.deeplearningbook.org/> — §4.3 Gradient-Based Optimization, p. 82; §6.5 Back-Propagation, pp. 204-224; §8.3 Basic Algorithms (SGD, momentum), p. 294. Tratamento formal do aprendizado por gradiente, da retropropagação e dos otimizadores derivados do SGD.
2. Deisenroth, M. P., Faisal, A. A. and Ong, C. S. (2020). *Mathematics for Machine Learning*. Cambridge University Press. <https://mml-book.github.io/> — §5.6 Backpropagation and Automatic Differentiation, p. 159; §7.1 Optimization Using Gradient Descent, p. 227. O cálculo por trás: gradientes, regra da cadeia e por que a diferenciação automática reversa é eficiente.
3. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 2 Minimizing loss functions with gradient descent, p. 37 (SGD p. 45); ch. 11 Training neural networks via backpropagation, pp. 362-366. Versão prática: gradiente descendente do zero e depois a retropropagação programada à mão para uma rede multicamadas.
4. Rumelhart, D. E., Hinton, G. E. and Williams, R. J. (1986). *Learning representations by back-propagating errors*. Nature 323. <https://doi.org/10.1038/323533a0> — O artigo de 1986 que popularizou a retropropagação em redes multicamadas.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
