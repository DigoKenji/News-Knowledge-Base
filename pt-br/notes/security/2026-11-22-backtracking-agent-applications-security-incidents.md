---
type: Paper Summary
title: Incidentes de Segurança em Aplicações de Agentes com Backtracking
description: Origin, uma ferramenta de grafo entre camadas, identifica corretamente o ponto de entrada de comprometimento em todos os oito incidentes de ataque a aplicações de agentes testados.
resource: https://arxiv.org/abs/2611.26525
tags:
- security
- ai-agent
- agent-memory
- indirect-prompt-injection
- rag
- multi-agent-orchestration
- memory-poisoning
- information-flow-control
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26525
  title: Backtracking Agent Applications Security Incidents
  author: Yicheng Cai, Taegyu Kim, Peng Liu
status: stable
source_url: https://arxiv.org/abs/2611.26525
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: security
concepts:
- ai-agent
- agent-memory
- indirect-prompt-injection
- rag
- multi-agent-orchestration
- memory-poisoning
- information-flow-control
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/420baf74172d8139a64adaef06d9f6e4
lang: pt-br
translation_of: /notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md
---

## 🎯 Em uma frase
Origin, uma ferramenta de backtracking construída sobre um grafo entre camadas, identifica o ponto de entrada de comprometimento (CEP) correto em todos os oito incidentes de ataque testados, em cinco aplicações de agentes de código aberto, com um único candidato por incidente.

## ❓ O problema
Quando uma aplicação de agente baseada em LLM sofre um ataque, quem responde precisa encontrar o ponto de entrada de comprometimento (CEP): a chamada de função que primeiro deixou o conteúdo malicioso entrar na aplicação. Os dois ataques dominantes são a injeção indireta de prompt (IPI), em que instruções escondidas em conteúdo recuperado, como páginas web, sequestram o agente, e o envenenamento de memória e de contexto (MC-Poison), em que dados maliciosos são gravados em um armazenamento que o agente lê depois. As evidências utilizáveis são o log de trajetória, que registra funcionalidades do agente como planejamento ou recuperação de memória, e o código-fonte; mas o log raramente indica a chamada de função responsável, e o analista precisa fazer a ligação entre as duas camadas manualmente. No exemplo do GPT-Researcher do artigo, a análise estática de taint (rastreamento de dados desde uma chamada de origem, passando por variáveis do programa, até uma chamada de destino) apontou 1.895 chamadas de API como candidatas, e a poda guiada por log deixou 1.518 que ainda precisavam de inspeção manual, o que, segundo os autores, levaria horas. Os analisadores estáticos existentes raciocinam apenas sobre variáveis do programa e não têm noção de qual funcionalidade do agente cada variável serve.

## 💡 A ideia
Em vez de associar código a funções do agente uma a uma, os autores agrupam as funcionalidades do agente em algumas regiões, de acordo com o tipo de fonte de informação que tocam. Uma fonte externa é qualquer coisa fora da aplicação, como a entrada do usuário, uma página web, um e-mail ou a saída de um LLM; uma fonte interna é algo dentro da aplicação, como uma base de conhecimento ou a memória. Cada região é um conjunto de variáveis do programa que implementa um grupo de funcionalidades, de modo que um analista pode fazer uma pergunta em nível de região, como `CHECK PA DEPENDS_ON RR`, e receber de volta um grafo pequeno. O passo difícil é atribuir variáveis às regiões, porque um nome como `loader` ou `ret_data` não diz nada sobre o papel da variável. Os autores resolvem isso codificando os passos que um analista segue durante a inspeção de código como um conjunto de regras, que precisam de apenas um pequeno número de rótulos fornecidos pelo desenvolvedor.

## ⚙️ Como funciona

### Grafo de Dependência entre Camadas (CLDG)
O CLDG é um grafo direcionado com dois planos. O plano de variáveis vem da propagação de taint para frente a partir do valor de retorno de cada chamada de API da aplicação, sensível a contexto, a fluxo e a campo, de modo que qualquer CEP que corresponda à definição fica em um caminho dele. O plano de funções contém todas as funções alcançáveis a partir dessas chamadas de API, e cada nó de função recebe um rótulo de região. Arestas entre planos conectam cada função a todas as variáveis do programa que ela define ou usa, o que permite rastrear uma resposta em nível de região até variáveis concretas.

```text
G = (R, V_fn, λ, E_cross, V_var, E_var)
```

R é o conjunto de regiões (as cinco regiões representativas do artigo são Per, Perceiving (percepção); PA, Planning and Acting (planejamento e ação); RR, Recalling and Reflecting (recordação e reflexão); PK, Prompt Kitchen (cozinha de prompt); e UNK, Unknown (desconhecido)). V_fn é o conjunto de nós de função alcançáveis a partir de chamadas de API. λ associa cada nó de função a uma região ou a UNK. V_var é o conjunto de variáveis do programa alcançáveis por taint, e E_var é o conjunto de arestas de taint entre elas. E_cross conecta cada função às variáveis que ela define ou usa.

