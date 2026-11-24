---
type: Article Summary
title: O Agente Jurídico de IA da Uber Reduz em 20% o Tempo de Revisão de Contratos com Regras e RAG
description: O Agente de Redlining Jurídico da Uber reduz em mais de 20% o tempo de revisão de contratos ao combinar regras determinísticas com RAG sobre o feedback dos advogados, mantendo os advogados no controle.
resource: https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
tags:
- ai-ml
- ai-agents
- rag
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
  title: Uber's Legal AI Agent Cuts Contract Review Time by 20% with Rules and RAG
  author: Leela Kumili
status: stable
source_url: https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
source: InfoQ
source_type: blog
published: '2026-11-16'
domain: ai-ml
subdomain: ai-agents
concepts:
- rag
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d817c9a4bf174b09ef2f1
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-uber-s-legal-ai-agent-cuts-contract-review-time-by-20-with-r.md
---

# O Agente Jurídico de IA da Uber Reduz em 20% o Tempo de Revisão de Contratos com Regras e RAG

**Fonte:** https://www.infoq.com/news/2026/11/uber-legal-redlining-agent/
**Autor(a):** Leela Kumili
**Data:** 2026-11-16
**Conceitos:** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)

---

## 🎯 Tese central
O Agente de Redlining Jurídico da Uber reduz em mais de 20% o tempo médio de revisão de contratos ao combinar recuperação sobre decisões anteriores de advogados, regras jurídicas determinísticas e modelos de linguagem, enquanto os advogados continuam responsáveis por aprovar cada recomendação.

## 🧭 Por que o autor escreveu
A revisão de contratos é repetitiva: partes externas propõem alterações, e os advogados precisam julgar cada mudança com base na política da empresa. O artigo descreve como a Uber construiu um agente para essa etapa, como o design mudou depois que uma primeira tentativa ficou aquém do esperado e o que a empresa relata. Se a tese se sustentar, a revisão jurídica de rotina pode ser acelerada quando precedentes e regras rígidas moldam a saída do modelo e os advogados permanecem no controle.

## O que o agente faz
O agente é entregue como um complemento (add-in) dentro do Microsoft Word. Ele lê as alterações propostas por partes externas, interpreta a intenção delas, compara cada mudança com as políticas jurídicas e sugere se a cláusula deve ser aceita, rejeitada ou modificada. Ele também escreve comentários explicando cada recomendação e sinaliza as mudanças que merecem uma análise mais cuidadosa. A Uber começou a primeira geração no início de 2024 e fez um piloto com sua equipe jurídica em 2025. Segundo a empresa, o tempo médio de revisão de contratos caiu mais de 20%, com 91% de acurácia nas decisões geradas por IA. O artigo não explica como esses números foram medidos.

> 🔎 Contexto adicionado: redlining significa marcar as alterações propostas em um contrato. No Word, isso geralmente é feito com edições controladas (tracked changes) e comentários.

### Agente de redlining
O agente trata cada alteração externa como uma pergunta: o que essa mudança tenta alcançar e ela se encaixa na política? A saída dele é uma sugestão (aceitar, rejeitar ou modificar), uma explicação em um comentário do Word e um sinal quando um humano deve olhar com mais atenção. O agente não finaliza o contrato. Ele produz uma proposta que um advogado revisa dentro do documento que já usa.

### Revisão com humano no loop
Os advogados mantêm a responsabilidade por revisar e aprovar cada recomendação. O artigo apresenta isso como central no design, e não como uma formalidade. A equipe jurídica também define o tom e o estilo desejados, então as pessoas responsáveis pelo resultado moldam a forma como o agente se expressa.

## Da primeira pipeline de RAG a um ciclo de feedback
A primeira versão usava geração aumentada por recuperação. Ela buscava seções relevantes de playbooks jurídicos para orientar as respostas do modelo. O sistema tinha dificuldade com a acurácia da recuperação semântica, com o tom da negociação e com cenários que nenhum exemplo existente cobria. Em seguida, a Uber acrescentou um ciclo de feedback que captura a decisão de cada advogado, o raciocínio por trás dela e o texto final editado.

