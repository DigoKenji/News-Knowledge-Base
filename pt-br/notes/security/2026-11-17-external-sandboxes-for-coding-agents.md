---
type: Article Summary
title: Sandboxes externos para agentes de programação
description: Manter os agentes de programação fora de um sandbox, enquanto os comandos deles rodam dentro de um, impõe limites que nenhum julgamento do modelo consegue contornar.
resource: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
tags:
- security
- appsec
- threat-modeling
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
  title: External sandboxes for coding agents
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
source: Simon Willison
source_type: blog
published: '2026-11-17'
domain: security
subdomain: appsec
concepts:
- threat-modeling
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8126954ddc27d6eb42ef
lang: pt-br
translation_of: /notes/security/2026-11-17-external-sandboxes-for-coding-agents.md
---

## 🎯 Tese central
Agentes de programação podem ficar muito mais seguros se o agente for mantido fora de um sandbox e apenas os comandos que ele executa forem enviados a um sandbox separado, que impõe limites de arquivos, rede e credenciais, porque essa fronteira não depende do julgamento de nenhum modelo.

## 🧭 Por que o autor escreveu
O link post de Simon Willison apresenta um trabalho que ele fez com a Prime Radiant sobre como executar agentes de programação com segurança, especialmente modelos locais menores, que erram mais e são mais suscetíveis a prompt injection. Se a tese estiver certa, você pode deixar um agente executar comandos livremente dentro de uma fronteira que você controla, em vez de aprovar cada ação manualmente ou confiar em acesso total e torcer para dar certo.

## Introdução do link blog (Simon Willison)
O post de Simon Willison é uma entrada curta de link blog, datada de 17 de novembro de 2026. Ele diz que vem explorando formas de isolar em sandbox o código que os agentes executam, com foco em tornar mais seguro testar modelos locais sem se preocupar com a possibilidade de um erro ou de um ataque de prompt injection levar esses modelos a tomar ações danosas em uma máquina. Ele trabalhou no projeto com a Prime Radiant, o laboratório de pesquisa em IA por trás do Superpowers e de outros projetos.

A entrada resume o padrão de sandbox externo: o código que o agente executa roda dentro de um sandbox dedicado, enquanto o próprio agente fica fora desse sandbox. Para demonstrar o padrão, a equipe lançou três implementações de servidor de sandbox:

