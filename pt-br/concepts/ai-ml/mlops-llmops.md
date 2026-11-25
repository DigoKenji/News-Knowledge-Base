---
type: Concept
title: 'MLOps e LLMOps'
description: 'As práticas de engenharia que levam sistemas de ML e LLM do notebook para uma produção confiável - dados e modelos versionados, pipelines automatizados (features, treino, inferência), implantação, monitoramento e feedback - com o LLMOps acrescentando prompts, guardrails e avaliação específica de LLM.'
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 1 Building ML systems with feature/training/inference pipelines, pp. 8-22; ch. 11 MLOps and LLMOps, pp. 401-440 (MLOps principles p. 407; LLMOps p. 410; prompt monitoring p. 413)'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 1 AI Engineering Versus ML Engineering, p. 39; ch. 10 AI Engineering Architecture, pp. 449-474 (monitoring and observability p. 465; pipeline orchestration p. 472); user feedback pp. 474-492'
- title: 'Sculley, D. et al. (2015). Hidden Technical Debt in Machine Learning Systems. NeurIPS 2015'
  resource: 'https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/mlops-llmops.md
---

# MLOps e LLMOps

**Definição.** MLOps aplica ideias de DevOps (automação, versionamento, CI/CD, monitoramento) a sistemas cujo comportamento depende tanto de dados e modelos treinados quanto de código. Iusztin e Labonne enquadram um sistema de ML como três pipelines independentes (FTI): um pipeline de features transforma dados brutos em features ou embeddings guardados em um feature store ou banco vetorial; um pipeline de treino transforma features em um modelo versionado em um registro; um pipeline de inferência carrega ambos para servir previsões. O LLMOps acrescenta o que é novo com modelos de fundação: gestão e monitoramento de prompts, guardrails nas entradas e saídas, avaliação com LLM-as-a-judge e de RAG, acompanhamento de custo e latência, e o manejo de modelos enormes que geralmente passam por fine-tuning em vez de treino do zero. A arquitetura de engenharia de IA de Huyen monta a mesma pilha de produção passo a passo: contexto, guardrails, roteador e gateway, caches, padrões de agentes e, por fim, monitoramento, observabilidade e feedback do usuário.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `mlops`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Sculley et al. mostraram que o modelo é uma parte pequena de um sistema de ML real; os dados, a configuração e o código de serviço ao redor são onde a dívida técnica se acumula, e o MLOps existe para gerenciar isso. O drift (de dados e de conceito) é o motivo de o monitoramento nunca terminar. O próprio Project_News é um pequeno sistema de LLMOps: pipelines agendados, prompts versionados aprovados pelo Kenji, um validador e os digests como ciclo de feedback.

## Exemplo

O pipeline de features de um assistente RAG refaz os embeddings de documentos novos toda noite, um pipeline de treino faz fine-tuning de um modelo pequeno todo mês e só o registra se ele superar o atual no conjunto de avaliação, e o pipeline de inferência registra cada prompt, resposta, latência e nota do usuário.

## Conceitos mais amplos

- [DevOps](/pt-br/concepts/software-engineering/devops.md)

## Conceitos mais específicos

- [Feature store](/pt-br/concepts/ai-ml/feature-store.md)
- [Gateway de IA](/pt-br/concepts/ai-ml/ai-gateway.md)

## Conceitos relacionados

- [Avaliação de modelos e métricas](/pt-br/concepts/ai-ml/model-evaluation.md)
- [Drift de dados](/pt-br/concepts/ai-ml/data-drift.md)
- [Observabilidade (logs, métricas, traces)](/pt-br/concepts/cloud-infra/observability.md)

## Fontes

1. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 1 Building ML systems with feature/training/inference pipelines, pp. 8-22; ch. 11 MLOps and LLMOps, pp. 401-440 (MLOps principles p. 407; LLMOps p. 410; prompt monitoring p. 413). A arquitetura de pipelines FTI e o caminho de DevOps para MLOps e LLMOps.
2. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 1 AI Engineering Versus ML Engineering, p. 39; ch. 10 AI Engineering Architecture, pp. 449-474 (monitoring and observability p. 465; pipeline orchestration p. 472); user feedback pp. 474-492. Como a pilha de produção de aplicações com modelos de fundação difere da engenharia de ML clássica.
3. Sculley, D. et al. (2015). *Hidden Technical Debt in Machine Learning Systems*. NeurIPS 2015. <https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems> — Dívida técnica oculta em sistemas de ML.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
