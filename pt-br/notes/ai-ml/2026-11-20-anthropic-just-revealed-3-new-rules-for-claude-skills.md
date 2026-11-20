---
type: Article Summary
title: Anthropic acaba de revelar 3 NOVAS regras para Claude Skills
description: Skills do Claude 5.5 precisam de instruções críticas no início, grau de liberdade compatível com o risco da tarefa e checklists com loops de validação.
resource: https://medium.com/ux-planet/anthropic-just-revealed-3-new-rules-for-claude-skills-bf6d5ff66930
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/ux-planet/anthropic-just-revealed-3-new-rules-for-claude-skills-bf6d5ff66930
  title: Anthropic Just Revealed 3 NEW Rules for Claude Skills
  author: UX Planet
status: stable
source_url: https://medium.com/ux-planet/anthropic-just-revealed-3-new-rules-for-claude-skills-bf6d5ff66930
source: Medium (UX Planet)
source_type: medium
published: '2026-11-01'
domain: ai-ml
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d8144acc7c287c750aae3
lang: pt-br
translation_of: /notes/ai-ml/2026-11-20-anthropic-just-revealed-3-new-rules-for-claude-skills.md
---

## 🎯 Tese central
Skills criadas para os modelos Claude 5.5 (Haiku 5.5, Sonnet 5.5, Opus 5.5) devem seguir três regras atualizadas: colocar as instruções críticas nas primeiras 100 linhas do SKILL.md, ajustar a especificidade das instruções ao risco e à variabilidade de cada tarefa, e adicionar checklists com loops de feedback de validação.

## 🧭 Por que o autor escreveu
A Anthropic atualizou suas melhores práticas para skills junto com os modelos 5.5. O autor argumenta que hábitos de modelos anteriores agora podem custar tokens e gerar saídas genéricas, então as skills existentes precisam ser revisadas à luz das novas regras.

## Introdução
O artigo expõe a premissa: regras que funcionavam com modelos Claude anteriores podem agora prejudicar o desempenho, seja por consumo desnecessário de tokens, seja por saídas genéricas. Ele promete três regras e um prompt pronto para auditar skills existentes.

## 1. Coloque as informações e instruções mais importantes nas primeiras 100 linhas do SKILL.md

### Diretriz de tamanho do SKILL.md
A Anthropic recomenda manter o SKILL.md de uma skill com menos de 500 linhas de texto. O autor observa que essas linhas não têm a mesma importância para o Claude. O restante da regra depende de onde o Claude olha primeiro.

### A prévia das primeiras 100 linhas
Ao executar uma skill, o Claude normalmente lê apenas as primeiras 100 linhas do SKILL.md, por exemplo com `head -100 SKILL.md`, em vez de ler o arquivo inteiro. O autor enfatiza que 100 linhas é uma regra recomendada, não um limite técnico. O contexto crítico (para que serve a skill e como começar) pertence a essas primeiras linhas, e os detalhes secundários podem ocupar as 400 linhas restantes.

```javascript
head -100 SKILL.md
```

**O que isso faz:**
- Mostra o comando que o Claude normalmente executa para pré-visualizar um arquivo de skill.
- Limita a leitura a 100 linhas, então só o topo do arquivo é visto no primeiro momento.
- Explica por que a posição importa: o que estiver abaixo da linha 100 pode nunca ser lido na etapa de prévia.

> ❌ **Antipadrão — Enterrar o contexto crítico (implícito no artigo):** colocar as instruções centrais depois das primeiras 100 linhas, por exemplo após um longo preâmbulo. **Por que as pessoas fazem isso:** escrevem a skill de cima para baixo, como um documento, com o contexto de fundo primeiro. **Consequência:** a prévia pode deixar de fora as instruções que conduzem a tarefa. **Correção:** comece pelo propósito, pelo contexto de acionamento e pelos passos centrais, e depois coloque os detalhes abaixo.

### Divulgação progressiva
Quando é preciso mais informação, o autor recomenda a divulgação progressiva. O SKILL.md referencia outros arquivos Markdown, e o Claude os carrega sob demanda, somente quando precisa. Manter o arquivo principal curto e os detalhes em arquivos linkados permite que a skill carregue mais conhecimento no total sem pagar por isso em cada execução.

> 🔎 Contexto adicionado: o SKILL.md é o arquivo principal de instruções de uma Claude Skill, e o restante da pasta da skill pode conter arquivos de apoio. O padrão do artigo depende de o Claude abrir esses arquivos somente quando um passo precisar deles.

Exemplo do artigo: uma skill de revisão de UX (design-review) cujo Passo 1 aponta para um arquivo PRD.md separado, em vez de colar o conteúdo dele.

```javascript
---
name: design-review
description: conducts design review
---

# Design review

## Review process step by step
Step 1: Check product requiremenet documentation listed in PRD.md
Step 2: Conduct review process
Step 3: Generate a report
```

