---
type: Article Summary
title: 'Netflix no QConSF 2026: adaptando o SDLC para agentes de codificação com IA'
description: A Netflix Commerce tornou seguro colocar em produção agentes de codificação com IA ao transformar decisões de arquitetura, documentação e regras de revisão em contexto que agentes e engenheiros leem.
resource: https://www.infoq.com/news/2026/11/netflix-sdlc-ai-coding-agents
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/netflix-sdlc-ai-coding-agents
  title: 'Netflix at QConSF 2026: Retrofitting the SDLC for AI Coding Agents'
  author: Andrew Hoblitzell
status: stable
source_url: https://www.infoq.com/news/2026/11/netflix-sdlc-ai-coding-agents
source: InfoQ
source_type: blog
published: '2026-11-23'
domain: software-engineering
concepts:
- ai-agent
- adr
- graph-rag
- model-context-protocol
- context-engineering
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81e5ab58c4d2ff5685e4
lang: pt-br
translation_of: /notes/software-engineering/2026-11-24-netflix-at-qconsf-2026-retrofitting-the-sdlc-for-ai-coding-a.md
---

## 🎯 Tese central
Fazer agentes de codificação com IA e engenheiros entregarem juntos, com segurança, é sobretudo um problema de documentação, consistência e revisão, e não um problema dos próprios agentes.

## 🧭 Por que o autor escreveu
Khuzam parte de uma lacuna: os agentes prometiam velocidade, mas a equipe de Commerce da Netflix havia passado 20 anos construindo seu código, sua documentação e seus testes para leitores humanos. A palestra descreve como a equipe reformulou seu ciclo de desenvolvimento de software para que agentes e engenheiros pudessem entregar juntos. Se a tese se sustentar, as correções são boa engenharia comum, voltada a um novo tipo de leitor.

> 🔎 Contexto adicionado: O ciclo de vida de desenvolvimento de software (SDLC) é o caminho que o código percorre desde o planejamento até a construção, o teste, a revisão e a publicação.

## Cenário: Commerce e UI orientada pelo servidor
Mark Khuzam, engenheiro de software sênior na Netflix, apresentou isso na QCon San Francisco 2026. A Commerce executa grande parte de sua interface como UI orientada pelo servidor, abordagem que a Netflix já discutiu em QCon anteriores. Um único serviço de composição de telas, atrás do API gateway, monta as telas para iOS, Android, web e TV.

### UI orientada pelo servidor
> 🔎 Contexto adicionado: Na UI orientada pelo servidor, o servidor descreve o que uma tela contém e cada cliente renderiza essa descrição, em vez de cada aplicativo definir suas próprias telas.

A Commerce usa esse padrão em escala. Como um mesmo serviço de composição alimenta quatro plataformas, uma mudança nele chega a todas elas. O artigo não explica o mecanismo em mais detalhes e aponta para palestras anteriores da QCon sobre o assunto.

## Premissa inicial: agentes prometiam velocidade
Khuzam abriu com um e-mail simulado da empresa anunciando que todos os engenheiros agora tinham o Claude Code e perguntando o quanto todos estavam entregando mais rápido. A frase com que ele enquadrou o problema foi: "Agentes prometiam velocidade. Mas passamos 20 anos construindo para humanos." O restante da palestra trabalha essa incompatibilidade. O código, a documentação e as convenções foram feitos para leitores humanos, e os agentes precisaram trabalhar com eles como estavam.

### Janela de contexto
A janela de contexto é a quantidade de texto que um modelo consegue considerar de uma vez. No artigo, ela é a restrição que derrubou a primeira tentativa da equipe: a janela de contexto do agente se esgotou antes que ele encontrasse a página de documentação certa. Por isso, o agente precisa chegar cedo ao material relevante, porque não consegue ler tudo. A documentação precisa ser localizável, não apenas existir.

## Primeira tentativa: um agente migra testes
A primeira tentativa da equipe foi pedir a um agente que migrasse testes, e foram necessárias quatro tentativas. Cada uma acrescentou orientações: primeiro "migre este arquivo", depois instruções, depois as APIs a usar e, por fim, como simular dados. Surgiram mais dois problemas. A documentação era difícil de encontrar e era inconsistente, porque os testes não eram escritos do mesmo jeito em toda a base de código.

> ❌ **Anti-padrão — Convenções inconsistentes:** o mesmo tipo de teste é escrito de um jeito diferente em cada parte da base de código. **Por que as pessoas fazem isso:** o artigo não dá motivo; isto é uma interpretação, já que nenhum padrão escrito está sendo seguido. **Consequência:** o agente não tem um único padrão para copiar, então cada tarefa exige mais orientação. **Correção:** escrever as convenções como registros de decisão, o que a equipe fez.

