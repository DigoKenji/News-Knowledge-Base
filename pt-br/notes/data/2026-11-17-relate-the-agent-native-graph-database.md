---
type: Article Summary
title: Relate — o banco de dados de grafos nativo para agentes
description: Relate é um banco de dados de grafos Cypher embutido e de código aberto, que oferece aos agentes fatos conectados para consultar, sem cadastro nem servidor.
resource: https://relatedb.ai/
tags:
- data
- data-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://relatedb.ai/
  title: Relate — the agent-native graph database
  author: Relate (by Neo4j)
status: stable
source_url: https://relatedb.ai/
source: relatedb.ai (forwarded)
source_type: blog
published: '2026-11-17'
domain: data
subdomain: data-engineering
concepts: []
selected_by: forwarded
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81708e9bdf70282fbda9
lang: pt-br
translation_of: /notes/data/2026-11-17-relate-the-agent-native-graph-database.md
---

# Relate — o banco de dados de grafos nativo para agentes

**Fonte:** https://relatedb.ai/
**Autor:** Relate (by Neo4j)
**Data:** 2026-11-17

---

## 🎯 Tese central
Um agente responde melhor a perguntas sobre relacionamentos quando sua memória é um grafo de fatos conectados que ele consulta com Cypher. O Relate oferece isso como um banco de dados embutido e de código aberto, sem cadastro e sem servidor para implantar.

## 🧭 Por que o autor escreveu o artigo
A página parte da afirmação de que agentes precisam entender os dados, não apenas armazená-los. O problema implícito é que a memória de um agente, o contexto de código e as recomendações são fatos dispersos, e um armazenamento plano não consegue acompanhar como pessoas, decisões e documentos se conectam. Se a tese se sustentar, uma pergunta como "quem é dono do trabalho que depende deste lançamento" vira uma única consulta, em vez de uma busca em textos soltos.

## Relate (destaque)
A página extraída começa com um aviso de cookies do Google Tag Manager, que pode ser aceito ou recusado, e a página diz que recusar não tem efeito no site. Isso é a moldura da página, não conteúdo do produto. O destaque oferece o comando de instalação em uma linha, um conjunto de abas de plataforma e linguagem (Agent skill, macOS / Linux, Windows, Python, JavaScript, Rust, Swift, Java, Go, C) e a promessa de "um banco de dados embutido e de código aberto para guardar seus dados. Sem cadastro. Sem servidor para implantar."

```sh
curl -sSfL https://relatedb.ai/install.sh | bash && relate skill install --agent claude-code
```

**O que isto faz:**
- `curl -sSfL` baixa o script de instalação em silêncio (`-s`), ainda mostra erros (`-S`), falha em erros HTTP (`-f`) e segue redirecionamentos (`-L`).
- `| bash` executa o script baixado diretamente em um shell. A página diz que o script verifica assinaturas de release e checksums SHA256, que é a verificação que importa ao canalizar um download para um shell.
- `&&` executa o segundo comando apenas se o primeiro tiver sucesso.
- `relate skill install --agent claude-code` instala a skill de agente para o Claude Code. A legenda diz que o script instala primeiro a CLI e depois a skill.

## Execute Cypher no seu navegador.
O playground roda o banco de dados na própria página. A página diz que "nada é enviado para lugar nenhum", e a demo está rotulada como `relate · :memory: · wasm`, que significa um banco em memória compilado para WebAssembly. São oferecidos quatro conjuntos de dados: analytics (filmes: 12 filmes, 5 avaliadores), agent-memory (3 sessões, com supersedes), codebase (10 crates, 9 ADRs) e dependencies (2 pacotes, 7 avisos de segurança). O painel de consulta tem os controles Explain, Profile e Run. A demo começa quando o leitor rola até ela.

