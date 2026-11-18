---
type: Article Summary
title: Executando o app Python completo do Datasette, do lado do servidor, no navegador com service workers e Pyodide
description: O Datasette Lite agora roda o app completo do Datasette, do lado do servidor, no navegador, interceptando requisições com service workers.
resource: https://simonwillison.net/2026/Nov/17/datasette-lite/
tags:
- software-engineering
- web-platform
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/17/datasette-lite/
  title: Running the full server-side Datasette Python app in the browser with service workers and Pyodide
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/17/datasette-lite/
source: Simon Willison
source_type: blog
published: '2026-11-17'
domain: software-engineering
subdomain: web-platform
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8107ab88c9fd556d93c7
lang: pt-br
translation_of: /notes/software-engineering/2026-11-17-running-the-full-server-side-datasette-python-app-in-the-bro.md
---

## 🎯 Tese central
O Datasette Lite agora roda a aplicação Python completa do Datasette, do lado do servidor, no navegador, usando service workers para interceptar requisições. Assim, ele se comporta quase exatamente como um servidor Datasette real.

## 🧭 Por que o autor escreveu
O primeiro Datasette Lite (2022) simulava requisições HTTP e colava os resultados na página, o que quebrava qualquer recurso que dependesse de JavaScript. Se o novo desenho for fiel o bastante, um único link pode reproduzir uma demo, um plugin ou um bug sem que ninguém precise rodar um servidor, e quem lê vê o mesmo comportamento que uma implantação real mostraria.

## Introdução: o que a reescrita consegue fazer
O Datasette Lite é o port feito pelo autor do Datasette, a aplicação Python para navegar em dados, para rodar em um navegador usando WebAssembly e Pyodide. A reescrita é a primeira versão capaz de rodar quase qualquer recurso do Datasette no navegador. Usando URLs cuidadosamente montadas, você pode:

- Carregar o Datasette do lado do cliente para navegar em qualquer banco SQLite, arquivo JSON, arquivo CSV ou script SQL
- Selecionar uma versão lançada do Datasette para carregar a partir do PyPI
- Carregar um commit, tag ou nome de branch específico do repositório do Datasette no GitHub
- Escolher entre mais de 100 versões do SQLite para usar com o Datasette
- Instalar plugins adicionais do Datasette a partir do PyPI ou de URLs de arquivos wheel, incluindo ferramentas de visualização em JavaScript

O autor observa que isso o torna útil para linkar demos de funcionalidades em andamento ou reproduções de bugs, sem precisar subir um servidor web completo.

### Datasette Lite
O Datasette Lite é a versão do Datasette hospedada no navegador, disponível em lite.datasette.io. Trata-se de uma aplicação completa, não de um visualizador somente leitura: as mesmas rotas, plugins e recursos de JavaScript funcionam como funcionariam em um servidor. O autor o lançou em 2022 quase como uma piada e depois percebeu que ele era útil na prática.

### Pyodide e WebAssembly
O Pyodide é o projeto que faz o Python rodar no navegador, e o WebAssembly é o formato binário que os navegadores executam com velocidade quase nativa. Juntos, eles permitem que o código Python do Datasette, e o mecanismo SQLite por baixo dele, rodem na máquina do visitante, sem backend.

> 🔎 Contexto adicionado: o Pyodide é o CPython compilado para WebAssembly. O WebAssembly é um alvo de compilação que os navegadores executam, então código escrito em outras linguagens pode ser compilado e executado dentro de uma página.

## Anatomia de uma URL compartilhável
O autor analisa um exemplo de link para mostrar o que cada parte faz. A URL completa é:

```
https://lite.datasette.io/
  ?install=datasette-maplibre
  &url=https://.../demo.db
  &metadata=https://.../metadata.yml
  #/demo/capitals_and_countries%23map=0/4.2/180&color=continent
```