- [external-sandbox-v86](https://github.com/prime-radiant-inc/external-sandbox-v86) roda um servidor Node.js que usa v86 para executar Alpine Linux dentro de WebAssembly.
- [external-sandbox-container](https://github.com/prime-radiant-inc/external-sandbox-container) é um servidor web em Python que executa o sandbox em Alpine Linux dentro de uma instância do Apple Containers.
- [external-sandbox-quicksand](https://github.com/prime-radiant-inc/external-sandbox-quicksand) é outro servidor web em Python, que usa a biblioteca de sandboxing Quicksand da Microsoft, a qual empacota QEMU e Alpine Linux em um pacote Python.

As três oferecem a mesma API em localhost, então você pode configurar o Pi, o Claude Code ou o Codex para enviar chamadas de API para qualquer um dos sandboxes que estiver rodando. Os sandboxes também incluem um proxy opcional de allow-list para HTTP/HTTPS e um mecanismo de substituição de credenciais, que faz chamadas de API autenticadas sem compartilhar um segredo com o agente. Uma interface web permite alterar a configuração do proxy enquanto o sandbox está em execução. Simon encerra dizendo que continua fascinado pelo desafio de executar código não confiável com segurança e que esse padrão é uma forma interessante de testar abordagens diferentes. A entrada termina com uma lista de artigos recentes do blog, que não é expandida aqui.

## Experimente com dois comandos
O artigo completo começa com uma demonstração rápida que precisa de dois comandos e do Node.js 22.19 ou mais recente. Você entra no diretório do projeto e inicia um sandbox com npx. Isso baixa o Alpine Linux e o executa dentro do v86, um emulador x86 compilado para WebAssembly e hospedado no Node.js:

```bash
npx --yes external-sandbox-v86 \
  --preserve-paths --mount-rw "project=$PWD"
```

**O que isso faz:**
- `npx --yes` executa o pacote publicado `external-sandbox-v86` sem pedir confirmação.
- `--mount-rw "project=$PWD"` monta o diretório atual do projeto com leitura e escrita dentro do sandbox, sob o nome `project`.
- `--preserve-paths` mantém válido dentro do sandbox o caminho original do host, para que comandos que mencionem caminhos do host continuem funcionando.

Em um segundo terminal, no mesmo diretório, você inicia um agente de programação Pi. Se você nunca usou o Pi, executa `/login` dentro do agente para dar a ele acesso a um modelo:

```bash
npx --yes @earendil-works/pi-coding-agent \
  -e npm:external-sandbox-pi --no-builtin-tools \
  --sandbox-url http://127.0.0.1:8686
```

**O que isso faz:**
- `-e npm:external-sandbox-pi` carrega a extensão que direciona as ferramentas do Pi para o sandbox.
- `--no-builtin-tools` desativa as ferramentas próprias do Pi, de modo que nada rode no seu computador mesmo se a extensão não carregar.
- `--sandbox-url` aponta o Pi para o servidor v86 na porta 8686.

Executar `!uname -a` dentro do Pi deve mostrar que os comandos rodam no Alpine Linux. A saída de exemplo do artigo começa com `Linux (none) 6.12.114-0-virt`, que é o kernel do sistema convidado Alpine, e não o seu Mac. Claude Code e Codex também funcionam, com mais configuração, descrita abaixo.

## O que isso oferece
O artigo lista cinco propriedades dessa configuração:

- **Seu agente continua no seu Mac.** Pi, Claude Code e Codex rodam no seu computador como de costume. Apenas os comandos que eles executam são enviados ao sandbox.
- **Seu sandbox só enxerga as pastas que você montar.** Você escolhe quais pastas ficam disponíveis para esses comandos e se cada uma será somente leitura ou leitura e escrita.
- **Nenhum acesso à rede, a menos que você permita.** Por padrão, o sandbox não consegue alcançar o mundo externo. Adicionar a opção `--proxy` dá acesso por um proxy HTTP/HTTPS, limitado aos hosts que você permitir.
- **Chaves de API que o agente não consegue ver.** O proxy faz a substituição de credenciais: requisições para hosts específicos recebem as chaves de API, de modo que os segredos ficam fora do sandbox e invisíveis ao agente que faz a chamada.
- **Uma interface web para alterar as regras.** Ela oferece um terminal no navegador, ferramentas para alterar a configuração do proxy e um log das requisições feitas pela rede.
- **Qualquer agente funciona com qualquer sandbox.** Os três sandboxes implementam o mesmo protocolo, então você pode combiná-los com o agente de sua preferência.

## Como diagrama
O artigo inclui, neste ponto, um diagrama da arquitetura. O diagrama é uma imagem e não está presente na extração de texto, então seu conteúdo não é reproduzido aqui. O texto ao redor descreve a mesma divisão: o agente no host e um sandbox que executa os comandos dele.

## Por que usar sandboxes externos?
A motivação do artigo parte da constatação de que agentes de programação podem errar, e que tudo o que eles conseguem ler ou editar pode acabar danificado ou comprometido quando isso acontece. Em um mundo ideal, os agentes rodariam em ambientes nos quais qualquer dano que possam causar fosse estritamente limitado. Na prática, muitas pessoas dão aos agentes acesso total ao computador e à internet e torcem para dar certo.

Uma solução comum é aprovar manualmente cada ação que o agente quer executar. O autor chama isso de um jeito terrível de trabalhar, porque é frustrantemente lento e leva à fadiga de decisão, quando as pessoas aprovam tudo sem entender a pergunta.

> ❌ **Antipadrão — Aprovação manual de cada ação:** exigir que uma pessoa confirme cada comando ou edição de arquivo. **Por que as pessoas fazem isso:** parece a única opção segura quando não se pode confiar no agente. **Consequência:** trabalho lento e fadiga de decisão, então quem revisa para de ler e aprova tudo. **Correção:** restringir, na fronteira, o que o agente pode tocar, para que ações rotineiras não precisem de revisão.

O padrão de sandbox externo permite que agentes de programação executem código e editem arquivos dentro de um sandbox que dá ao usuário controle sobre os arquivos, as variáveis de ambiente e as conexões de rede que o agente pode usar. O agente mantém a flexibilidade de executar comandos e de escrever e rodar código.

Em seguida, o artigo compara as abordagens dos dois grandes fornecedores. O modo automático do Claude Code e a revisão automática do Codex usam verificações adicionais de IA para decidir quando é provavelmente seguro continuar, e o autor diz que elas parecem funcionar bem. O ponto do autor é que essas verificações são recursos de produtos específicos e ainda dependem de um modelo julgar corretamente cada ação. Um sandbox é uma proteção que não depende do julgamento de nenhum modelo. Isso importa mais quando você testa modelos alternativos, incluindo modelos mais baratos ou modelos que rodam localmente, porque esses modelos mais leves erram mais e são alvos mais fáceis de prompt injection deliberada.

🔎 Contexto adicionado: prompt injection é um ataque em que um texto presente em conteúdo que o agente lê, como uma página web ou um arquivo, tenta fazer o agente seguir instruções que o usuário nunca deu. Um modelo que interpreta mal esse texto pode então executar comandos perigosos.

As metas da equipe são formas robustas, confiáveis e fáceis de usar para que agentes de programação trabalhem em um sandbox. Os comandos devem conseguir ler e escrever apenas os arquivos que o usuário escolher e fazer apenas as requisições de rede que o usuário permitir.

Foram consideradas duas formas de colocar um agente em um sandbox. A primeira executa o agente dentro do sandbox, que é a abordagem de sistemas como os Sprites da Fly. A segunda executa o agente fora do sandbox e faz com que ele envie comandos a um sandbox que existe separadamente. A equipe escolheu a segunda. Ela evita comportamentos estranhos em que o agente modifica a si mesmo ou o seu ambiente imediato, e mantém a fronteira do sandbox bem definida e mais fácil de auditar. O nome "sandboxes externos" reflete que o sandbox é externo ao agente, e não ao computador, já que ambos podem rodar na mesma máquina.

## Os três protótipos
As três implementações são projetos separados, mas compatíveis, cada um usando uma estratégia diferente de sandboxing externo:

- **external-sandbox-v86** é um processo Node.js que usa v86 em WebAssembly para executar Alpine Linux. O pacote npm dele inclui uma distribuição Linux completa que roda dentro do sandbox.
- **external-sandbox-container** é um processo Python que encapsula o Apple Containers executando Linux. Ele consegue iniciar um contêiner, executar comandos nele pelo protocolo e encerrar o contêiner ao desligar.
- **external-sandbox-quicksand** é um processo Python que roda uma máquina virtual QEMU completa por meio do Quicksand, da Microsoft. O QEMU e a imagem Alpine vêm como wheels de Python, então não há mais nada para instalar.

Os três sistemas oferecem APIs web em localhost idênticas, mas cada agente se conecta a eles de um jeito diferente:

- O Claude Code envia para o sandbox apenas os comandos da sua ferramenta Bash. As ferramentas nativas Read e Edit continuam rodando no host, fora do sandbox.
- O Codex envia suas operações de processo e de sistema de arquivos, incluindo o apply_patch nativo.
- O Pi envia suas ferramentas bash, read, write, edit, grep, find e ls. grep e find precisam do ripgrep dentro do sistema convidado, que todas as três imagens padrão incluem.

A equipe também produziu uma especificação e uma ferramenta de testes de conformidade para a API, para que futuros servidores de sandbox compatíveis possam ser construídos com outras tecnologias.

## Como usar com outros sandboxes e agentes
O servidor Node.js do Alpine com v86 roda na porta 8686 por padrão. O servidor Python com Apple Containers usa a porta 8687, e o servidor Python com Quicksand usa a porta 8688, então os três podem rodar lado a lado. O servidor v86 é publicado no npm e roda com npx. Os dois servidores Python são publicados no PyPI como external-sandbox-container e external-sandbox-quicksand, e rodam com uvx.

Inicie o servidor v86 assim:

```bash
# v86 Alpine Node.js (Node.js 22+)
npx --yes external-sandbox-v86 \
  --preserve-paths --mount-rw "project=$PWD"
```

**O que isso faz:**
- O comentário registra o requisito de Node.js 22 ou mais recente.
- O comando é o mesmo da demonstração rápida, e ele serve a porta padrão 8686.
- O diretório do projeto é montado com leitura e escrita, e `--preserve-paths` mantém válido o caminho do host.

Para o Apple Containers, o artigo traz o seguinte:

```bash
# Python Apple Containers (Mac com Apple silicon e a CLI container da Apple)
container system start
uvx external-sandbox-container \
  --preserve-paths --mount-rw "project=$PWD"
```

**O que isso faz:**
- `container system start` inicia o runtime de contêineres da Apple, do qual o servidor precisa.
- `uvx` executa o pacote Python publicado sem instalá-lo de forma permanente.
- O servidor do Apple Containers exige um Mac com Apple silicon e a CLI container da Apple.

Para o Quicksand, o artigo traz o seguinte. A primeira execução demora mais, enquanto ele prepara e guarda em cache a imagem do Alpine:

```bash
# Python Quicksand (Mac com Apple silicon)
uvx external-sandbox-quicksand \
  --preserve-paths --mount-rw "project=$PWD"
```

**O que isso faz:**
- `uvx` obtém e executa o servidor Quicksand, que empacota QEMU e Alpine como wheels de Python.
- As flags de montagem e `--preserve-paths` se comportam como nos outros servidores.
- A primeira execução é mais lenta porque a imagem do Alpine é preparada e guardada em cache.

Cada servidor monta o diretório atual com leitura e escrita em `/mnt/project` e, graças a `--preserve-paths`, também no caminho original. Use `--mount-ro name=/algum/caminho` para pastas somente leitura. Abrir a porta do servidor em um navegador dá acesso ao terminal e à interface de regras de rede.

## Claude Code
As ferramentas de leitura e edição do Claude Code não passam pelo sandbox. As regras de montagem limitam o que os comandos Bash podem tocar, mas o Claude ainda consegue ler e editar arquivos no seu Mac diretamente. Essa é uma limitação da configuração, que o artigo declara com clareza.

Para rodar o Claude Code contra um sandbox, obtenha o script de prefixo de shell compartilhado no repositório external-sandbox:

```bash
git clone https://github.com/prime-radiant-inc/external-sandbox.git
PREFIX_DIR="$(cd external-sandbox/claude-code && pwd)"
```

**O que isso faz:**
- A primeira linha clona o repositório que contém o protocolo, a ferramenta de conformidade e o script de prefixo do Claude Code.
- A segunda linha guarda em `PREFIX_DIR` o caminho absoluto da pasta `claude-code`, para que os comandos seguintes não dependam do diretório de trabalho.

Depois, execute o Claude Code a partir do diretório do projeto em que o servidor foi iniciado. O exemplo usa o v86 na porta 8686:

```bash
mkdir -p .external-sandbox-claude-tmp
CLAUDE_CODE_SHELL=/bin/bash \
CLAUDE_CODE_TMPDIR="$PWD/.external-sandbox-claude-tmp" \
CLAUDE_CODE_SHELL_PREFIX="$PREFIX_DIR/claude-shell-prefix.sh" \
EXTERNAL_SANDBOX_API_URL=http://127.0.0.1:8686 \
claude
```

**O que isso faz:**
- `CLAUDE_CODE_SHELL=/bin/bash` seleciona o shell que o Claude Code usa na sua ferramenta Bash.
- `CLAUDE_CODE_TMPDIR` aponta o diretório temporário do Claude Code para uma pasta dentro da montagem com escrita, porque ele o usa para a sua própria contabilidade interna.
- `CLAUDE_CODE_SHELL_PREFIX` faz com que cada comando Bash passe pelo script de prefixo, que o encaminha ao sandbox.
- `EXTERNAL_SANDBOX_API_URL` informa ao script de prefixo qual sandbox contatar. Use a porta 8687 para o Apple Containers ou a 8688 para o Quicksand.

O diretório `.external-sandbox-claude-tmp` precisa estar dentro da montagem com escrita. Caso contrário, o Claude Code não consegue fazer a contabilidade interna dele.

## Codex
Para rodar o Codex contra um sandbox, crie um diretório home dedicado do Codex com um arquivo `environments.toml` que aponte para o sandbox. O exemplo usa o v86 na porta 8686:

```bash
mkdir -p ~/.codex-external-sandbox
cat > ~/.codex-external-sandbox/environments.toml <<'EOF'
default = "external-sandbox"
include_local = false
[[environments]]
id = "external-sandbox"
url = "ws://127.0.0.1:8686/codex"
connect_timeout_sec = 5
initialize_timeout_sec = 10
EOF
```

**O que isso faz:**
- `default = "external-sandbox"` faz do sandbox o ambiente que o Codex usa por padrão.
- `include_local = false` impede o Codex de oferecer também o seu ambiente local.
- A entrada `[[environments]]` dá ao sandbox um id e uma URL WebSocket no endpoint `/codex` do servidor.
- Os dois timeouts limitam quanto tempo o Codex espera para conectar (5 segundos) e para inicializar (10 segundos).

Faça login uma vez com esse home e depois inicie o Codex com ele:

```bash
CODEX_HOME=~/.codex-external-sandbox codex login
CODEX_HOME=~/.codex-external-sandbox codex --sandbox danger-full-access \
  -c features.shell_snapshot=false \
  -c features.unified_exec_tty=false \
  --cd "$PWD"
```

**O que isso faz:**
- `CODEX_HOME` mantém essa configuração separada de qualquer configuração normal do Codex.
- `--sandbox danger-full-access` diz ao Codex para não adicionar o próprio sandbox dentro do sandbox externo.
- As duas flags `-c` desativam duas funcionalidades do Codex, o snapshot do shell e o unified exec baseado em TTY, para que o Codex funcione com o protocolo do sandbox externo.
- `--cd "$PWD"` inicia o Codex no diretório do projeto.

O artigo informa que seus testes automatizados passam com o Codex CLI 0.164.0, usando `codex exec` e um modelo simulado. O protocolo exec-server do Codex é experimental, então outras versões podem precisar de verificação.

## Pi
Para rodar o Pi contra um sandbox, carregue a extensão external-sandbox-pi com as ferramentas integradas do Pi desativadas. Execute isso a partir do diretório do projeto em que o servidor foi iniciado. O exemplo usa o Quicksand na porta 8688:

```bash
pi -e npm:external-sandbox-pi --no-builtin-tools \
  --sandbox-url http://127.0.0.1:8688
```

**O que isso faz:**
- `-e npm:external-sandbox-pi` baixa a extensão apenas para esta execução.
- `--no-builtin-tools` significa que nada roda no seu computador caso a extensão não carregue.
- `--sandbox-url` aponta o Pi para o servidor Quicksand na porta 8688.

A extensão exige o Pi 1.5 ou mais recente. A barra de status do Pi mostra `Sandbox: ...` quando ela se conecta.

## Controlando o acesso à rede
Por padrão, nenhuma das três implementações tem acesso externo à rede. Isso protege contra bugs ou ataques de prompt injection que poderiam vazar dados privados para uma fonte externa. O artigo se refere ao lethal trifecta, a combinação de riscos descrita no post anterior de Simon Willison sobre o assunto.

🔎 Contexto adicionado: o lethal trifecta, como Simon Willison o descreveu, é um agente que pode acessar dados privados, está exposto a conteúdo não confiável e pode se comunicar com o exterior. O sandbox elimina o último desses três por padrão.

Cada sandbox pode ser configurado para permitir acesso HTTP e HTTPS por meio de um proxy. O proxy controla quais hosts podem ser alcançados e quais verbos HTTP podem ser usados com eles. Isso permite acesso somente leitura ou total a hosts específicos. Quando o proxy está ativado, as variáveis `HTTP_PROXY` e `HTTPS_PROXY` são definidas dentro do sandbox, para que clientes como o wget saibam como chegar à rede por meio dele. O sandbox também disponibiliza o certificado CA do proxy, apontando variáveis como `SSL_CERT_FILE`, `REQUESTS_CA_BUNDLE` e `NODE_EXTRA_CA_CERTS` para ele. Assim, os clientes conseguem acessar sites seguros enquanto o proxy inspeciona e filtra o tráfego deles. Código que ignora essas configurações e tenta se conectar diretamente continua bloqueado.

A opção `--proxy-config` aponta para um arquivo que lista os hosts e métodos habilitados para a sessão. Segue um exemplo de arquivo de configuração:

```json
{
  "allow": [
    {
      "origin": "https://pypi.org",
      "methods": ["GET", "HEAD"]
    },
    {
      "origin": "https://files.pythonhosted.org",
      "methods": ["GET"]
    },
    {
      "origin": "https://api.github.com",
      "methods": ["GET"],
      "credential": {
        "header": "authorization",
        "placeholder": "Bearer placeholder-github-token",
        "secretEnv": "GITHUB_TOKEN",
        "prefix": "Bearer "
      }
    }
  ],
  "guestEnv": {
    "GITHUB_TOKEN": "placeholder-github-token"
  }
}
```

**O que isso faz:**
- Cada entrada `allow` nomeia uma `origin` e os métodos HTTP `methods` permitidos para ela. PyPI e o host de arquivos Python permitem apenas `GET` (e `HEAD` no PyPI), ou seja, acesso somente leitura.
- A entrada de `api.github.com` é a única com um bloco `credential`, que habilita a substituição de credenciais para esse host.
- `secretEnv` nomeia a variável no host que guarda o segredo real, aqui `GITHUB_TOKEN`. `prefix` e `placeholder` definem o valor do cabeçalho que o sandbox envia.
- O bloco `guestEnv` dá ao código dentro do sandbox uma variável `GITHUB_TOKEN` que contém apenas o placeholder.

## Substituição de credenciais para APIs HTTP
A substituição de credenciais permite que o código no sandbox faça chamadas autenticadas a APIs web sem que a chave fique exposta ao agente nem disponível dentro do sandbox.

O exemplo acima configura isso para a API do GitHub. As requisições passam pelo proxy para `https://api.github.com/` com um cabeçalho de autorização `Bearer placeholder-github-token`. O proxy substitui esse cabeçalho pelo valor da variável de ambiente `GITHUB_TOKEN`, que existe no ambiente do servidor de sandbox no host. O bloco `guestEnv` dá ao código dentro do sandbox uma variável `GITHUB_TOKEN` que contém apenas o placeholder. Assim, agentes podem chamar APIs autenticadas sem que a chave de API fique visível para eles.

O artigo acrescenta um alerta. Um host permitido ainda pode devolver uma credencial na própria resposta. Por isso, use regras de credencial apenas com serviços em que você confia, e mantenha os métodos e caminhos bem restritos.

Para executar o sandbox com o token do GitHub disponível para o proxy:

```bash
GITHUB_TOKEN="$(gh auth token)" npx --yes external-sandbox-v86 \
  --proxy-config proxy.json \
  --preserve-paths --mount-rw "project=$PWD"
```

**O que isso faz:**
- `GITHUB_TOKEN="$(gh auth token)"` define o token real apenas no host, obtido a partir do login da CLI do GitHub.
- `--proxy-config proxy.json` carrega a allow-list e as regras de credencial mostradas acima.
- O token nunca entra no sandbox. Apenas o placeholder entra.

## Monitorando a rede e editando a política
O artigo mostra uma sessão na interface web do sandbox em que diferentes requisições são bloqueadas ou permitidas de acordo com a configuração de rede de exemplo. O script `fetch.py` nessa sessão faz uma requisição e exibe o resultado.

A página `/proxy` da interface web permite editar a política de rede enquanto a sessão está em execução. Ela também mostra um log das requisições recentes que o proxy permitiu ou negou. A página `/proxy` existe apenas se o servidor foi iniciado com `--proxy` ou com uma configuração inicial dada por `--proxy-config proxy.json`.

O formato completo da política é compartilhado pelos três sandboxes e está descrito na seção de proxy da especificação do protocolo. O guia do proxy do v86 tem mais exemplos. O artigo também inclui capturas de tela da interface web, que não estão na extração de texto.

## Como cada agente se conecta a um sandbox
Diferentes agentes de programação oferecem formas diferentes de fazer com que cada comando executado rode em um ambiente personalizado. O artigo aborda três mecanismos.

### CLAUDE_CODE_SHELL_PREFIX
O mecanismo do Claude Code é uma variável de ambiente. Quando `CLAUDE_CODE_SHELL_PREFIX` está definida, a ferramenta Bash do Claude executa cada comando com esse valor como prefixo. O efeito é este:

```bash
claude-shell-prefix.sh 'uname -m; pwd; printf "hello from Linux\n"'
```

**O que isso faz:**
- O script de prefixo recebe o comando completo como uma única string de shell, aqui `uname -m; pwd; printf "hello from Linux\n"`.
- Colocar o comando inteiro como um único argumento preserva ponto e vírgula e variáveis de ambiente para que o sandbox as execute.
- O script pode então enviar essa string à API do sandbox, em vez de executá-la no host.
- O artigo observa que isso funciona desde que o ambiente de execução consiga lidar com strings de shell completas, incluindo variáveis de ambiente e ponto e vírgula.

### Codex exec-server
O Codex tem um mecanismo `exec-server`, um pequeno servidor JSON-RPC para criar e controlar subprocessos. A OpenAI fornece a própria implementação como parte do Codex. Como o Codex é open source, os detalhes podem ser extraídos e uma implementação separada pode ser construída. A equipe fez isso, para que o Codex possa ser apontado para um servidor que implementa a especificação JSON-RPC e controlar outro ambiente. O arquivo `environments.toml` mostrado na seção do Codex faz esse apontamento, e a `url` dele é o endpoint WebSocket `/codex` do sandbox.

### Extensões do Pi
O agente de programação Pi permite que extensões substituam as ferramentas padrão por versões personalizadas. A external-sandbox-pi substitui as ferramentas bash, read, write, edit, grep, find e ls por versões que atuam sobre o sandbox configurado. Ela também direciona os atalhos `!command` e `!!command` do Pi para o sandbox. `!command` executa um comando e adiciona a saída à conversa, enquanto `!!command` o executa sem enviar a saída ao modelo.

Executar o Pi com `-e npm:external-sandbox-pi --no-builtin-tools` garante que o Pi não tenha ferramentas que rodem no seu próprio computador, mesmo que a extensão não carregue. Se o sandbox não puder ser alcançado, as ferramentas devolvem um erro em vez de recorrer aos padrões.

Para confirmar que os comandos chegam ao sandbox, execute isto dentro do Pi:

```text
!uname -a
```

**O que isso faz:**
- `!uname -a` roda pelo sandbox porque a extensão redireciona o atalho `!`.
- Você deve ver `Linux` em vez de `Darwin`. O restante da saída depende de qual sandbox você executa.

Para fazer o Pi sempre usar o sandbox, instale a extensão e defina uma variável de ambiente:

```bash
pi install npm:external-sandbox-pi
export EXTERNAL_SANDBOX_API_URL=http://127.0.0.1:8686
```

**O que isso faz:**
- `pi install` adiciona a extensão de forma permanente, diferentemente da forma `-e` por execução.
- `EXTERNAL_SANDBOX_API_URL` informa à extensão qual sandbox usar. Coloque a linha de export no seu perfil de shell para mantê-la entre sessões de terminal.

Para remover a extensão:

```bash
pi remove npm:external-sandbox-pi
```

**O que isso faz:**
- `pi remove` desinstala a extensão que o `pi install` adicionou.

## Nos conte o que você acha
O artigo diz que esses sandboxes ainda são protótipos experimentais. Eles demonstram bem o padrão de sandbox externo, mas não passaram por auditoria no nível necessário para confiar neles com cargas de trabalho sensíveis.

Duas limitações são destacadas:

> ⚠️ **Montagens com escrita ainda podem causar danos.** Uma montagem com leitura e escrita permite que o agente danifique qualquer coisa naquela pasta. Mantenha a pasta sob controle de versão e use montagens somente leitura sempre que possível.

> ⚠️ **Dependências específicas de plataforma.** Os sandboxes rodam Linux, então dependências compiladas para macOS, como uma pasta `node_modules` ou `.venv` no seu projeto, podem precisar ser reinstaladas dentro do sandbox.

A conclusão do autor é que três implementações de sandbox funcionando com três agentes clientes mostram que o protocolo permite combinar modelos, agentes e plataformas de sandboxing. Para construir um sandbox com outra tecnologia, comece pela especificação e pela ferramenta de conformidade. Feedback e relatos de falhas de design vão para as issues do repositório external-sandbox.

## 🧠 Modelos mentais
### Padrão de sandbox externo
**O modelo:** o agente roda no host e toma suas decisões ali, enquanto cada comando que ele executa roda em um sandbox separado, que o agente não consegue modificar. A fronteira fica entre o agente e os comandos, e não em volta do computador inteiro.
**Como aplicar:** ao dar novas capacidades a um agente, pergunte de que lado da fronteira essa capacidade vive e se o agente conseguiria alterar o sandbox a partir de dentro.

### Uma fronteira que não depende de julgamento
**O modelo:** a segurança de uma verificação baseada em modelo, como um modo automático ou uma revisão por IA, depende de o modelo julgar corretamente cada ação. Um sandbox limita o que pode acontecer, qualquer que seja a decisão do modelo.
**Como aplicar:** use verificações baseadas em modelo como uma camada de conveniência, e confie em limites impostos para tudo o que não pode acontecer, principalmente ao usar modelos mais fracos ou locais.

### Privilégio mínimo por montagem e host
**O modelo:** dê ao sandbox apenas as pastas de que ele precisa, somente leitura quando possível, e apenas os hosts e métodos HTTP de que ele precisa. As credenciais ficam no host e são inseridas pelo proxy.
**Como aplicar:** comece sem rede e com uma montagem somente leitura, e então adicione um host ou uma pasta de cada vez, quando uma tarefa comprovar que precisa deles.

## 🔑 Principais conclusões
- Um sandbox protege independentemente do julgamento do modelo, contraste que o artigo faz com as verificações de IA do modo automático do Claude Code e da revisão automática do Codex.
- Manter o agente fora do sandbox evita que ele modifique a si mesmo ou o próprio ambiente, e mantém a fronteira mais fácil de auditar.
- Por padrão, os sandboxes não têm acesso externo à rede, e um proxy libera apenas os hosts e métodos HTTP que você configurar.
- A substituição de credenciais permite que um agente faça chamadas de API autenticadas enquanto a chave real fica no host, invisível para o agente.
- As ferramentas Read e Edit do Claude Code rodam no host, então apenas os comandos Bash dele são isolados em sandbox. O artigo destaca isso como uma limitação.
- O mesmo protocolo em localhost funciona com três sandboxes (v86 na porta 8686, Apple Containers na 8687, Quicksand na 8688) e com três agentes.

## 🏛️ Para o Kenji
Esta é uma forma prática de rodar agentes com modelos locais ou mais baratos sem dar a eles acesso à sua máquina inteira. Ela se aplica diretamente a como você governaria ferramentas de agentes em um time: decidir quais arquivos, hosts e credenciais cada agente pode usar, e impor isso em uma fronteira, em vez de depender de aprovações ação por ação. A pergunta que isso levanta para o seu trabalho de arquitetura é quais dos seus próprios fluxos de trabalho com agentes ainda dependem do julgamento de um modelo para a segurança, e quais poderiam transferir essa responsabilidade para uma fronteira imposta. (Esta é a sua interpretação, não a do autor.)

## 📚 Plano de leitura
### Do artigo
- [external-sandbox-v86](https://github.com/prime-radiant-inc/external-sandbox-v86) — o sandbox em Node.js e WebAssembly, o mais simples para começar.
- [external-sandbox-container](https://github.com/prime-radiant-inc/external-sandbox-container) — a variante com Apple Containers, para Macs com Apple silicon.
- [external-sandbox-quicksand](https://github.com/prime-radiant-inc/external-sandbox-quicksand) — a variante baseada em QEMU, distribuída como pacote Python.
- [especificação](https://github.com/prime-radiant-inc/external-sandbox/blob/main/PROTOCOL.md?ref=blog.primeradiant.com) — o protocolo que os três sandboxes implementam, incluindo o formato da política do proxy.
- [ferramenta de conformidade](https://github.com/prime-radiant-inc/external-sandbox?ref=blog.primeradiant.com) — a ferramenta de testes para verificar um novo servidor de sandbox contra o protocolo.
- [guia do proxy do v86](https://github.com/prime-radiant-inc/external-sandbox-v86/blob/main/docs/proxy.md?ref=blog.primeradiant.com) — mais exemplos de configuração do proxy.
- [the lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/?ref=blog.primeradiant.com) — o enquadramento de risco por trás do padrão de não ter acesso à rede.
- [modo automático](https://code.claude.com/docs/en/auto-mode-config?ref=blog.primeradiant.com) — a abordagem de verificação por IA do Claude Code, que o artigo contrasta com o sandboxing.
- [revisão automática](https://alignment.openai.com/auto-review/?ref=blog.primeradiant.com) — a abordagem equivalente de verificação por IA do Codex.
### Para ir além
- Sprites da Fly — uma abordagem hospedada que roda o agente dentro do sandbox, a alternativa que a equipe considerou e descartou.
- A seção de proxy da especificação do protocolo de sandbox externo — leia-a junto com o guia do proxy do v86 se você planeja escrever uma política para um projeto real.
