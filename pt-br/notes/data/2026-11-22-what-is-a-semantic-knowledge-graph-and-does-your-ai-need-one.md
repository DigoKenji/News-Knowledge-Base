---
type: Article Summary
title: O que é um grafo de conhecimento semântico, e a sua IA precisa de um?
description: A IA agêntica precisa de uma camada semântica e de grafos de conhecimento que codifiquem o contexto do negócio, e não apenas dados, para agir de forma confiável.
resource: https://www.cio.com/article/4249101/what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.html
tags:
- data
- knowledge-graph
- rag
- ai-agent
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4249101/what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.html
  title: What is a semantic knowledge graph, and does your AI need one?
  author: CIO.com
status: stable
source_url: https://www.cio.com/article/4249101/what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.html
source: CIO.com
source_type: blog
published: '2026-11-18'
domain: data
concepts:
- knowledge-graph
- rag
- ai-agent
- multi-agent-orchestration
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d811d888bfd346469d409
lang: pt-br
translation_of: /notes/data/2026-11-22-what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.md
---

## 🎯 Tese central
Agentes de IA que realizam trabalho real precisam de uma camada semântica que codifique o significado e o contexto do negócio sobre os dados corporativos, e os grafos de conhecimento são a estrutura que sustenta esse contexto.

## 🧭 Por que o autor escreveu o artigo
Empresas já investiram muito em data lakes, lakehouses, sistemas ERP e CRM, mas o artigo argumenta que dados brutos não explicam o que está acontecendo em um negócio. Se isso for verdade, codificar o contexto do negócio se torna um requisito de arquitetura para a IA agêntica, e não um projeto opcional de analytics.

## Introdução: de tabelas a grafos semânticos
### Banco de dados de grafos
Um banco de dados tradicional organiza as informações em tabelas, com uma coluna para cada atributo, como um nome ou um empregador. Um banco de dados de grafos armazena a mesma informação como nós, como uma pessoa e uma empresa, ligados por arestas que representam a conexão entre eles. Como qualquer nó pode se conectar a qualquer outro, o modelo é flexível, e encontrar entidades relacionadas significa percorrer as arestas. O artigo cita dependências de cadeia de suprimentos, detecção de fraude e mapeamento de preferências de usuários para recomendações como usos típicos.

### Grafo de conhecimento
Um grafo de conhecimento fica sobre um banco de dados de grafos como um mapa de relações que explica o que os dados são. No exemplo do artigo, um nó com o nome de uma pessoa se conecta a um nó com informações de uma empresa, porque a pessoa é funcionária dessa empresa. Portanto, o grafo carrega significado sobre os dados, e não apenas os dados em si. Para os agentes, é isso que permite seguir relações até os fatos de que precisam.

### Grafo de conhecimento semântico
Um grafo semântico, ou grafo de conhecimento semântico, acrescenta um nível mais profundo de significado sobre as relações. Exemplo do artigo: uma lista de empresas pode ser entendida como clientes ou clientes potenciais, e uma lista de pessoas pode ser de contatos potenciais para ligações de vendas, com os cargos indicando sua importância relativa. O significado adicionado é específico para a forma como a empresa usa os seus dados.

### Camada semântica
A camada semântica trata do que os dados significam para a empresa e de como eles são realmente usados na prática. Quando agentes de IA recebem tarefas de trabalho reais, precisam dessa informação. A Cosentino, uma empresa de manufatura cujo CIO global, Rafael Domene, é citado, já havia investido pesadamente em um grande data lake e lakehouse, além de sistemas ERP e CRM. Para Domene: "Você pode coletar todos os dados, mas isso não basta para explicar o que está acontecendo." A camada semântica fornece o contexto de negócio por trás dos dados.

### IA agêntica
Domene a define assim: "IA agêntica é uma IA que consegue fazer coisas, não apenas dizer coisas." Ele acrescenta que fornecer "não apenas dados, mas informação" é o que permite que a IA faça o trabalho da maneira certa, ou seja, com conhecimento de negócio e entendimento do que está acontecendo.