**O que isso faz:**
- O host é o Datasette Lite. Os parâmetros de consulta `?` são lidos antes de a aplicação iniciar.
- `install=datasette-maplibre` instala um plugin a partir do PyPI. Você pode repetir `&install=...` para quantos plugins precisar.
- `url=` carrega um banco SQLite de um endereço remoto. O arquivo precisa enviar cabeçalhos CORS abertos, e é por isso que o autor hospeda esses arquivos em Gists do GitHub.
- `metadata=` carrega um arquivo de metadados que adiciona descrições em HTML às tabelas e à página inicial.
- O fragmento após `#` é uma localização dentro do Datasette. `/demo/capitals_and_countries` abre uma visão SQL, e o segundo fragmento, depois do `%23` codificado (que é `#`), diz ao plugin de mapa para dar zoom em `map=0/4.2/180` e para colorir as formas pela coluna `continent`.

A página de exemplo mostra o plugin datasette-maplibre rodando um banco de formas de países em GeoJSON, com um mapa combinado e mapas por fronteira dentro de uma tabela. O autor diz que a URL "faz muito trabalho", e ele já escreveu sobre a técnica antes.

### Software endereçável por URL
A ideia do autor é que a URL é a descrição completa de um ambiente em execução: a versão do Datasette, os plugins, os dados, os metadados e o estado da visualização estão todos codificados no link. Quem abre o link obtém a mesma configuração, o que elimina a necessidade de um guia de instalação. O autor descreveu esse padrão em um post anterior sobre ambientes Python com Pyodide endereçáveis por URL. O conjunto completo de argumentos suportados está no README do Datasette Lite.

### Cabeçalhos CORS
CORS (Cross-Origin Resource Sharing) é a regra do navegador que impede uma página de um site de ler dados obtidos de outro site, a menos que o servidor permita. O parâmetro `url=` só funciona quando o host dos dados envia os cabeçalhos corretos, e é por isso que o autor usa Gists do GitHub.

> 🔎 Contexto adicionado: quem impõe o CORS é o navegador, e não o servidor. Um arquivo pode ser baixado diretamente com uma ferramenta e ainda assim ser bloqueado quando uma página web tenta lê-lo.

## Usando a ferramenta para reproduzir bugs
O autor já usa o Datasette Lite durante o desenvolvimento do Datasette. Em uma issue do GitHub sobre um bug de paginação, ele publicou dois links. O primeiro usa `ref=1.0a42` e mostra o comportamento quebrado: clicar em "próxima" deixa a segunda página vazia. O segundo usa `ref=1.0a43` e mostra a mesma página com a correção aplicada. Ambos carregam o mesmo script SQL de um Gist.

### Links de reprodução
Um link de reprodução fixa três coisas de uma vez: a versão do Datasette (por meio de `ref=`), os dados (por meio de `sql=` ou `url=`) e a localização da página (por meio do fragmento). Como quem lê só precisa clicar, um revisor pode comparar lado a lado as versões quebrada e corrigida, sem instalar nenhuma delas. Esse é o valor prático que o autor aponta para funcionalidades em andamento e relatos de bugs.

## Como funciona: service workers
O autor explica por que a reescrita foi necessária, descrevendo primeiro a versão original e depois o desenho atual.

### A abordagem original de 2022
A primeira versão rodava o Datasette em um web worker do Pyodide. Ela simulava requisições HTTP por essa pilha e depois inseria a resposta HTML na página principal com `innerHTML`. Isso funcionava para páginas básicas, mas qualquer recurso que usasse JavaScript quebrava, porque tags `<script>` inseridas com `innerHTML` não são executadas. O contorno com web worker também não conseguia carregar arquivos extras de JavaScript ou CSS.

> ❌ **Antipadrão — colar respostas na página com innerHTML:** a aplicação monta uma resposta HTML em um worker e a injeta na página em execução. **Por que as pessoas fazem isso:** é a forma mais simples de mostrar a saída de um runtime que não é de navegador. **Consequência:** scripts e referências de folhas de estilo na resposta não se comportam como se comportariam em um servidor, então os recursos interativos falham silenciosamente. **Correção:** interceptar as próprias requisições do navegador com um service worker, para que a página carregue seus assets pelo caminho normal de requisições.