**O que isso faz:**
- O frontmatter YAML dá à skill um `name` e uma `description` de uma linha que dizem ao Claude quando usá-la.
- O título de nível superior e os passos numerados formam o núcleo curto e legível que a prévia lê.
- O Passo 1 se refere ao PRD.md em vez de incorporar os requisitos do produto, o que mantém o SKILL.md pequeno.
- O texto do passo foi mantido no arquivo exatamente como o autor escreveu, incluindo a grafia "requiremenet".

### Sumário nos arquivos referenciados
Para cada arquivo Markdown que uma skill referencia, o autor recomenda um sumário nas primeiras 100 linhas. O Claude pode ler o sumário para saber o que o arquivo contém, sem ler tudo.

```javascript
...

# What this file contains
1. Product Overview — What we're building and why
2. Problem Statement — What problem we're solving
3. Goals & Success Metrics — What success looks like
4. Target Users — Who we're building for
5. User Stories & Use Cases — How users will interact with the product

...
```

**O que isso faz:**
- As linhas `...` marcam onde o restante do arquivo continua; são marcadores de posição, não sintaxe.
- O título "What this file contains" sinaliza ao Claude o propósito do sumário.
- Cada entrada numerada associa o nome de uma seção a uma finalidade de uma linha, para que o Claude decida qual seção abrir.
- Colocar o sumário nas primeiras 100 linhas permite que a mesma lógica de prévia funcione para os arquivos referenciados.

### Passo prático: audite sua biblioteca de skills
O autor sugere verificar cada skill usada com frequência em duas coisas: se o SKILL.md ultrapassa 500 linhas e quais arquivos Markdown ele linka. O artigo traz um prompt para essa auditoria que proíbe alterações.

```javascript
Check my most-used skills.

For each skill tell me
1. If SKILL.md is over 500 lines
2. If SKILL.md links to any markdown files, list every file it links to

Provide the output as a list

Do not change anything.
```

**O que isso faz:**
- "Check my most-used skills" limita a auditoria às skills que mais importam.
- O item 1 testa a diretriz de 500 linhas em cada SKILL.md.
- O item 2 lista os arquivos linkados, o que mostra se a divulgação progressiva está em uso.
- "Do not change anything" torna a execução somente leitura, para que a auditoria possa ser revisada antes de qualquer edição.

## 2. Defina o grau de liberdade adequado para os modelos de IA da Claude
O artigo define três níveis de prescrição para as instruções de uma skill. Os modelos Claude 5.5 são mais capazes e criativos, então um nível que servia aos modelos antigos pode agora ser rígido demais.

### Alta liberdade
A alta liberdade dá ao Claude instruções gerais sobre o objetivo, sem especificar como alcançá-lo. Serve a tarefas nas quais o próprio julgamento do Claude é o principal recurso.

### Liberdade média
A liberdade média define um objetivo mais limites, como um formato de saída obrigatório. O exemplo do artigo é "Always use this template when creating a user research report" (Sempre use este modelo ao criar um relatório de pesquisa com usuários). O Claude ainda escolhe como fazer o trabalho dentro desses limites.

### Baixa liberdade
A baixa liberdade dá uma sequência muito específica que o Claude deve seguir. O exemplo do artigo é um roteiro de entrevista. Oferece o maior controle e a menor flexibilidade.

### Misturando níveis em uma mesma skill
O autor observa que uma skill pode combinar os três níveis. Em uma skill de pesquisa com usuários, os objetivos da pesquisa podem usar liberdade média, o planejamento do estudo, baixa liberdade, e a síntese, alta liberdade. A regra é ajustar a especificidade à importância e à variabilidade da tarefa.

> ❌ **Antipadrão — Instruções excessivamente prescritivas herdadas:** manter regras rígidas, passo a passo, escritas para modelos anteriores. **Por que as pessoas fazem isso:** regras rígidas pareciam mais seguras e davam mais controle quando os modelos eram menos capazes. **Consequência:** modelos mais novos podem ser limitados por instruções desnecessárias. **Correção:** revise cada instrução, remova as que parecem prescritivas demais e reavalie o grau de liberdade de cada passo.

## 3. Forneça checklists para o Claude
Para skills complexas e de múltiplas etapas, o autor recomenda deixar os passos bem claros e adicionar checklists com o que o Claude deve verificar antes de avançar. O exemplo é um fluxo de síntese de pesquisa com usuários.

> 📋 **Checklist — Fluxo de síntese de pesquisa com usuários**
> - [ ] Passo 1: Revisar as notas das entrevistas e os dados da pesquisa
> - [ ] Passo 2: Identificar padrões recorrentes e dores dos usuários
> - [ ] Passo 3: Agrupar as descobertas em temas principais
> - [ ] Passo 4: Traduzir os insights em oportunidades de design
> - [ ] Passo 5: Validar as descobertas contra as evidências da pesquisa