### Net Promoter Score
Os desenvolvedores avaliaram o framework interno com um Net Promoter Score (NPS) de -3. Passar de um teste para uma funcionalidade inteira, da tela aos dados, da rota ao teste, exigia ensinar ao agente como a equipe constrói software. A pontuação baixa é a linha de base que o restante da história melhora.

> 🔎 Contexto adicionado: O NPS é calculado a partir da pergunta "você recomendaria?" e varia de -100 a +100. Pontuações abaixo de zero significam mais detratores do que promotores.

## Engenharia de contexto e registros de decisão
Khuzam descreveu a correção como engenharia de contexto, "colocar o que os engenheiros sabem no que os agentes veem". A equipe escreveu 45 registros de decisão de arquitetura (ADRs), cada um "escrito para pessoas e para agentes". Cada registro traz o contexto, a decisão, um padrão e um anti-padrão, além de notas de uso de IA sobre o que um agente deve fazer. Também linka documentos e código relacionados, e os registros hoje vêm dentro de quase todos os repositórios.

### Engenharia de contexto
> 🔎 Contexto adicionado: Engenharia de contexto, aqui, significa decidir que informação um agente vê, e em que forma, quando trabalha na sua base de código.

A expressão abrange duas metades. O conhecimento precisa existir por escrito e precisa chegar ao agente no momento em que é necessário. Os engenheiros já sabem como a equipe constrói software; o trabalho é tornar esse conhecimento disponível para um agente. O artigo oferece essa expressão como sua definição e não dá uma definição formal.

### Registro de decisão de arquitetura
Um ADR registra uma decisão arquitetural junto com o contexto que levou a ela. A versão da equipe acrescenta um padrão a seguir, um anti-padrão a evitar e notas sobre o que um agente deve fazer. O artigo apresenta esses campos como o que permite a um agente agir sobre um registro, e os links ligam cada regra ao código que ela governa. Os 45 registros são a base que o agente de revisão verifica depois.

## Localizabilidade: AGENTS.md como robots.txt
Para manter os registros e mais de 300 outros documentos localizáveis, a equipe se inspirou em como os buscadores leem um site. Um arquivo AGENTS.md, o arquivo de instruções do projeto que agentes de codificação leem antes de qualquer coisa, faz o papel do robots.txt.

### AGENTS.md
> 🔎 Contexto adicionado: AGENTS.md é um arquivo simples na raiz de um repositório que diz aos agentes de codificação como trabalhar ali. O artigo o descreve como a primeira coisa que um agente de codificação lê.

Um agente que começa por esse arquivo consegue encontrar os 45 ADRs e os outros mais de 300 documentos, em vez de depender de busca dentro de uma janela de contexto limitada. O artigo não mostra o conteúdo do arquivo, então não se deve presumir nenhum layout específico.

### robots.txt
> 🔎 Contexto adicionado: robots.txt é um pequeno arquivo na raiz de um site que diz aos rastreadores de buscadores para onde eles podem ir.

A analogia funciona porque o robots.txt é um ponto de entrada conhecido, que os rastreadores leem primeiro. A equipe aplicou a mesma lógica para que os agentes encontrem primeiro o material importante.

## Depois do código: CI determinística e um agente de revisão
Khuzam observou que o pipeline de CI continua determinístico, executando builds, testes, comparação de capturas de tela, filas de merge e canários. O que mudou foi como os agentes se conectam a cada fase. Um agente de revisão faz uma pré-revisão de cada pull request com base na documentação e nos ADRs, sinaliza anti-padrões e verifica o que depende da mudança. Os engenheiros podem adicionar um rótulo `revise` para que um agente leia a revisão e envie uma correção para o mesmo PR. As mudanças ainda não são mescladas automaticamente, e os engenheiros são donos do código.

### Integração contínua (CI)
> 🔎 Contexto adicionado: CI é um pipeline automatizado que constrói e testa cada mudança antes de ela ser mesclada.

A equipe manteve esse pipeline como estava. Os agentes são acrescentados em torno das verificações, e não no lugar delas. O artigo trata as verificações determinísticas como a base sobre a qual o trabalho dos agentes se apoia.

### Agente de revisão
A primeira versão do agente de revisão deixou 23 comentários em um único PR, começando com "No geral, este PR está ótimo!"

> ❌ **Anti-padrão — Comentar sobre tudo:** o agente de revisão publica cada observação, inclusive elogios. **Por que as pessoas fazem isso:** o artigo não dá motivo; o resultado da primeira versão sugere que ela tentou cobrir tudo. **Consequência:** 23 comentários enterram os poucos que importam. **Correção:** publicar apenas bloqueios verificados, cada um com evidência, uma correção e a regra do ADR que ele viola.