### Service workers
Service workers são scripts do navegador que podem interceptar requisições HTTP e respondê-las a partir de código do lado do cliente. O Datasette Lite registra um service worker para todo o domínio lite.datasette.io. A página pai cria um iframe, e o iframe requisita páginas e assets do Datasette a partir de URLs deste formato:

```
/app/aa94e6851595/                              # the homepage (HTML)
/app/aa94e6851595/data/places                   # a table page (HTML)
/app/aa94e6851595/-/static/navigation-search.js # (JavaScript)
/app/aa94e6851595/-/static/app.css              # (CSS)
```

**O que isso faz:**
- São caminhos padrão do Datasette, com um prefixo extra `/app/<session>/` na frente.
- As requisições de HTML, JavaScript e CSS passam pelo service worker, então o tratamento de arquivos estáticos do próprio Datasette roda sem alterações.
- O prefixo é a chave que permite que um único service worker atenda muitas instâncias independentes do Datasette, como explicado abaixo.
- Como os caminhos parecem URLs comuns do Datasette, a aplicação não precisa de código especial para o navegador.

> 🔎 Contexto adicionado: um service worker permanece registrado no navegador e trata as requisições do seu escopo, podendo responder a uma requisição com uma resposta que ele mesmo constrói. É isso que permite à página carregar seu JavaScript e CSS a partir do cliente.

### Prefixo de sessão
O prefixo `/app/<session>/` existe por causa das múltiplas abas. O service worker é compartilhado por todas as abas do domínio. Se ele roteasse cada requisição para uma única instância do Datasette, todas as abas compartilhariam aquela instância, quaisquer que fossem os plugins ou configurações pedidos pela URL de cada uma. Em vez disso, a página pai gera um ID de sessão aleatório e o usa como prefixo em cada requisição oculta. O service worker lê o ID de sessão e devolve a requisição à página pai, que a encaminha para uma instância dedicada do Datasette para aquela sessão.

> ❌ **Antipadrão — um runtime compartilhado atrás de um roteador compartilhado:** uma única instância do Datasette atende todas as requisições do domínio. **Por que as pessoas fazem isso:** é mais simples e usa menos memória. **Consequência:** os plugins, dados e configurações de cada aba sobrescrevem os das outras, então um link de demo pode mostrar a configuração de outra demo. **Correção:** colocar um identificador por sessão no caminho da URL e rotear cada requisição para o seu próprio runtime.

### Web workers continuam necessários
O autor observa que o Pyodide não pode rodar dentro de um service worker por muito tempo, porque não consegue manter o estado por mais de cerca de 30 segundos, dependendo do navegador. Por isso, a instância do Datasette de cada sessão ainda roda em um web worker, e o service worker cuida apenas do roteamento.

> 🔎 Contexto adicionado: um web worker é uma thread de JavaScript em segundo plano para trabalhos longos, separada da página. O navegador pode parar um service worker quando ele está ocioso, enquanto um web worker vive enquanto a página dele viver.

### Diagramas e histórico
O autor pediu ao Claude Fable 5.2 que gerasse vários diagramas SVG para explicar o desenho, e um deles agora está no README do Datasette Lite. O sistema começou em maio de 2026 como um experimento de pesquisa em IA construído com o Claude Opus 4.8. O autor disse que não se sentiu confiante o bastante para lançá-lo até novembro de 2026, quando usou o Claude Fable 5.2 para concluí-lo.

## Bônus: escolhendo a versão do SQLite
O autor acrescentou a possibilidade de escolher uma versão do SQLite na noite anterior, como efeito colateral de investigar um possível bug no SQLite. O Datasette foi projetado para funcionar com qualquer versão lançada do SQLite desde a 3.25, que saiu em setembro de 2018. Desde então, houve 87 lançamentos. Poder rodar o Datasette com uma versão específica do SQLite ajuda a demonstrar novos recursos do SQLite e a depurar problemas estranhos. Em um Mac, o autor já havia precisado de vários truques para trocar de versão, o que ele descreve como "não é um processo trivial".