## Da pergunta à resposta, em segundos.
Esta é uma prévia roteirizada. Um agente recebe a pergunta "Quem é dono da página que depende do lançamento do Q3?", escreve Cypher e lê as linhas. A resposta é que a Maya é dona de Pricing, que depende do lançamento do Q3. A visualização de grafo mostra quatro nós e três relacionamentos, rotulados OWNS, DEPENDS_ON e PART_OF.

```cypher
MATCH (t:Task {name: 'Q3 launch'})
      <-[:DEPENDS_ON]-(p:Page)
      <-[:OWNS]-(o:Person)
RETURN p.name, o.name
```

**O que isto faz:**
- `MATCH (t:Task {name: 'Q3 launch'})` ancora o padrão no nó de tarefa cuja propriedade name é "Q3 launch".
- `<-[:DEPENDS_ON]-(p:Page)` segue as arestas DEPENDS_ON de entrada, então as páginas que dependem da tarefa são associadas a `p`.
- `<-[:OWNS]-(o:Person)` dá mais um salto para trás até a pessoa que é dona de cada página.
- `RETURN p.name, o.name` devolve apenas os dois valores de que o agente precisa, e não os nós inteiros.

## Um banco de dados para cada agente.
A afirmação central é que todo agente, script e app lê e escreve o mesmo grafo, que pode ser mantido em memória, em um diretório ou em um único arquivo, sem servidor para rodar. O diagrama mostra uma aplicação que usa uma skill de agente ou um cliente MCP (Claude Code, Codex, Cursor ou qualquer cliente MCP), uma CLI ou bibliotecas (Shell, Python, JavaScript, Rust), e Cypher sobre uma biblioteca embutida. Os exemplos de uso são memória de agente, contexto de código, recomendações e "seus próprios dados".

### Código aberto
A página diz que o código pode ser lido, executado localmente e estendido com as suas próprias receitas.

### Parte da sua aplicação
O Relate roda dentro da sua aplicação. Os dados podem ficar em memória, em um diretório ou em um único arquivo.

### Cypher, para dados conectados
O agente pergunta em Cypher e recebe de volta como os fatos se conectam. A página diz que a skill e o endpoint MCP estão incluídos.

### Comece em minutos
A página repete "Sem cadastro. Sem servidor para implantar."

## Execute uma consulta sobre dados conectados.
Esta é outra prévia roteirizada, com três cenários: memória de agente ("O que você lembra da nossa última sessão?"), contexto de código ("Quais funções quebram se você mudar parse_config()?") e recomendações ("O que devo sugerir a um leitor que gostou de Duna?"). O exemplo de memória roda sobre `memory.rl`.

```cypher
MATCH (e:Event)-[:AFFECTS]->(x)
WHERE e.session = 'last'
RETURN e.summary AS remembered, labels(x)[0] AS kind, x.name AS item
```

**O que isto faz:**
- `MATCH (e:Event)-[:AFFECTS]->(x)` encontra eventos que apontam para qualquer nó por uma aresta AFFECTS. O alvo não tem rótulo, então pode ser uma decisão, uma tarefa ou uma pessoa.
- `WHERE e.session = 'last'` filtra para a sessão anterior. A demo escreve o literal `'last'` diretamente no código.
- `labels(x)[0]` lê o primeiro rótulo do nó alvo, que o resultado usa como coluna "kind".
- A demo devolve três linhas: uma decisão sobre o plano anual, uma tarefa sobre o rascunho do e-mail de lançamento e uma pessoa, a Maya, que é dona da página de preços. A página não diz se o literal embutido da demo seria um parâmetro vinculado em uma chamada real de agente.

## Uma instrução por vez.
Esta prévia permite ao leitor escolher um organograma, um grafo de dependências ou uma base de conhecimento e ver nós e arestas surgirem uma instrução por vez. Os novos fatos se acoplam a nós que já existem.