### Geração aumentada por recuperação (RAG)
> 🔎 Contexto adicionado: o RAG combina uma etapa de busca com um modelo de linguagem. O texto relevante é recuperado e colocado no prompt, para que a resposta se baseie nele.

Neste artigo, RAG significava recuperar seções de playbooks para orientar a redação do modelo. A fraqueza estava na qualidade da recuperação: o sistema não encontrava de forma confiável a passagem certa, e os playbooks escritos não cobriam todos os casos de negociação. O artigo não traz nenhuma métrica de recuperação para essa versão. A lição é que fundamentar um modelo em documentos só é tão bom quanto a etapa de recuperação, e que um corpus de políticas sozinho pode não capturar o julgamento prático.

### Ciclo de feedback dos advogados
O ciclo registra três coisas para cada decisão: o que o advogado escolheu, por quê e o texto que ele efetivamente usou. Esses registros se tornam material que o agente pode recuperar em revisões posteriores. A mudança-chave é que o agente recupera exemplos de como cláusulas semelhantes foram tratadas, e não apenas se uma recomendação anterior foi aceita ou rejeitada.

> ❌ **Anti-padrão: armazenar apenas os desfechos de aceitar/rejeitar:** manter apenas se uma recomendação passada foi aceita ou rejeitada. **Por que as pessoas fazem isso:** não informado no artigo. **Consequência:** não detalhada; o artigo relata apenas que a primeira versão de RAG tinha dificuldade com a acurácia da recuperação e com o tom, e que o ciclo mais rico substituiu a dependência dos sinais de aceitar/rejeitar. **Correção:** capturar o raciocínio e o texto final editado, para que o sistema possa mostrar como uma cláusula semelhante foi de fato tratada.

## Recuperando precedentes em tempo de execução
No momento da consulta, o agente restringe o histórico em etapas. A busca por similaridade e a filtragem por metadados reduzem milhares de interações históricas a cerca de 20 candidatos. Uma segunda etapa, baseada em modelo de linguagem, avalia a relevância deles. A ponderação por decaimento exponencial, com meia-vida configurável atualmente definida em 365 dias, favorece as decisões recentes sem descartar as antigas. Especialistas do domínio também controlam a linguagem e o estilo de negociação.

### Busca por similaridade e filtragem por metadados
A busca por similaridade encontra registros armazenados cujos embeddings estão próximos do embedding da cláusula atual. A filtragem por metadados restringe essa busca usando atributos associados a cada registro; o artigo não lista quais atributos a Uber usa. Juntas, elas fazem o primeiro corte, de milhares de interações para cerca de 20 candidatos. Isso importa porque a segunda etapa só consegue julgar o que a primeira deixa passar.

### Filtragem de relevância baseada em LLM
A segunda etapa pede a um modelo de linguagem que julgue se cada um dos cerca de 20 candidatos é relevante para a alteração atual. O artigo não descreve o prompt nem os critérios. Ele dá a essa etapa um papel mais restrito do que o da primeira: precisão acima de cobertura, aplicada a um conjunto pequeno.

> 🔎 Contexto adicionado: isso se parece com o padrão LLM-as-a-judge, em que um modelo pontua candidatos segundo critérios declarados. O artigo não usa esse termo.

### Ponderação por decaimento exponencial
A ponderação por decaimento exponencial atribui a cada registro um peso que cai pela metade a cada meia-vida decorrida. Com a meia-vida de 365 dias do artigo, uma decisão de um ano atrás conta metade do que uma tomada hoje.

> 🔎 Contexto adicionado: o peso é 0,5 elevado à potência (idade em dias dividida por 365). A meia-vida é configurável, então os 365 dias do artigo são uma configuração, não um valor universal.