## Camadas justificadas
### Construção da camada semântica
Construir uma camada semântica sempre foi difícil. Ela exige especialistas no assunto, e não é um projeto de uma vez só, porque a forma como uma empresa faz as coisas muda constantemente. Analistas costumavam pular essa etapa, já que o próprio conhecimento que tinham da empresa fornecia o contexto. Agentes de IA são diferentes: eles entendem mal como as empresas funcionam em geral, e sabem ainda menos sobre como uma empresa específica funciona. A Cosentino começou sua camada há três anos e pretende concluí-la até o fim do ano, com os principais processos e fluxos já implementados e o trabalho restante concentrado principalmente na área financeira, incluindo logística, manufatura, operações, atendimento ao cliente, vendas, compras e RH.

### Pedidos atrasados
Domene descreve um processo de pedidos envolvendo 15 departamentos, cada um com seus próprios dashboards, em que a empresa estava "tendo muitos problemas para descobrir, em cima da hora, que um pedido específico não seria atendido." Hoje, um agente de IA pode examinar qualquer pedido de qualquer cliente, prever se ele chegará no prazo e identificar onde está o atraso. O valor não está apenas na informação: se um vendedor fica sabendo de um problema enquanto ele acontece, pode voltar ao cliente, explicar o que aconteceu e oferecer soluções. Domene: "Sem contexto, não consigo conectar as coisas."

### Bloqueios por limite de crédito
Quando um cliente ultrapassava o limite de crédito e um pedido era bloqueado, o atraso médio costumava ser de 22 dias. O artigo informa que o atraso máximo hoje é de um dia. Domene chama isso de "uma melhoria de serviço e de desempenho", e diz que a equipe financeira que cuidava desse trabalho agora está livre para tarefas mais vitais para os clientes.

### Economia e tempo de vendas
Domene diz que a empresa obteve até US$ 10 milhões em economia no primeiro ano do projeto e espera US$ 100 milhões em economia até o fim do ano. Os funcionários de vendas costumavam gastar mais de um terço do tempo com trabalho administrativo, e agentes de IA construídos no ano passado e testados neste ano devem ajudá-los em breve. Domene faz a ressalva de que "nem todo esse tempo economizado será convertido em receita."

### Construção liderada por humanos
O projeto começou como um esforço em grande parte manual, conduzido pelos especialistas da empresa junto com engenheiros da fornecedora, a empresa de process mining Celonis. Domene diz que os fluxos fixos do SAP tornaram o trabalho relativamente fácil, mas ainda assim exigiu esforço humano, e a IA foi sendo acrescentada com o tempo para ampliar as capacidades das pessoas. David Hoelzer, membro da SANS Institute e diretor de operações da Enclave Forensics, acrescenta que os humanos nem sempre criam documentos de forma que a IA consiga usar para entender relações semânticas.

> ❌ **Anti-padrão — camada semântica totalmente autônoma:** deixar a IA construir o grafo de conhecimento sozinha. **Por que as pessoas fazem isso:** o artigo não diz, mas o atrativo é que a IA acelera o trabalho.
> 🔎 Contexto adicionado: a leitura tentadora é que a IA poderia terminar o trabalho sozinha, o que o artigo contesta. **Consequência:** Hoelzer alerta que a IA "pode acabar com relações que não fazem sentido". **Correção:** usar a IA para acelerar a construção do grafo e mantê-lo atualizado, enquanto pessoas com "um conjunto muito particular de habilidades" permanecem no processo.

## Grafos de conhecimento versus bancos de dados vetoriais
### Geração aumentada por recuperação (RAG)
Há cerca de um ano, a forma padrão de alimentar um LLM com informações novas era o RAG combinado com bancos de dados vetoriais. O artigo diz que essa ainda é uma boa abordagem para dados não estruturados em pipelines RAG mais antigos. O contexto relevante de um prompt é incluído na entrada do LLM, e é assim que o modelo obtém a informação de que precisa para a tarefa em questão.