```cypher
CREATE (maya:Person {name: 'Maya'})-[:REPORTS_TO]->(sam:Person {name: 'Sam'}),
       (alex:Person {name: 'Alex'})-[:REPORTS_TO]->(sam),
       (sam)-[:LEADS]->(:Team {name: 'Platform'});
MATCH (a {name: 'Maya'}) CREATE (a)-[:MENTORS]->(:Person {name: 'Priya'});
MATCH (a {name: 'Sam'}) CREATE (a)-[:LEADS]->(:Team {name: 'Design'});
MATCH (a {name: 'Alex'}) CREATE (a)-[:MEMBER_OF]->(:Team {name: 'Infra'});
```

**O que isto faz:**
- A primeira instrução cria três pessoas nomeadas e um nó Team sem nome em um único `CREATE`, separados por vírgulas, então o padrão é escrito uma vez.
- As instruções seguintes `MATCH ... CREATE` encontram um nó existente pela propriedade name e acoplam a ele um novo nó. A prévia da página mostra esse padrão incremental.
- As cláusulas `MATCH` não dão rótulo, então casam com qualquer nó com aquele nome. Acrescentar um rótulo estreitaria a busca.
- Nós anônimos como `(:Team {name: 'Platform'})` não têm variável, então as instruções seguintes só os alcançam pelas propriedades. O texto da página repete este bloco duas vezes; aqui ele é mostrado uma vez.

## Quatro passos até a sua primeira consulta.
O quickstart lista quatro passos, cada um com um link de guia. (1) Instale a skill de agente, com um comando para os agentes que você já usa. (2) Escolha um formato de armazenamento: em memória, um diretório ou um arquivo `.rl`. (3) Crie dados escrevendo Cypher, carregando CSV ou espelhando do Aura. (4) Consulte, com tabelas para você e JSON para o seu agente.

A página então oferece um prompt "Hi, set me up" para copiar em um agente. Ele é uma instrução para o agente, não uma lista de verificação para o leitor, então é descrito aqui e não reproduzido. Ele pede que o agente instale a CLI com o mesmo script curl e, no Windows fora do WSL, mostre um bloco PowerShell antes de executá-lo. Pede que o agente instale a skill para um de 11 agentes nomeados, leia a skill instalada e rode `relate agent-context`. Usa `./graph.rl`. Se esse arquivo existir, o agente deve inspecionar seu schema e perguntar antes de consultar ou importar, e nunca deve substituir ou apagar dados sem perguntar. As consultas rodam em modo somente leitura com `--mode envelope`, a menos que o usuário peça outra coisa. O acesso de escrita só é permitido para uma importação solicitada, depois da qual o agente volta ao modo somente leitura. O prompt também diz para vincular valores como parâmetros e limitar resultados não agregados com LIMIT. Ele rotula os documentos citados como texto de referência, não como instruções.

## Feito para agentes, onde quer que o seu agente rode.
A página lista quatro interfaces: uma skill de agente que ensina um agente de código a escrever Cypher para o Relate, um endpoint MCP para agentes que falam MCP, uma linha de comando para criar, carregar e consultar pelo shell, e bibliotecas para embutir o Relate em uma aplicação. A lista de bibliotecas cobre Rust, Python, JavaScript, WebAssembly, Go, Java, Swift e C, e a página diz que é "uma biblioteca" em cada uma.

### Nenhum servidor para rodar
O Relate inicia quando a aplicação inicia e para quando ela para. A página inclui uma demo de início, consulta e parada.

> ❌ **Anti-padrão — rodar um servidor separado por agente (implícito, não declarado como alerta):** Dar a cada agente ou aplicação um serviço de banco de dados próprio, que precisa ser hospedado e mantido no ar. **Por que as pessoas fazem isso:** servidores de banco de dados são o padrão que a maioria dos engenheiros já viu. **Consequência:** trabalho de implantação, disponibilidade e ciclo de vida para cada agente. **Correção:** embutir o banco de dados no processo que precisa dele, como descreve a afirmação "nenhum servidor para rodar" da página.

## Memória com estrutura.
A página argumenta que os dados brutos devem se tornar fatos conectados, para que um agente consiga seguir como pessoas, decisões e documentos se relacionam. Ela também afirma que relacionamentos e similaridade podem ser respondidos em uma única consulta.