A versão atual publica apenas esses bloqueios verificados.

### Rótulo revise
Adicionar o rótulo `revise` pede a um agente que leia a revisão e envie uma correção para o mesmo PR. O artigo descreve o mecanismo, mas não relata resultados para ele, e não diz com que frequência é usado.

## Resultados: o que Khuzam relatou
Khuzam relatou nenhum incidente Sev 1 em 2026 em um serviço central, 32% mais pull requests em relação ao ano anterior e um NPS de desenvolvedores subindo de -3 para +25 ao longo de três trimestres. O -3 anterior era a pontuação do framework interno. O artigo não detalha como esses números foram medidos.

Antes da sessão, Khuzam enquadrou o que está em jogo no LinkedIn: "Agentes podem escrever código, mas o que nos dá confiança para colocar serviços Tier 0 em produção para 300M+ de membros sem que tudo desmorone?"

> 🔎 Contexto adicionado: Tier 0 normalmente denota a classe mais crítica de serviço no esquema de classificação de uma empresa. O artigo não define o termo.

## Contexto entre serviços: o que não se sustentou
O trabalho que cruza serviços levantou uma pergunta mais difícil. Um engenheiro de produto que constrói uma funcionalidade toca na API de pagamentos, no faturamento, no gateway e em todos os clientes, então a equipe precisou decidir quem é dono do contexto e como ele acompanha o trabalho. Khuzam listou quatro abordagens que não se sustentaram.

### Carregar todos os repositórios
> ❌ **Anti-padrão — Carregar tudo:** colocar todos os repositórios no contexto do agente. **Por que as pessoas fazem isso:** o artigo não dá motivo; é a forma mais simples de mostrar a um agente o sistema inteiro (interpretação). **Consequência:** o artigo diz que isso não coube. **Correção:** um grafo restrito a um domínio, fixado em commits.

### Submódulos Git
Submódulos Git aninham um repositório dentro de outro. O artigo os chama de frágeis e não dá detalhes. Para o contexto entre serviços, essa fragilidade é um problema porque a visão do agente depende de muitos repositórios aninhados que precisam continuar sincronizados (interpretação).

### Servidor MCP para Git
Um servidor MCP para Git, que expõe o conteúdo do repositório a um agente pelo Model Context Protocol, "vê um arquivo, não o sistema". Ler arquivos um de cada vez não revela como os serviços se conectam.

> 🔎 Contexto adicionado: MCP é o padrão aberto usado para conectar agentes a ferramentas e dados; aqui, é a forma como um repositório é exposto ao agente.

### Cópias compartilhadas
Cópias compartilhadas de contexto ficaram desatualizadas. Uma cópia feita em um momento deixa de corresponder ao código e à documentação de onde veio. A correção da equipe é o oposto: grafos fixados em commits específicos e mantidos atualizados.

## GraphRAG por domínio
A equipe precisava de um contexto que fosse preciso, portável entre agentes, de propriedade de cada domínio e mantido atualizado. Escolheu GraphRAG por domínio, tirando a estrutura do código e o significado da documentação e dos ADRs. Cada grafo é fixado em commits específicos e servido por uma única ferramenta MCP. Claude Code, Cursor, Codex, um bot do Slack e a revisão de CI consultam o mesmo grafo, e toda resposta cita um arquivo e uma linha.

### GraphRAG
> 🔎 Contexto adicionado: GraphRAG combina um grafo de entidades e relações com recuperação de informação, de modo que um agente recebe fatos conectados e também trechos de texto.

Separar o trabalho dessa forma significa que a estrutura vem do código e o significado vem das decisões escritas. Fixar cada grafo em commits liga as respostas a um estado conhecido do código. Ser dono de cada grafo por domínio significa que a equipe responsável por um domínio mantém o contexto dele. Citar um arquivo e uma linha em cada resposta permite que um revisor a verifique.

### Model Context Protocol
> 🔎 Contexto adicionado: Model Context Protocol é um padrão aberto que permite que aplicações de IA se conectem a ferramentas e fontes de dados por meio de uma interface comum.

Uma única ferramenta na frente do grafo permite que cinco clientes diferentes, de agentes de codificação a um bot do Slack e à revisão de CI, consultem a mesma fonte. Cada cliente não precisa de uma cópia própria do contexto.

## Regras finais e julgamento humano
Khuzam encerrou com três regras: decida o que os agentes devem fazer e diga a eles, facilite encontrar como fazê-lo e lembre que o julgamento humano continua importando. Ele resumiu: "Nada disso é novo. É boa engenharia, feita para um novo leitor." O artigo diz que o InfoQ publicará o vídeo da sessão nas próximas semanas.