> ❌ **Anti-padrão — contexto recuperado excessivo:** passar grandes volumes de material recuperado em cada chamada ao LLM. **Por que as pessoas fazem isso:** o artigo não diz; mais contexto parece mais seguro. **Consequência:** o material "pode confundir o LLM ou estar desatualizado", e conteúdo em excesso significa mais tokens e custos mais altos, o que importa porque agentes fazem muitas chamadas ao LLM. **Correção:** deixar que os grafos de conhecimento retornem exatamente a informação necessária, com a camada semântica fornecendo o que fazer com ela.

### Banco de dados vetorial
Um banco de dados vetorial recebe informações não estruturadas e as organiza de modo que temas semelhantes fiquem agrupados. Esse agrupamento é o que permite trazer o material certo para dentro de um prompt. O ponto do artigo é que essa abordagem tende a produzir muito material, o que é útil para conteúdo não estruturado, mas menos preciso do que um grafo para o trabalho de agentes.

### Grafo de conhecimento para agentes
Os grafos de conhecimento permitem que agentes encontrem exatamente a informação de que precisam, enquanto a camada semântica lhes dá o conhecimento sobre o que fazer com ela. A New York Life Group Benefits Solutions usa as duas: seu grafo de conhecimento é construído sobre o framework de agentes LangGraph, do LangChain. O CIO Matt Marze afirma: "Os grafos de conhecimento funcionam para a orquestração multiagente, e o banco de dados vetorial para as aplicações de RAG em IA generativa." Ele descreve o grafo partindo de informações básicas do negócio, dados, sinais e fontes externas, como a comunidade de corretores, com a camada semântica acrescentando entendimento de negócio, relações, definições comuns de produtos e planos, e a situação atual, o histórico, o fluxo de trabalho e as prioridades. "Tudo isso leva a insights, previsões e recomendações, e permite coordenar as capacidades dos agentes de IA."

### Orquestração multiagente
No uso de Marze, o grafo de conhecimento é o que coordena as capacidades de vários agentes.
> 🔎 Contexto adicionado: orquestração, aqui, significa decidir qual agente cuida de qual parte de uma tarefa e em que ordem.

### Interesse crescente
James Kaplan, sócio distinto da McKinsey & Company, diz que há um ano e meio as pessoas "olhavam para mim como se eu tivesse três cabeças" quando ele falava sobre grafos de conhecimento. No último ano, ele percebeu um aumento real de interesse, e afirma que as instituições podem usar grafos de conhecimento para decisões mais complexas e, com agentes, para tornar os agentes mais capazes.

## O outro lado
### Alucinação e imprecisão acumulada
Precisão e alucinações são um grande problema para modelos de IA em geral e, em sistemas agênticos com várias chamadas ao LLM, imprecisões podem se acumular rapidamente. Zakir Hussain, sócio e líder de dados e IA das Américas para os setores industrial e de energia da EY, explica que todos esses modelos são probabilísticos. Quando usados para questões ou decisões em nível corporativo, "eles começam a alucinar porque, em muitos casos, não entendem ou interpretam mal o contexto." O treinamento genérico torna mais difíceis as perguntas específicas sobre uma empresa: "Assim que se torna específico para uma empresa, é aí que as coisas começam a dar errado."

### Resultado de precisão
De acordo com um artigo publicado no fim de 2025 por pesquisadores da UC Santa Barbara e da pesquisa em IA da JP Morgan, grafos de conhecimento melhoram a precisão em 26% ou mais em relação a linhas de base padrão de chain-of-thought. O artigo não oferece mais detalhes sobre o benchmark, as tarefas ou as linhas de base, então trate o número como o artigo o apresenta.

### Codificar o contexto da empresa
Hussain diz que toda organização está tentando usar a IA para crescimento, eficiência e produtividade, e que "para desbloquear essas três camadas — crescimento, eficiência e produtividade — você precisa codificar o contexto da empresa." Ele considera a conversa sobre ontologia, camada semântica e grafo de conhecimento "bastante importante neste momento."