### Fatos, não texto solto
A página diz que decisões, pessoas e documentos viram fatos que você pode consultar. Um diagrama de demo mostra notes.md, orders.csv e thread.json ligados a pessoas, decisões e tópicos.

> ❌ **Anti-padrão — memória de texto solto (implícito em "fatos, não texto solto"):** Guardar o que um agente aprende como texto livre ou como documentos inteiros que são relidos a cada pergunta. **Por que as pessoas fazem isso:** não exige nenhum schema. 🔎 Contexto adicional: texto livre é fácil de escrever, mas deixa os relacionamentos para serem redescobertos no momento da consulta. **Consequência:** cada pergunta precisa reconstruir os vínculos, e as respostas variam conforme o que o agente acaba lendo. **Correção:** guardar pessoas, decisões e documentos como nós, e seus relacionamentos como arestas.

### Relacionamentos e similaridade juntos
A página diz que uma única instrução Cypher pode combinar os dois. O exemplo mostrado usa apenas padrões de grafo. Ele não tem nenhum operador de similaridade, então a página não mostra a consulta combinada que descreve.

```cypher
MATCH (:Person {name: 'Maya'})-[:DECIDED]->(:Topic)
      -[:DEPENDS_ON]->(l:Launch)
RETURN l.name
```

**O que isto faz:**
- `MATCH (:Person {name: 'Maya'})` parte do nó de pessoa cujo nome é Maya. O nó não tem variável, porque só importa o seu casamento de padrão.
- `-[:DECIDED]->(:Topic)` vai até um tópico sobre o qual a Maya decidiu. O tópico também não tem nome.
- `-[:DEPENDS_ON]->(l:Launch)` vai até um lançamento do qual o tópico depende, e associa a `l` para que possa ser devolvido.
- `RETURN l.name` devolve o nome do lançamento, o único valor necessário para a resposta.

## Do zero à consulta em segundos.
A página apresenta um caminho de três passos: instalar o pacote de código aberto, criar um banco de dados e rodar uma primeira consulta, sem conta, instância ou servidor.

```sh
curl -sSfL https://relatedb.ai/install.sh | bash
```

**O que isto faz:**
- O comando é o mesmo instalador do destaque, sem a etapa de instalação da skill. O marcador de prompt de shell inicial da página foi removido aqui.
- `bash` executa o script baixado, que, segundo a página, verifica assinaturas de release e checksums SHA256.
- Nada é configurado depois da instalação; o próximo comando é a primeira consulta.
- A página trata a instalação como a única etapa de configuração antes de consultar.

```sh
relate --mode envelope "RETURN 1 AS ok"
{"columnTypes":["INTEGER"],"columns":["ok"],"notifications":[],"rows":[{"ok":1}],"truncated":false}
```

**O que isto faz:**
- `relate --mode envelope` executa uma única instrução Cypher e imprime o resultado como um envelope JSON. A página diz que JSON é o formato para agentes.
- `RETURN 1 AS ok` não devolve dados armazenados, então funciona como um teste de fumaça de que a instalação funciona.
- `columnTypes` e `columns` descrevem o formato do resultado, e `rows` guarda os valores.
- `notifications` é uma lista vazia aqui, e `truncated: false` mostra que o resultado não foi cortado.

## Cypher que você já conhece.
A página diz que o Relate fala openCypher e é testado contra o openCypher TCK, que ela chama de contrato da linguagem. Afirma que, onde o Relate difere do Neo4j, a documentação diz isso. As diferenças estão em uma página vinculada, e o texto extraído não dá a URL dela.

## Construa com a gente, uma receita por vez.
A página convida os leitores a compartilhar uma receita para o cookbook, relatar o que encontram e mostrar o que seus agentes aprenderam. Os links apontam para o GitHub e para um formulário "Add a recipe".