### Wheels WebAssembly do pysqlite3
O autor construiu os wheels em um repositório novo, `datasette/pysqlite3-wasm-wheels`. Ele contém wheels Python compilados para WebAssembly do pysqlite3, cobrindo 163 versões do SQLite, com um arquivo por versão. A versão mais recente é a SQLite 3.54.1.

```
pysqlite3-0.6.2-3.54.1-cp314-cp314-pyemscripten_2026_0_wasm32.whl
```

**O que isso faz:**
- `pysqlite3-0.6.2` é a versão do pacote Python pysqlite3.
- `3.54.1` é a versão do SQLite empacotada dentro do wheel.
- `cp314` significa que o wheel tem como alvo o CPython 3.14.
- `pyemscripten_2026_0_wasm32` é a tag de plataforma, que indica uma build WebAssembly para o ambiente Pyodide.
- O nome do arquivo codifica qual versão do SQLite o wheel contém, então ele pode ser localizado a partir de um número de versão.

O Datasette Lite aceita um parâmetro especial `?sqlite=`. Ele recebe um número de versão, como `3.54.1`, encontra o wheel correspondente, o baixa e instala no ambiente Pyodide, e configura o Datasette para usá-lo. O autor também construiu uma ferramenta de comparação que executa uma mesma consulta SQL em versões selecionadas do SQLite. O exemplo dele, `select json_object('a', (select json_array(1)))`, produz resultados diferentes nas versões 3.54.0 e 3.54.1. O texto não explica a diferença, que aparece apenas na captura de tela.

## Além do Datasette: o padrão geral
O autor sustenta que nada no Datasette Lite é exclusivo do Datasette. Quase qualquer aplicação Python poderia rodar desse jeito, como uma forma barata de oferecer ambientes de pré-visualização, demonstrar funcionalidades e recriar bugs. Extensões em C e Rust costumavam bloquear isso, mas agentes de programação agora conseguem compilá-las para WebAssembly compatível com o Pyodide. Essas builds podem ser publicadas no PyPI sob a PEP 783, que, segundo o autor, está disponível desde abril de 2026.

### Extensões nativas em WebAssembly
Pacotes Python com código C ou Rust precisam ser compilados para o alvo WebAssembly do navegador antes que o Pyodide consiga carregá-los. O autor diz que um agente agora consegue fazer esse trabalho, e a PEP 783 oferece um caminho para publicar os resultados no PyPI. O autor linka o pull request no PyPI que dá suporte a isso.

### Bancos de dados como limite
O SQLite funciona sem nenhum esforço extra, porque é uma biblioteca embutida e os wheels acima já o incluem. Rodar PostgreSQL ou MySQL dentro de WebAssembly é possível, mas o autor descreve isso como um caminho pouco trilhado. O autor espera ver ambientes de pré-visualização em WebAssembly se tornarem um padrão comum em todo o ecossistema Python.

## 🧠 Modelos mentais
### Roteador compartilhado, runtime por sessão
**O modelo:** um único service worker atende um domínio inteiro, então um ID de sessão no caminho da URL decide qual runtime isolado trata cada requisição.
**Como aplicar:** quando uma mesma porta de entrada compartilhada precisa manter muitos usuários ou abas separados, coloque a identidade da sessão no caminho e dê a cada sessão o seu próprio runtime com estado.

### A URL é a configuração completa
**O modelo:** um link pode codificar a versão do software, os plugins, a fonte de dados, os metadados e o estado da visualização, então o link sozinho reproduz um ambiente.
**Como aplicar:** ao compartilhar uma demo ou um relato de bug, coloque no link cada variável que altera o comportamento, em vez de descrevê-la em prosa.