### Rótulos do desenvolvedor e inferência de regiões
Antes da inferência, o desenvolvedor rotula cada função que toca uma fonte de informação como EXT (externa) ou INT (interna); os rótulos vêm de uma tabela de função para fonte coletada uma vez por aplicação. Os autores usam um LLM para rascunhar esse rotulagem como um relatório estruturado, e um humano a valida, o que reduz o trabalho de rotulagem à revisão de um relatório. A inferência de regiões então aplica três regras de propagação de rótulos e nove regras de condição-ação (Tabela 1). A propagação leva um rótulo adiante a partir de uma leitura de uma fonte e para trás a partir de uma escrita, e marca chamadas de montagem de prompt, como `format` ou nomes que contêm "prompt", dentro de uma única função. As regras de condição-ação combinam esses rótulos em uma região: uma função que recebe uma requisição do usuário recebe um rótulo externo e nenhum rótulo de montagem de prompt, e por isso é classificada como Per, que apenas lê passivamente informações externas.

### Linguagem de consulta e avaliação
Um analista submete consultas `CHECK` cujos átomos são asserções `DEPENDS_ON` entre regiões ou variáveis, combinadas com AND e OR, e encadeadas com uma seta, de modo que cada passo restringe o resultado anterior. Uma consulta em nível de região retorna os nós de função nos caminhos de taint entre as regiões consultadas; o analista então escolhe as variáveis de origem e de destino para uma consulta em nível de variável. Consultas em nível de variável usam o grafo de chamadas e os grafos de fluxo de controle para manter apenas os caminhos viáveis no fluxo de controle, e a saída é um conjunto de caminhos em nível de função que mostra o CEP. O protótipo tem cerca de 16 KLoC de Python construídas sobre o Joern (v4.0.360), com o Pyrefly (v0.45.0) para inferência de tipos.

```text
CHECK (PK DEPENDS_ON PA) AND (PA DEPENDS_ON PK)
```

Esta é a consulta que o artigo usa para o backtracking de IPI. O primeiro átomo pergunta se dados da região PA, como uma página web rastreada, podem fluir para a região PK, onde um prompt é montado. O segundo pergunta se o prompt montado pode fluir de volta para PA, por exemplo quando a saída do LLM é gravada em um documento.

## 🧪 Evidências
| Configuração | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| 8 incidentes, 5 aplicações: CEP correto | Nenhuma linha de base numérica informada | 8 de 8 corretos, 1 candidato cada | CEP e caminho identificados (Tabela 3) |
| Verificação do caminho do CEP, 8 incidentes | Joern 0 de 8, CodeQL 0 de 8, Pysa 1 de 8 (IPI no MetaGPT) | 8 de 8 (Origin) | Caminho de taint correto encontrado (Tabela 4) |
| Escopo da inspeção | Grafo de taint completo em nível de variável (100%) | Plano de variáveis reduzido a 6,49%–35,76% do original | Proporção dos nós do plano de variáveis retornados (a introdução informa redução de 64–94%) |
| Acurácia da inferência de regiões | Não informada | Precisão e revocação de 0,70 a 0,88 em cinco aplicações; aider a menor | Precisão e revocação |
| Esforço de rotulagem pelo desenvolvedor | Não informado | 2,4%–8,9% das funções recebem rótulos | Proporção de funções com rótulos EXT ou INT |
| Coleta de conhecimento com relatório do LLM | Rotulagem manual | Velocidade +15,4–75,0%, acurácia +8,3–50,4% | Variação de velocidade e acurácia (Seção 10.8) |
| Quivr MC-Poison (M), esforço do analista | Inspeção de 2.615 LoC do código-fonte do Quivr | 8 nomes de função e 2 subgrafos de taint | Artefatos de código inspecionados |
| Tempo de execução e memória | Não informado | Construção do CLDG responde por mais de 95% do tempo total; consultas abaixo de 90 ms em média; pico de memória abaixo de 7 GB | Tempo e pico de memória |

O resultado principal é 8 de 8 CEPs identificados, com um candidato cada, mas o trecho não informa quantos analistas ou execuções repetidas foram usados por incidente. A comparação com ferramentas estáticas testa verificação, não descoberta: Joern, CodeQL e Pysa receberam o CEP e o destino de referência, e mesmo assim raramente relataram o caminho correto. Todos os números vêm da avaliação dos próprios autores em cinco aplicações, e a acurácia das regiões é dada como intervalo, não por aplicação. Este trecho termina no meio de uma frase na Seção 10.5, portanto as Tabelas 5 a 8 e os apêndices não estavam disponíveis para verificação.