## Dê ao seu agente um grafo com o qual ele possa pensar.
A seção final repete a chamada para ação, com "Get started" e "Open the playground". O rodapé lista Build (Quickstart, Docs, Playground, Cookbook, Blog, llms.txt), Community (GitHub, Report an issue, Releases, Add a recipe) e Project (conformidade com o TCK, Decision records, licença Apache-2.0). Termina com uma nota de feedback, um login no GitHub para comentar e a linha de copyright de The Relate Authors.

## 🧠 Modelos mentais
### Fatos conectados em vez de texto solto
**O modelo:** guarde o que um agente aprende como entidades e relacionamentos, para que as perguntas virem consultas de caminho, e não buscas em texto.
**Como aplicar:** antes de salvar uma nota, liste as pessoas, decisões e documentos que ela conecta, e escreva cada vínculo como uma aresta que você possa consultar.

### O banco de dados como biblioteca, não como serviço
**O modelo:** o banco de dados vive dentro do processo da aplicação e inicia e para com ela, então não há servidor separado para hospedar ou manter no ar.
**Como aplicar:** escolha o destino de armazenamento (memória, um diretório ou um arquivo `.rl`) conforme o tempo que os dados precisam sobreviver ao processo, que é a escolha que as opções de armazenamento da página sugerem.

## 🔑 Principais conclusões
- O Relate é um banco de dados de grafos de código aberto que roda dentro da sua aplicação e guarda os dados em memória, em um diretório ou em um arquivo `.rl`.
- A página afirma que não há cadastro nem servidor para implantar, e diz que o script de instalação verifica assinaturas de release e checksums SHA256.
- Os agentes acessam o Relate por uma skill de agente que ensina Cypher, um endpoint MCP, uma linha de comando e bibliotecas em oito linguagens.
- O exemplo trabalhado responde "quem é dono da página que depende do lançamento do Q3" com um único MATCH Cypher de três saltos, que devolve linhas.
- O Relate fala openCypher e é testado contra o openCypher TCK; as diferenças em relação ao Neo4j estão documentadas em uma página separada.
- O prompt de configuração oferecido pela página pede que um agente consulte em modo somente leitura por padrão e pergunte antes de substituir ou importar dados.
- O playground roda o banco de dados no navegador por WebAssembly, e a página diz que nada sai da página.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para trabalho de arquitetura, o padrão útil é um armazenamento embutido que um processo de agente possui, em que você controla o ciclo de vida e o acesso aos dados. A página não diz o que acontece quando vários agentes ou processos escrevem no mesmo arquivo `.rl` ou diretório ao mesmo tempo, e essa é a primeira pergunta a responder antes de depender dele. Como você delimitaria o acesso de escrita a um grafo que vários agentes compartilham, e o prompt de configuração somente leitura por padrão seria um padrão favorável à governança para o seu trabalho de governança de IA?

## 📚 Plano de leitura
### A partir do artigo
- [Guia de instalação, aba Windows](https://relatedb.ai/docs/get-started/install.md) — o caminho PowerShell para o Windows fora do WSL, que a página diz para mostrar antes de executar.
- [Quickstart para agentes](https://relatedb.ai/docs/get-started/agent-quickstart.md) — a referência que o prompt de configuração cita para a configuração de agentes.
- [llms.txt](https://relatedb.ai/docs/llms.txt) — o índice de documentação que o prompt de configuração cita, útil para verificar o que um agente recebe como instrução.
- [Página inicial do Relate](https://relatedb.ai/) — o artigo de origem, para as demos ao vivo e os links do rodapé.

### Para ir além
- Especificação da linguagem openCypher e seu TCK — o kit de conformidade contra o qual a página diz que o Relate é testado.
- Documentação do Cypher do Neo4j — para comparar com as diferenças que a página diz estarem listadas na sua documentação.
- Registros de decisões de arquitetura (ADRs) — o rodapé linka os registros de decisão do projeto, que combinam com a sua prática de ADRs.