## 🧠 Modelos mentais
### Dados não são informação
**O modelo:** Coletar dados não explica o que aconteceu. A informação precisa de contexto de negócio, significado e entendimento de como a empresa funciona.
**Como aplicar:** Quando uma métrica fica vermelha, pergunte quais processos e contexto a explicam antes de acrescentar mais fontes de dados.

### IA agêntica faz trabalho, não apenas relatórios
**O modelo:** IA agêntica é uma IA que consegue fazer coisas, não apenas dizer coisas (Domene).
**Como aplicar:** Avalie um agente pelas ações que ele consegue concluir de forma confiável, e forneça o contexto de negócio que essas ações exigem.

### Modelos probabilísticos precisam de contexto codificado
**O modelo:** Modelos probabilísticos alucinam em questões corporativas quando interpretam mal o contexto, então o contexto da empresa precisa ser explicitado (Hussain).
**Como aplicar:** Antes de confiar nas respostas de um agente sobre questões específicas da empresa, registre as definições, relações e prioridades da empresa em uma forma que o sistema consiga usar.

## 🔑 Principais conclusões
- Agentes que atuam sobre dados de negócio precisam de uma camada semântica que forneça significado, processos e contexto específicos da empresa; o artigo argumenta que dados brutos não bastam.
- Na Cosentino, o atraso médio de 22 dias em pedidos bloqueados por limite de crédito agora é de no máximo um dia, com até US$ 10 milhões economizados no primeiro ano e US$ 100 milhões esperados até o fim do ano.
- Construir a camada semântica é um trabalho contínuo e conduzido por especialistas; a IA pode acelerar e manter o grafo de conhecimento, mas não deve construí-lo sozinha.
- Grafos de conhecimento permitem que agentes encontrem informações exatas, enquanto o contexto excessivo do RAG adiciona tokens, custo e possível confusão.
- A New York Life Group Benefits Solutions combina grafos de conhecimento para orquestração multiagente com bancos de dados vetoriais para RAG em IA generativa.
- Um artigo publicado no fim de 2025 pela UC Santa Barbara e pela pesquisa em IA da JP Morgan relata que grafos de conhecimento melhoram a precisão em 26% ou mais em relação a linhas de base padrão de chain-of-thought.
- Modelos probabilísticos alucinam em questões específicas da empresa, e as imprecisões se acumulam em sistemas agênticos com várias chamadas, por isso o contexto da empresa precisa ser codificado.

## 🏛️ Para o Kenji
Para um Staff Architect que decide como os agentes obtêm contexto corporativo, a camada semântica parece um componente arquitetural governado, com donos, definições de negócio e manutenção contínua, que convive com a recuperação vetorial em vez de substituí-la. A questão que ela levanta é quais definições e relações de negócio devem ser codificadas primeiro, e quem as mantém à medida que os processos mudam. Esta é a sua interpretação, não a do autor.

## 📚 Plano de leitura
### A partir do artigo
- O corpo do artigo não contém links, então não há nenhum a listar.

### Para aprofundar
- Artigo de pesquisa da UC Santa Barbara e da pesquisa em IA da JP Morgan, publicado no fim de 2025 — a fonte do número de 26% de precisão; verifique o benchmark e as linhas de base antes de confiar no número.
- LangGraph, o framework de agentes do LangChain sobre o qual a New York Life constrói seu grafo de conhecimento — uma visão concreta de um framework de agentes baseado em grafos.
- Process mining da Celonis — a fornecedora com quem a Cosentino trabalhou para construir seu grafo de conhecimento e sua camada semântica.
- Geração aumentada por recuperação com bancos de dados vetoriais — a abordagem mais antiga que o artigo contrapõe aos grafos de conhecimento para o trabalho de agentes.

## Conceitos
- [Grafo de conhecimento](/pt-br/concepts/data/knowledge-graph.md)
- [Geração aumentada por recuperação](/pt-br/concepts/ai-ml/rag.md)
- [Agente de IA](/pt-br/concepts/ai-ml/ai-agent.md)
- [Orquestração multiagente](/pt-br/concepts/ai-ml/multi-agent-orchestration.md)