## Divisão de equipes e arquitetura
A Uber Engineering enquadrou o problema em um post no LinkedIn: "Como a IA pode simplificar a revisão repetitiva de contratos mantendo os advogados no controle?" A divisão do trabalho é explícita. A equipe jurídica define o tom e o estilo desejados, enquanto os engenheiros mantêm a estrutura dos prompts e o fluxo da aplicação. O sistema combina um complemento do Word em React e TypeScript com um backend em Python.

### Orquestração com LangGraph
O backend em Python é orquestrado com LangGraph. O artigo não diz por que esse framework foi escolhido em vez de alternativas.

> 🔎 Contexto adicionado: o LangGraph permite definir um fluxo de LLM como um grafo de etapas que compartilham estado, o que se adapta bem a uma lógica de revisão de várias etapas.

### Nós de fluxo em paralelo
Vários nós do fluxo rodam em paralelo: detecção de intenção, avaliação de risco e consulta de políticas. Executá-los simultaneamente significa que o agente não espera uma análise terminar para começar outra. O artigo não traz números de latência.

### OpenSearch
O OpenSearch armazena as regras jurídicas vetorizadas e o feedback histórico. Ele é a camada de recuperação por trás da busca por similaridade descrita acima.

> 🔎 Contexto adicionado: o OpenSearch é um mecanismo de busca e análise de código aberto que pode indexar vetores para consultas por similaridade.

### Estado de sessão no Redis
O Redis mantém o estado da sessão para que os usuários possam fazer perguntas de acompanhamento sobre a mesma revisão sem reiniciar o contexto.

> 🔎 Contexto adicionado: o Redis é um armazenamento de dados em memória, frequentemente usado para estados de curta duração, como sessões.

## Regras determinísticas e verificação
Um motor de regras determinístico impõe os requisitos jurídicos não negociáveis e sobrepõe-se às recomendações do modelo sempre que uma regra correspondente se aplica. Os modelos de linguagem cuidam das tarefas que exigem interpretação contextual, incluindo recomendações de cláusulas e o tom da negociação. Uma etapa final de verificação confere as respostas geradas contra as regras recuperadas.

### Motor de regras determinístico
Um motor de regras aplica condições fixas, então a mesma entrada sempre produz o mesmo resultado. Colocar os requisitos não negociáveis aqui garante que eles valham mesmo quando a sugestão de um modelo seria diferente. O artigo deixa a divisão explícita: regras para o que precisa ser verdadeiro, modelos para o que exige julgamento. O artigo não lista as regras nem explica como elas são escritas.

> ❌ **Anti-padrão: deixar o modelo decidir os não negociáveis:** deixar as cláusulas obrigatórias ao julgamento do modelo. **Por que as pessoas fazem isso:** não informado no artigo. **Consequência:** não informada no artigo; o design sugere que o modelo poderia, de outro modo, sobrepor-se a um requisito legal. **Correção:** codificar os requisitos não negociáveis como regras determinísticas que sobrepõem a saída do modelo.

### Etapa final de verificação
Antes que uma resposta seja devolvida, o sistema a confere com as regras recuperadas. O artigo não descreve as verificações, o tratamento de falhas nem com que frequência elas sobrepõem a saída.

## Próximos passos
A Uber está explorando harnesses agênticos, incluindo Claude Code e OpenCode, além de skills que estruturam e automatizam fluxos de trabalho em suas iniciativas de IA agêntica. A empresa também planeja investigar ontologias jurídicas e grafos de conhecimento para representar conceitos e relações do direito.

### Harnesses agênticos
Um harness é o runtime que permite a um modelo usar ferramentas e seguir um fluxo de trabalho. O artigo cita Claude Code e OpenCode como opções em avaliação, e skills como forma de estruturar trabalho repetível.

### Ontologias jurídicas e grafos de conhecimento
Uma ontologia define os conceitos de um domínio e como eles se relacionam. Um grafo de conhecimento armazena essas relações como dados. O artigo as apresenta como uma futura base semântica para uma interpretação e um raciocínio mais consistentes, e não como um componente atual.