O exemplo é apresentado com o texto "Use this checklist to track your progress" (Use este checklist para acompanhar seu progresso) sob um título "Research Progress". Cada passo é um portão verificável que impede o Claude de pular etapas.

```javascript
....

## User research synthesis workflow

Use this checklist to track your progress:

Research Progress:
- [ ] Step 1: Review interview notes and research data
- [ ] Step 2: Identify recurring patterns and user pain points
- [ ] Step 3: Group findings into key themes
- [ ] Step 4: Translate insights into design opportunities
- [ ] Step 5: Validate findings against research evidence
```

**O que isso faz:**
- A linha `....` marca conteúdo anterior omitido do arquivo da skill.
- O título nomeia o fluxo de trabalho, para que o Claude saiba qual checklist se aplica.
- A instrução "Use this checklist to track your progress" diz ao Claude para marcar os itens conforme avança.
- O rótulo `Research Progress:` e os cinco itens `- [ ]` formam uma lista ordenada e verificável.
- O texto dos passos cobre o caminho completo, das notas brutas às descobertas validadas, nessa ordem.

### Loops de feedback
Na fase de validação, o autor recomenda um loop de feedback: executar a validação, corrigir os problemas encontrados pelo validador e validar de novo. O validador pode ficar dentro dos arquivos Markdown da skill, então o loop faz parte da própria skill.

> 🔎 Contexto adicionado: um loop de feedback repete uma verificação até que ela passe, em vez de confiar em uma única passagem. O artigo descreve o padrão e não dá uma implementação específica de validador.

## 🧠 Modelos mentais

### A regra das primeiras 100 linhas
**O modelo:** o Claude lê o topo do SKILL.md primeiro, então o topo é a parte mais usada do arquivo.
**Como aplicar:** coloque propósito, contexto de acionamento e passos centrais nas primeiras 100 linhas, e mova os detalhes para arquivos linkados.

### Disco de grau de liberdade
**O modelo:** cada instrução fica em uma escala que vai de objetivo geral (alta liberdade) a script exato (baixa liberdade).
**Como aplicar:** defina o nível pela importância e pela variabilidade da tarefa. Use baixa liberdade para passos fixos e arriscados, e alta liberdade para passos que dependem de julgamento.

### Checklist mais loop de validação
**O modelo:** portões verificáveis e uma checagem que se repete até passar capturam erros que uma única passagem deixa escapar.
**Como aplicar:** adicione um checklist a cada fluxo de trabalho de múltiplas etapas, e combine a etapa de validação com um ciclo de executar, corrigir e executar de novo.

## 🔑 Principais conclusões
- A Anthropic recomenda manter o SKILL.md com menos de 500 linhas, e o autor diz que só as primeiras 100 são lidas de forma confiável na prévia.
- O Claude pré-visualiza as 100 primeiras linhas do SKILL.md, então as instruções críticas devem estar ali, com os detalhes secundários nas 400 linhas restantes ou em arquivos linkados.
- A divulgação progressiva permite que o Claude carregue arquivos Markdown referenciados sob demanda, então os detalhes não custam nada até que sejam necessários.
- Cada arquivo referenciado deve abrir com um sumário dentro das primeiras 100 linhas, para que o Claude possa percorrê-lo.
- O grau de liberdade (alto, médio, baixo) deve corresponder à importância e à variabilidade de cada tarefa, e uma skill pode misturar os três.
- Skills antigas podem ser prescritivas demais para os modelos Claude 5.5, então vale revisar e remover instruções desnecessárias.
- Fluxos de múltiplas etapas precisam de checklists e de um ciclo de validar, corrigir e validar de novo.

## 🏛️ Para o Kenji
O SKILL.md funciona como o ponto de entrada de uma interface que um agente de IA lê. Sua estrutura (o que fica no início, o que é linkado, quão rigidamente cada passo é prescrito) é uma decisão de arquitetura da informação, e uma auditoria da biblioteca de skills se parece com uma revisão de governança de arquitetura. Esta é a sua interpretação, não a do autor. A pergunta que isso levanta: como você mediria se uma skill mais prescritiva produz resultados melhores do que uma mais flexível?

## 📚 Plano de leitura
### No artigo
- Nenhuma fonte externa é citada. Os únicos links do artigo apontam para SKILL.md, um nome de arquivo e não um recurso.

### Para aprofundar
- Documentação da Anthropic sobre Claude Skills e suas melhores práticas — a fonte em que o artigo baseia as regras; o artigo não dá URL, então procure a página oficial.
- Divulgação progressiva como padrão de design em documentação e design de interfaces — um padrão geral que o artigo aplica aos arquivos de skill.