## ⚠️ Limitações
- Os autores restringem o trabalho a agentes de fluxo de trabalho predefinido (C1), em que o código, e não o LLM, decide o controle de fluxo. Agentes semelhantes ao OpenClaw (C2), em que o LLM escolhe a próxima funcionalidade em tempo de execução, e frameworks de biblioteca de agentes estão fora do escopo e ficam para pesquisas futuras.
- A inferência de regiões exige rótulos fornecidos pelo desenvolvedor, e os rótulos rascunhados pelo LLM ainda precisam de validação humana.
- Os autores chamam suas regras de condição-ação de "práticas, e não exaustivas". O aider teve a menor acurácia porque seus comandos interativos não são encadeados automaticamente, o que gera caminhos de taint curtos e desconexos.
- O Joern teve três limitações que causaram falsos negativos; os autores as corrigiram e adicionaram resolução de chamadas com informação de tipos via Pyrefly.
- Os autores dizem que o código-fonte do Origin será publicado no GitHub; o trecho não contém nenhum link.
- 🔎 Contexto adicional: os oito incidentes são ataques que os autores construíram com métodos de trabalhos anteriores, em cinco aplicações de código aberto; nada aqui mostra resultados em incidentes de produção ou em sistemas de código fechado.
- 🔎 Contexto adicional: o estudo com humanos teve quatro participantes, e o trecho não informa quantos analistas executaram os oito casos de backtracking, portanto o resultado de 8 de 8 se apoia em uma amostra de analistas pouco descrita.

## 🏗️ O que isso significa para construir sistemas
Esta é uma interpretação, não uma afirmação dos autores. O Origin ficaria no caminho de resposta a incidentes de uma plataforma de agentes: o log de trajetória e os logs da aplicação apontam a funcionalidade suspeita, e um CLDG construído offline a partir do repositório dá aos responsáveis uma interface de consulta em nível de região. O custo está sobretudo na parte offline, já que a construção do grafo domina o tempo de execução e o pico de memória fica abaixo de 7 GB, mas alguém precisa manter os rótulos EXT/INT à medida que o código muda. Para confiar nisso em produção, seria necessário revisar os rótulos na sua própria base de código, medir a taxa de falsos negativos em um conjunto maior de incidentes e ter evidências de que funciona em código que os autores não testaram.

## 🔑 Principais conclusões
- Logs de trajetória descrevem funcionalidades do agente, mas raramente indicam a chamada de função que causou um comprometimento, por isso o backtracking precisa de uma ponte entre as duas camadas.
- A ponte do artigo é um grafo de dependência entre camadas que agrupa variáveis em regiões de acordo com o tipo de fonte de informação que tocam.
- O Origin identificou o CEP correto, com um único candidato, em todos os oito incidentes em cinco aplicações de agentes de código aberto.
- Joern e CodeQL não verificaram nenhum dos oito caminhos de taint corretos da Tabela 4, e o Pysa verificou um, por isso ferramentas genéricas de taint não bastam sozinhas.
- Consultas em nível de região reduzem o plano de variáveis a 6,49%–35,76% do seu tamanho original, mas a abordagem depende de desenvolvedores rotularem 2,4%–8,9% das funções e de revisão humana dos rascunhos do LLM.
- Quando uma chamada não é registrada em log, os autores recomendam colocar instruções de log em torno da chamada do CEP, para que a aplicação seja diagnosticável em incidentes futuros.

## 📚 Plano de leitura
### No artigo
- [arXiv:2611.26525](https://arxiv.org/abs/2611.26525) — o artigo completo; este trecho para na Seção 10.5, então leia ali as Tabelas 5 a 8 e os apêndices.
### Para se aprofundar
- BackTracker (citado como [45]) — a ideia de backtracking por dependência em que o artigo se apoia.
- Joern, CodeQL e Pysa — os analisadores estáticos de taint com os quais o artigo se compara, e de onde vieram seus falsos negativos.
- Top 10 da OWASP para ameaças em aplicações de agentes (citado como [54]) — a lista de ameaças que coloca IPI e MC-Poison como os dois mais prevalentes.

## Conceitos
- [Agente de IA](/pt-br/concepts/ai-ml/ai-agent.md)
- [Memória de agente](/pt-br/concepts/ai-ml/agent-memory.md)
- [Injeção indireta de prompt](/pt-br/concepts/security/indirect-prompt-injection.md)
- [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)
- [Orquestração multiagente](/pt-br/concepts/ai-ml/multi-agent-orchestration.md)
- [Envenenamento de memória](/pt-br/concepts/security/memory-poisoning.md)
- [Controle de fluxo de informação](/pt-br/concepts/security/information-flow-control.md)