## 🧠 Modelos mentais
### Precedente acima de veredictos
**O modelo:** uma decisão armazenada é mais útil como um exemplo trabalhado (o que foi pedido, o que foi decidido, por quê e o texto final), e não como um rótulo binário de aceitar ou rejeitar.
**Como aplicar:** ao projetar qualquer captura de feedback para um trabalho assistido por IA, registre o raciocínio e a saída final junto com o veredicto.

### Regras acima do modelo
**O modelo:** requisitos não negociáveis pertencem a um código determinístico que sobrepõe o modelo, e o modelo cuida da interpretação e do tom.
**Como aplicar:** liste cada requisito que nunca pode ser violado, implemente-o como regra e confira a saída do modelo contra essas regras antes que ela chegue a um usuário.

### O especialista é dono do resultado
**O modelo:** a IA propõe, o especialista do domínio aprova, e o especialista também define o estilo que a IA usa.
**Como aplicar:** dê aos especialistas do domínio controle direto sobre as partes do sistema que refletem o julgamento deles e mantenha uma etapa explícita de aprovação.

## 🔑 Principais takeaways
- A Uber relata que seu Agente de Redlining Jurídico reduz em mais de 20% o tempo médio de revisão de contratos, com 91% de acurácia nas decisões geradas por IA; ambos os números são alegações da empresa.
- Um RAG simples sobre playbooks teve dificuldade com a acurácia da recuperação, o tom da negociação e os casos não cobertos, o que levou ao ciclo de feedback.
- Registrar o raciocínio e o texto final editado, e não apenas os desfechos de aceitar ou rejeitar, permite que o agente recupere como cláusulas semelhantes foram tratadas.
- A recuperação em tempo de execução reduz milhares de interações a cerca de 20 candidatos com busca por similaridade e filtragem por metadados, e depois uma etapa com LLM julga a relevância.
- A ponderação por decaimento exponencial com meia-vida de 365 dias favorece decisões recentes sem descartar precedentes mais antigos.
- Um motor de regras determinístico impõe os requisitos não negociáveis e sobrepõe as recomendações do modelo, com uma etapa final de verificação.
- Os advogados continuam responsáveis pela aprovação; a equipe jurídica define o tom, e os engenheiros são donos da estrutura dos prompts e do fluxo.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. O padrão se aplica a qualquer fluxo de revisão em nível de Staff em que decisões de especialistas são registradas apenas como desfechos, o que as torna um precedente fraco. O artigo também mostra uma divisão clara entre regras determinísticas e julgamento do modelo, que se mapeia diretamente para controles de governança. A pergunta que ele levanta: nos seus próprios sistemas de IA, quais requisitos são impostos por regras que não podem ser sobrepostas, e quais são deixados a um modelo que pode sofrer deriva?

## 📚 Plano de leitura
### Do artigo
- [Post no blog da Uber: Building Uber's redlining agent](https://www.uber.com/us/en/blog/building-ubers-redlining-agent/) — a fonte primária, com os diagramas de arquitetura que este resumo omite.
- [Post da Uber Engineering no LinkedIn](https://www.linkedin.com/posts/how-can-ai-help-streamline-repetitive-contract-share-7513997846849732610-vbbo/) — a pergunta que enquadra o tema e o resumo da própria equipe.
- [LangGraph](https://www.langchain.com/langgraph) — o framework de orquestração usado no backend.
- [OpenSearch](https://opensearch.org/) — a busca e o armazenamento vetorial que guardam as regras e o feedback.
### Para ir além
- Geração aumentada por recuperação — como a qualidade da recuperação limita as respostas fundamentadas.
- Ponderação por decaimento exponencial — como a recuperação ponderada por recência é ajustada na prática.
- Ontologias jurídicas e grafos de conhecimento — a camada de representação que a Uber planeja explorar.