### Fixe cada versão para reproduzir
**O modelo:** o comportamento depende tanto da versão da aplicação quanto da versão do runtime, como a versão do SQLite, então uma reprodução precisa nomear ambas.
**Como aplicar:** execute a mesma consulta nas versões que importam, por exemplo 3.54.0 e 3.54.1, e descarte o ambiente quando o teste terminar.

## 🔑 Principais conclusões
- O Datasette Lite agora usa service workers para interceptar requisições de páginas e assets. Isso substitui a abordagem de 2022, que injetava HTML com innerHTML e quebrava os recursos de JavaScript.
- Cada sessão do navegador recebe um ID aleatório em seu prefixo de URL, então abas com configurações diferentes do Datasette permanecem separadas, mesmo com um único service worker atendendo o domínio inteiro.
- O Pyodide não consegue manter seu estado dentro de um service worker por muito tempo, cerca de 30 segundos dependendo do navegador, por isso o Datasette de cada sessão roda em um web worker.
- Uma única URL pode fixar a versão ou o commit do Datasette, os plugins, a versão do SQLite, os dados, os metadados e a visualização do mapa, o que torna demos e reproduções de bugs compartilháveis.
- O autor demonstrou um bug de paginação com dois links, um na versão 1.0a42 do Datasette mostrando o comportamento quebrado e outro na 1.0a43 mostrando a correção.
- O parâmetro `?sqlite=` funciona porque 163 versões do SQLite são empacotadas como wheels WebAssembly no repositório pysqlite3-wasm-wheels.
- A abordagem se generaliza para a maioria das aplicações Python. A principal lacuna que o autor cita são os bancos de dados de servidor, como PostgreSQL e MySQL.

## 🏛️ Para o Kenji
> Esta é a sua interpretação, não a do autor.

O padrão é um ambiente de pré-visualização descartável: um link inicia uma cópia fiel de um serviço sem nenhuma infraestrutura por trás. Para um arquiteto na trilha Staff, isso poderia substituir algumas implantações de pré-visualização por branch, em serviços cujo estado cabe no SQLite, e dá aos revisores um artefato reproduzível para relatos de bugs. A questão em aberto é como você trataria o estado em um banco de dados de servidor, como o PostgreSQL, que o artigo identifica como a parte difícil.

## 📚 Plano de leitura
### Do artigo
- [Datasette Lite](https://lite.datasette.io/) — a ferramenta em funcionamento, a forma mais rápida de testar os padrões de URL.
- [Ambientes Python com Pyodide endereçáveis por URL](https://simonwillison.net/2025/Feb/13/url-addressable-python/) — o post anterior do autor sobre a técnica de URL.
- [README do Datasette Lite](https://github.com/simonw/datasette-lite#load-a-different-datasette-version) — a lista completa de argumentos de URL suportados.
- [Post de lançamento do Datasette Lite (2022)](https://simonwillison.net/2022/May/4/datasette-lite/) — a versão original e o motivo de sua criação.
- [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) — o mecanismo do navegador do qual a reescrita depende.
- [Guia de CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) — por que arquivos de banco remotos precisam de cabeçalhos abertos.
- [Experimento de pesquisa em IA](https://github.com/simonw/research/tree/main/pyodide-asgi-browser#readme) — a origem do desenho, antes de ele ser levado à produção.
- [pysqlite3-wasm-wheels](https://github.com/datasette/pysqlite3-wasm-wheels) — o repositório com os 163 wheels do SQLite.
- [Pull request no PyPI para a PEP 783](https://github.com/pypi/warehouse/pull/19804) — a mudança que permite publicar extensões compatíveis com Pyodide no PyPI.

### Para ir além
- Documentação do projeto Pyodide — como o CPython é compilado e executado em WebAssembly.
- Fundamentos de WebAssembly — o alvo de compilação por trás do Pyodide e das builds do SQLite.
- Web Workers API — o modelo de thread em segundo plano que hospeda a instância do Datasette de cada sessão.
- pysqlite3 — os bindings Python dos quais os wheels WebAssembly são construídos.