### Julgamento humano
A última regra tem o maior peso na prática descrita pelo próprio artigo. As mudanças não são mescladas automaticamente, e os engenheiros são donos do código. Os agentes propõem e verificam, e as pessoas decidem o que vai para produção.

## 🧠 Modelos mentais
### Escreva para pessoas e para agentes
**O modelo:** um registro de decisão atende a dois leitores ao mesmo tempo: o engenheiro que precisa da justificativa e o agente que precisa de algo sobre o que possa agir. **Como aplicar:** dê a cada registro um contexto, uma decisão, um padrão, um anti-padrão e notas sobre o que um agente deve fazer, e linke-o ao código que ele governa.

### Um arquivo de entrada como robots.txt
**O modelo:** um arquivo conhecido na raiz diz a qualquer leitor, humano ou agente, onde está o material importante e como lê-lo. **Como aplicar:** mantenha um AGENTS.md em cada repositório que aponte para os registros de decisão e a documentação daquele domínio.

### Apenas bloqueios verificados
**O modelo:** um revisor automatizado conquista confiança publicando poucos comentários, e cada um deles verificado. **Como aplicar:** exija que todo comentário de revisão automatizado inclua evidência, uma correção sugerida e a regra que viola, e descarte comentários de elogio ou não verificados.

## 🔑 Principais conclusões
- Os agentes herdaram uma base de código feita para humanos. O primeiro agente de migração de testes da equipe precisou de quatro tentativas, acrescentando orientações de "migre este arquivo" até APIs e dados simulados.
- A equipe escreveu 45 ADRs "escritos para pessoas e para agentes", cada um com contexto, decisão, padrão, anti-padrão e notas de uso de IA, ligados à documentação e ao código.
- Um arquivo AGENTS.md funciona como o ponto de entrada que os agentes leem primeiro, apontando para os registros de decisão e para mais de 300 outros documentos, como o robots.txt faz com os rastreadores.
- O pipeline de CI continua determinístico. O agente de revisão publica apenas bloqueios verificados, cada um com evidência, uma correção e a regra do ADR que viola.
- O contexto entre serviços deve ser preciso, portável, de propriedade de cada domínio e mantido atualizado. A equipe escolheu grafos GraphRAG fixados em commits, servidos por uma única ferramenta MCP.
- Os agentes ainda não fazem merge automático das mudanças, e os engenheiros continuam donos do código.
- Khuzam relatou nenhum incidente Sev 1 em 2026 em um serviço central, 32% mais pull requests em relação ao ano anterior e um NPS de desenvolvedores subindo de -3 para +25 ao longo de três trimestres.

## 🏛️ Para o Kenji
*Sua interpretação, não a do autor:* a governança de arquitetura se torna prática quando as decisões são escritas de uma forma que revisores e ferramentas possam aplicar, e é isso que o padrão ADR mais AGENTS.md tenta fazer. No seu trabalho de arquitetura, a pergunta que isso levanta é quais decisões do seu time uma ferramenta poderia verificar automaticamente. Uma segunda pergunta é quem é dono do contexto quando uma mudança cruza domínios, à qual o artigo responde com a propriedade por domínio.

## 📚 Plano de leitura
### Do artigo
- O texto do artigo não cita links. Ele diz que os detalhes da sessão estão disponíveis por um link, mas não dá URL, então nenhuma é indicada aqui. O artigo também promete o vídeo da sessão no InfoQ nas próximas semanas.

### Para aprofundar
- Registros de decisão de arquitetura (ADRs): o formato por trás dos 45 registros. Leia para ver o que os campos de contexto, decisão, padrão e anti-padrão devem conter.
- A convenção AGENTS.md para agentes de codificação: o arquivo de entrada que a equipe compara ao robots.txt.
- GraphRAG: a abordagem de recuperação por trás dos grafos de contexto por domínio.
- Model Context Protocol: o padrão que permitiu que uma única ferramenta servisse Claude Code, Cursor, Codex, um bot do Slack e a revisão de CI.
- O Robots Exclusion Protocol (robots.txt): a convenção dos buscadores por trás da analogia de localizabilidade da equipe.

## 🔗 Conceitos
- [ai-agent](/pt-br/concepts/ai-agent.md)
- [adr](/pt-br/concepts/adr.md)
- [graph-rag](/pt-br/concepts/graph-rag.md)
- [model-context-protocol](/pt-br/concepts/model-context-protocol.md)
- [Engenharia de contexto](/pt-br/concepts/context-engineering.md)
