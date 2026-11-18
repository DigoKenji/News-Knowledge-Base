---
type: Article Summary
title: Executando agentes de programação sem entregar a eles minhas credenciais
description: Mantenha as credenciais e os comandos que as usam no seu laptop, e deixe agentes remotos pedirem ações aprovadas pela política, executadas localmente.
resource: https://blog.antonio.cx/running-agents-with-lend
tags:
- security
- appsec
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://blog.antonio.cx/running-agents-with-lend
  title: Running coding agents without handing them my credentials
  author: Antonio Mika
status: stable
source_url: https://blog.antonio.cx/running-agents-with-lend
source: Hacker News (100+ pts)
source_type: blog
published: '2026-11-13'
domain: security
subdomain: appsec
concepts:
- model-context-protocol
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81c8ba91c6ed87dc70f6
lang: pt-br
translation_of: /notes/security/2026-11-17-running-coding-agents-without-handing-them-my-credentials.md
---

# Executando agentes de programação sem entregar a eles minhas credenciais

**Fonte:** https://blog.antonio.cx/running-agents-with-lend
**Autor:** Antonio Mika
**Data:** 2026-11-13
**Conceitos:** [Model Context Protocol (MCP)](/pt-br/concepts/model-context-protocol.md)

---

## 🎯 Tese central
Mantenha suas credenciais, e os comandos que as usam, no seu laptop, e deixe agentes remotos pedirem ações específicas que você aprova localmente, em vez de entregar a eles os segredos.

## 🧭 Por que o autor escreveu
O autor executa agentes de programação em VMs remotas descartáveis, mas esses agentes frequentemente precisam agir como ele: ler o CI pelo `gh`, chamar APIs com tokens do 1Password e fazer SSH em outros hosts. As duas soluções usuais, copiar tokens para a VM ou encaminhar o agente SSH com `ssh -A`, concedem mais autoridade do que a tarefa exige. Se a tese se sustentar, os agentes obtêm acesso funcional sem jamais possuir o segredo, e cada uso sensível é aprovado e registrado na máquina onde o segredo está.

## Contexto
O autor usa agentes intensivamente como uma cadeia de ferramentas: Claude Code em um repositório, Codex revisando um branch em outro, e um job de longa duração em segundo plano, frequentemente revisando o trabalho uns dos outros. Nenhum deles roda no laptop. Eles rodam em VMs que podem ser descartadas, enquanto o laptop guarda o que importa: chaves SSH, um cofre do 1Password e as CLIs do `gh` e de nuvem logadas como ele.

### VMs remotas no picd
Os agentes rodam no picd, o serviço de hospedagem da pico.sh. Ele oferece uma VM pessoal com um engine Docker e um shell via SSH. O autor executa o herdr e os agentes ali, e o diretório home persiste entre reinicializações, então dotfiles e ferramentas continuam no lugar.

A configuração funciona bem até que um agente precise de algo que existe apenas no laptop. O restante do artigo explica como o autor executa agentes em paralelo com o herdr e o Ovis, e como a ferramenta lend lida com credenciais.

## herdr
O herdr é um multiplexador de terminal feito para agentes de programação. Ele oferece os workspaces, abas e painéis habituais, e também sabe qual agente roda em cada painel e o que esse agente está fazendo. A barra lateral mostra se cada agente está trabalhando, aguardando o usuário ou finalizado, o que facilita ver qual agente precisa de atenção.

### Controle de agentes pela CLI do herdr
A CLI se comunica com a sessão em execução, então agentes podem dirigir outros agentes. Um agente pode iniciar o Codex em um segundo painel, enviar um prompt a ele, esperar até que ele termine e depois ler o que ele disse. Os três comandos abaixo mostram esse fluxo.

```bash
herdr agent start reviewer --kind codex --pane "$PANE"
herdr agent prompt reviewer "Review the diff on am/fix-retries" --wait --until done
herdr agent read reviewer --source recent
```

**O que isso faz:**
- `agent start` cria um agente nomeado de um determinado tipo (aqui `codex`) no painel identificado por `$PANE`.
- `agent prompt ... --wait --until done` envia uma tarefa e bloqueia até que o revisor informe que terminou, para que quem chamou não precise fazer polling.
- `agent read ... --source recent` recupera a saída recente do revisor para que o agente que chamou possa agir com base na revisão.
- O autor usa isso para revisão de agente por agente, por isso o revisor é um painel separado e não um subprocesso.

### Máquinas remotas
Hosts remotos funcionam da mesma forma. `herdr machine add agent-vm` configura o herdr na VM e adiciona as sessões dessa VM ao cliente local. Essas sessões continuam em execução depois que o cliente local se desconecta.

## Ovis
O Ovis é um cliente de navegador e de celular para o herdr. Ele mostra a transcrição de cada agente como um chat, com o terminal ao vivo a um toque de distância. Quando um agente pede aprovação ou tem uma pergunta, o Ovis transforma isso em um cartão que o usuário pode responder, e envia uma notificação push. Ele lê as máquinas já salvas no herdr, então as VMs aparecem sem configuração extra. O autor acessa o Ovis pelo Tailscale.

> 🔎 Contexto adicional: o Tailscale é um serviço de rede privada que conecta os dispositivos de um usuário entre si, de modo que o celular consiga alcançar a VM sem um endereço público.

## O problema das credenciais
Agentes em uma VM ainda precisam agir como o usuário. Há duas formas usuais de organizar isso, e o autor não gosta de nenhuma delas.

### Copiar credenciais para a VM
A primeira forma é copiar as credenciais para a VM. Um token em `~/.config/gh/hosts.yml` pode ser lido por todo processo naquela máquina, incluindo o agente e tudo o que ele executa, enquanto o token continuar válido.

> ❌ **Anti-padrão — copiar credenciais para o host remoto:** Colocar um arquivo de token de longa duração na VM para que as ferramentas o usem diretamente. **Por que as pessoas fazem isso:** É a forma mais simples de fazer o `gh`, as CLIs de nuvem e os scripts de API funcionarem em uma máquina remota. **Consequência:** Todo processo na VM, incluindo qualquer agente e tudo o que ele executa, pode ler o token, e o token continua funcionando até ser revogado ou expirar. **Correção:** Mantenha o token na máquina onde foi emitido e direcione a chamada para lá, como o lend faz.

### Encaminhar o agente SSH com ssh -A
A segunda forma é encaminhar o agente SSH com `ssh -A`. As chaves privadas permanecem no laptop, mas a VM pode usar todas as chaves do agente sem pedir e sem nenhum registro do que foi assinado. Isso também cobre apenas chaves SSH, então não ajuda com tokens do `gh` ou de APIs.

> ❌ **Anti-padrão — encaminhamento irrestrito do agente:** Ativar o `ssh -A` para que o host remoto possa usar todas as chaves carregadas. **Por que as pessoas fazem isso:** É a forma padrão de deixar um jump host ou uma VM usar chaves SSH sem copiá-las. **Consequência:** O lado remoto pode assinar qualquer coisa a qualquer momento, e nada registra quais assinaturas foram feitas. **Correção:** Exponha apenas as chaves de que você precisa, exija aprovação por requisição e registre o que foi assinado.

O objetivo é que as credenciais permaneçam no laptop e que os comandos que precisam delas também rodem ali. O agente pede para executar algo, o autor aprova com o Touch ID, e o agente recebe a saída.

## lend
O lend é a ferramenta que o autor escreveu para isso. É um proxy que fica na frente do agente SSH existente, e o autor encaminha o socket dele no lugar do socket real. SSH e Git continuam funcionando normalmente, e o lend acrescenta uma forma de executar comandos por meio de uma extensão do agente SSH.

### lend como proxy na frente do agente SSH
Quando um agente na VM pede para executar `gh`, a requisição atravessa o socket encaminhado e o `gh` roda no laptop com o login do autor. O token nunca sai do laptop, então o agente nunca o possui. Toda requisição, seja uma assinatura ou um comando, é verificada contra uma política no laptop e precisa da aprovação do autor antes que qualquer coisa aconteça.

```text
Remote: lend exec -- command args
                  |
          forwarded SSH agent socket
                  |
Laptop: lend agent -> policy + local approval -> local command
                                           -> original SSH agent (signing)
```

**O que isso faz:**
- A primeira linha mostra o ponto de entrada remoto: `lend exec` recebe um comando e seus argumentos.
- No laptop, o `lend agent` aplica a política e a aprovação local primeiro, e depois executa localmente o comando aprovado.
- O caminho de assinatura vai para o agente SSH original, então o SSH e a assinatura do Git normais continuam funcionando pelo mesmo proxy.

### Política: negação por padrão no laptop
A política fica no laptop e lista o que a VM pode solicitar. Todo o resto é negado. A política do autor é curta:

```json
{
  "default": "deny",
  "commands": [
    {"executable": "/opt/homebrew/bin/gh", "approval": "biometric:inline"}
  ],
  "ssh": {
    "default": "deny",
    "keys": [
      {"fingerprint": "SHA256:...", "approval": "biometric:inline"}
    ]
  }
}
```

**O que isso faz:**
- `"default": "deny"` no topo significa que qualquer comando não listado é recusado sem nenhum aviso.
- A entrada `commands` permite exatamente um executável, o caminho do Homebrew para o `gh`, e associa a ele um modo de aprovação.
- `"approval": "biometric:inline"` coloca o Touch ID na própria caixa de diálogo da requisição, então o autor lê o pedido e toca uma vez.
- O bloco `ssh` também é negação por padrão, e somente a impressão digital listada é oferecida à VM.

Uma regra também pode ter um `args_prefix`, como `["pr", "view"]`, que restringe a regra a alguns subcomandos. `biometric` simples é o outro modo de aprovação. Ele pede o Touch ID em um segundo prompt depois que o autor clica em Permitir, enquanto `biometric:inline` combina os dois em uma única etapa.

### Conectando com lend ssh
Para se conectar, o autor executa `lend ssh` no laptop. Ele inicia um proxy só para aquela conexão e o encerra quando o SSH termina, então o tempo de vida do proxy coincide com o da sessão.

```bash
lend ssh --config ./policy.json -- agent-vm
```

**O que isso faz:**
- `lend ssh` envolve a conexão SSH, então o socket encaminhado existe apenas enquanto a sessão estiver aberta.
- `--config ./policy.json` indica a política do laptop que governa cada requisição durante essa sessão.
- `-- agent-vm` é o host ao qual se conectar, passado depois do separador para não ser lido como uma flag do lend.

### Executando comandos via MCP
O autor executa o herdr na VM, e o Claude Code se comunica com o lend por meio de `lend mcp`. Esse é um servidor MCP com uma ferramenta, `run_local_command`. A ferramenta recebe o comando a executar e um motivo, e o motivo é obrigatório.

```json
{
  "mcpServers": {
    "laptop": {"command": "lend", "args": ["mcp"]}
  }
}
```

**O que isso faz:**
- A chave `laptop` é apenas o nome do servidor; o agente vê a ferramenta como vinda desse servidor.
- `"command": "lend"` com `"args": ["mcp"]` inicia localmente, na VM, o servidor MCP do lend.
- O motivo obrigatório é mostrado a quem aprova no laptop, o que faz com que cada requisição explique a si mesma.

Em um exemplo, o autor pediu ao Claude Code na VM a descrição do repositório do lend. O agente chamou a ferramenta com `gh repo view`, o Mac exibiu a requisição, e um toque aprovou. A caixa de diálogo mostra o caminho completo do executável, todos os argumentos, o diretório em que o comando será executado no laptop, o motivo e o que o processo remoto disse sobre si mesmo. Após a aprovação, stdout, stderr e o código de saída são transmitidos de volta ao agente. O motivo e os detalhes do chamador vêm da VM, então o lend os marca como não verificados. Eles ajudam quem aprova a decidir, mas não podem mudar o que a política permite.

### Usando lend exec sem MCP
A mesma requisição funciona como uma chamada simples de CLI a partir da VM, sem MCP:

```bash
lend exec --reason "Read the repo description" -- gh repo view antoniomika/lend
```

**O que isso faz:**
- `lend exec` envia o comando ao laptop da mesma forma que a ferramenta MCP.
- `--reason` é obrigatório, então quem aprova sempre vê por que o comando está sendo executado.
- Tudo o que vem depois de `--` é o comando e seus argumentos, que a política verifica antes de qualquer execução.
- O comando aqui é `gh repo view antoniomika/lend`, uma chamada somente de leitura que se encaixa no estilo de regras estreitas com `args_prefix` do autor.

### Mantendo o socket estável entre reconexões
Os painéis do herdr sobrevivem a uma desconexão do SSH, mas o socket encaminhado pertence a uma conexão SSH específica. Após uma reconexão, o SSH fornece um novo caminho de socket, e os agentes que continuam rodando nos painéis mantêm o `SSH_AUTH_SOCK` antigo. O autor resolve isso com um symlink em um caminho fixo que é reapontado a cada conexão.

#### O hook ~/.ssh/rc
O autor atualiza o symlink a partir de `~/.ssh/rc` na VM. O SSH executa esse arquivo antes de iniciar o shell, então o script enxerga o socket encaminhado pela nova conexão.

```sh
#!/bin/sh

if [ -n "${SSH_AUTH_SOCK:-}" ] &&
    [ "$SSH_AUTH_SOCK" != "$HOME/.ssh/lend/agent.sock" ] &&
    "$HOME/.local/bin/lend" doctor --remote --socket "$SSH_AUTH_SOCK" --timeout 1s >/dev/null 2>&1; then
  install -d -m 700 "$HOME/.ssh/lend" &&
    ln -sfn -- "$SSH_AUTH_SOCK" "$HOME/.ssh/lend/agent.sock"
fi

if [ -n "${DISPLAY:-}" ] && read -r _ssh_proto _ssh_cookie; then
  case $DISPLAY in
    localhost:*) _ssh_display="unix:${DISPLAY#localhost:}" ;;
    *) _ssh_display=$DISPLAY ;;
  esac
  printf 'add %s %s %s\n' "$_ssh_display" "$_ssh_proto" "$_ssh_cookie" | /usr/bin/xauth -q -
fi
```

**O que isso faz:**
- A primeira condição verifica se há um socket presente, se ele não é já o caminho estável e se o `lend doctor --remote` confirma que ele fala o protocolo do lend em até um segundo.
- Só depois dessa verificação é que `install -d -m 700` cria o diretório privado e `ln -sfn` reaponta o link estável.
- O script chama o lend pelo caminho absoluto porque roda antes de o shell adicionar `~/.local/bin` ao `PATH`.
- O segundo bloco trata o X11: quando `DISPLAY` está definido, ele passa o cookie ao `xauth`, já que o SSH delega essa configuração a este arquivo quando ele existe.

#### A verificação em ~/.bashrc
Os shells nos painéis do herdr então leem o caminho estável, mas somente quando ele funciona.

```bash
if [ -S "$HOME/.ssh/lend/agent.sock" ] &&
    "$HOME/.local/bin/lend" doctor --remote --socket "$HOME/.ssh/lend/agent.sock" --timeout 1s >/dev/null 2>&1; then
  export SSH_AUTH_SOCK="$HOME/.ssh/lend/agent.sock"
fi
```

**O que isso faz:**
- O teste `-S` confirma que o caminho estável é um socket antes de o shell usá-lo.
- A mesma verificação `lend doctor --remote` roda novamente com timeout de um segundo, então um socket morto ou que não seja do lend nunca é exportado.
- `export SSH_AUTH_SOCK` altera o ambiente somente quando as duas verificações passam.
- Se qualquer uma das verificações falhar, a variável é mantida como estava, o que evita apontar os agentes para um socket morto.

Novos shells Bash nos painéis do herdr usam o caminho estável quando ele funciona. O MCP, os hooks de aprovação e o Git herdam o caminho quando iniciam. Após uma reconexão, o hook do SSH atualiza o symlink, e esses processos passam a usar o novo socket nas requisições seguintes. Agentes já em execução com o caminho antigo precisam de uma reinicialização com o caminho estável configurado.

A conexão SSH a partir do laptop precisa permanecer aberta. As requisições falham enquanto ela estiver desconectada, e uma reconexão inicia uma nova sessão do lend com aprovações e concessões novas. O link aponta para uma conexão por vez, então a atualização mais recente prevalece.

### Segredos que o agente nunca vê
O `gh` funciona porque já está logado no laptop. Algumas ferramentas precisam que um token seja passado, e para esses casos uma regra de comando pode ler o segredo com `op read` e passá-lo ao comando aprovado como variáveis de ambiente.

```json
{
  "secret_reader": "/opt/homebrew/bin/op",
  "secret_reader_args": ["read", "--no-newline"],
  "commands": [
    {
      "executable": "/opt/homebrew/bin/gh",
      "args_prefix": ["pr", "view"],
      "approval": "biometric",
      "secrets": {"GH_TOKEN": "op://Development/github/token"}
    }
  ]
}
```

**O que isso faz:**
- `secret_reader` e `secret_reader_args` dizem ao lend para buscar segredos com `op read --no-newline` no laptop.
- `args_prefix` limita essa regra a `gh pr view`, então o token injetado é usado apenas para esse subcomando.
- `"approval": "biometric"` pede o Touch ID em um segundo prompt depois de Permitir, em oposição ao modo inline usado antes.
- `"secrets": {"GH_TOKEN": "op://Development/github/token"}` mapeia o nome da variável de ambiente para uma referência do 1Password, então a VM fornece apenas a referência e nunca o valor.

Quando o agente pede para executar `gh pr view` e o autor aprova, o lend lê o token do 1Password, executa o `gh` com `GH_TOKEN` definido e devolve a saída. O token permanece no laptop, a VM não pode escolher quais variáveis são definidas ou quais referências são lidas, e o prompt mostra apenas os nomes das variáveis e as referências, nunca os valores. O autor também executou `gh api user` na VM com essa regra, e o 1Password pediu o próprio Touch ID antes que o `op read` devolvesse o token. A VM recebeu o login de volta, e o ambiente dela não tinha nenhum `GH_TOKEN`.

Há uma ressalva. Isso só vale se o comando aprovado não imprimir o próprio ambiente. Aprovar `env` ou `sh` devolve os valores ao agente.

> ❌ **Anti-padrão — aprovar env ou sh:** Permitir uma regra que executa `env`, `printenv` ou um shell capaz de ecoar variáveis enquanto um segredo está sendo injetado. **Por que as pessoas fazem isso:** Um shell ou o `env` parecem um comando de uso geral conveniente para aprovar. **Consequência:** Os segredos injetados aparecem na saída do comando, que é transmitida diretamente de volta ao agente. **Correção:** Aprove apenas ferramentas específicas que não imprimem o próprio ambiente, e nunca um shell genérico para uma regra que injeta segredos.

> ❌ **Anti-padrão — permitir o op como comando:** Colocar a própria CLI do 1Password na lista de permissões. **Por que as pessoas fazem isso:** Parece natural permitir a ferramenta que guarda os segredos. **Consequência:** O `op read` imprime o segredo, e essa saída vai diretamente para o agente. **Correção:** Mantenha o `op` apenas como `secret_reader`, e nunca como comando aprovado.

### Chaves SSH
As chaves têm política própria. Chaves que não estão listadas ficam ocultas da VM. Uma chave listada pode ser limitada pelo que pode assinar, e `max_signatures` limita quantas assinaturas a chave pode fazer em uma sessão. O artigo não informa um valor para esse limite.

### Concessões
Uma regra pode oferecer uma concessão quando o autor aprova, como cinco minutos ou três usos. Uma concessão existe apenas dentro de uma sessão de `lend ssh` e corresponde somente ao comando e aos argumentos exatos que foram aprovados. Ela termina quando a sessão termina ou quando a tela é bloqueada, então uma concessão não pode sobreviver à conexão que a criou.

### Vendo o que aconteceu
`lend status --all --watch` mostra o que está conectado, o que está aguardando, o que está em execução e quaisquer concessões ativas. `lend pause --all` cancela tudo e recusa novas requisições até que o autor retome. `lend history` mantém um registro do que foi executado e de como foi aprovado. Ele deixa de fora argumentos, saídas e segredos, e o autor pode optar por registrar os argumentos.

### Respondendo a prompts do agente a partir do laptop
Agentes também pedem permissão para coisas não relacionadas a credenciais, como executar um comando de shell na VM, editar um arquivo ou buscar uma URL. O `lend approve` envia essas perguntas para a mesma caixa de diálogo no Mac. Ele não executa nada no laptop. Ele pergunta ao autor, e o agente decide o que fazer com a resposta.

#### O hook PermissionRequest
No caso do Claude Code, o hook é um hook `PermissionRequest` na VM:

```json
{
  "hooks": {
    "PermissionRequest": [
      {"matcher": "*", "hooks": [{"type": "command", "command": "lend approve --hook claude-code"}]}
    ]
  }
}
```

**O que isso faz:**
- `"PermissionRequest"` é o evento que o Claude Code dispara quando, de outra forma, pararia e pediria no terminal.
- `"matcher": "*"` envia cada pedido de permissão pelo hook.
- O hook executa `lend approve --hook claude-code`, que encaminha o pedido para a caixa de diálogo do laptop em vez do terminal.
- A aprovação em si acontece no laptop, então o terminal da VM deixa de ser o lugar onde a decisão é tomada.

#### Tópicos de aprovação no laptop
A política do laptop lista quais tópicos podem pedir aprovação, e cada tópico tem seu próprio modo de aprovação.

```json
"approvals": {
  "topics": [{"name": "claude-code", "approval": "biometric:inline"}]
}
```

**O que isso faz:**
- O array `topics` nomeia o tópico `claude-code`, que o hook acima usa.
- `"approval": "biometric:inline"` aplica a caixa de diálogo inline do Touch ID a cada prompt de permissão do Claude Code.

Há adaptadores para Codex, Antigravity e pi, além de um formato JSON simples para outras ferramentas. O mesmo mecanismo funciona em scripts. Por exemplo, `lend approve --topic deploy --title "terraform apply" && terraform apply` executa o apply somente depois que o autor aprova no laptop. Se o laptop não puder ser alcançado, o hook se afasta e o agente pergunta na própria interface, como faria normalmente.

## O que o lend não protege
Um comando aprovado roda com o usuário do autor e pode acessar os arquivos dele. A aprovação decide o que é executado, mas o lend não isola o comando em um sandbox. Se o autor aprovar um shell, um interpretador ou qualquer coisa que leia instruções da entrada padrão, ele efetivamente aprovou a execução de qualquer coisa no laptop, porque a entrada padrão é transmitida após a aprovação e não é mostrada no prompt.

> ❌ **Anti-padrão — aprovar um shell, interpretador ou leitor de stdin:** Permitir `sh`, `python` ou qualquer comando que leia seu programa da entrada padrão. **Por que as pessoas fazem isso:** Uma ferramenta de uso geral poupa escrever regras estreitas para cada tarefa. **Consequência:** Após uma única aprovação, o fluxo de entrada pode carregar código arbitrário que o prompt nunca exibiu. **Correção:** Permita subcomandos estreitos, como `gh pr view` com `args_prefix`, e nunca um interpretador genérico.

Os detalhes do chamador no prompt vêm da VM e podem ser falsificados. Qualquer um que consiga alcançar o socket encaminhado na VM, inclusive o root, pode fazer requisições enquanto a sessão estiver aberta. A política ainda limita o que pode ser pedido, e cada requisição ainda precisa de aprovação.

> ❌ **Anti-padrão — confiar em contexto fornecido pelo chamador:** Ler o motivo ou os detalhes do chamador na caixa de diálogo como fatos verificados. **Por que as pessoas fazem isso:** A caixa de diálogo parece autoritativa, e o motivo está escrito em linguagem simples. **Consequência:** Um agente comprometido ou confuso pode descrever um comando inofensivo enquanto pede um prejudicial. **Correção:** Avalie o executável real, os argumentos completos e o diretório de trabalho, que o lend mostra, e trate o motivo como não verificado.

O artigo também declara suas limitações com clareza. O lend está em estágio inicial e roda em macOS e Linux. Ele não aloca um PTY, então programas interativos não funcionam por meio dele. No Linux, a aprovação é uma caixa de diálogo do Zenity ou um prompt de terminal, e a aprovação biométrica é exclusiva do macOS.

> 🔎 Contexto adicional: um PTY (pseudo-terminal) é o que permite que um programa se comporte como se tivesse um terminal interativo; sem ele, programas que esperam entrada do teclado ou controle de terminal em tela cheia não funcionam corretamente.

## Trabalhos anteriores
O [ssh-rev-exec](https://github.com/KOBA789/ssh-rev-exec) envia requisições de execução local por um proxy do agente SSH. O [op-forward](https://github.com/ekovshilovsky/op-forward) encaminha comandos da CLI do 1Password para uma máquina que tem autenticação de desktop. O lend é uma implementação separada da mesma técnica de extensão do agente, e acrescenta política por requisição, aprovação local e filtragem de chaves.

## Experimentando
Instale o lend tanto no laptop quanto no host remoto. O artigo indica o Homebrew e uma opção por script de release.

```bash
brew install antoniomika/tap/lend
# or
curl -fsSL https://github.com/antoniomika/lend/releases/latest/download/install.sh | sh
```

**O que isso faz:**
- `brew install antoniomika/tap/lend` instala a partir do tap do Homebrew do autor.
- A linha `curl ... | sh` executa o instalador da release mais recente no GitHub, uma opção que não precisa do Homebrew.
- O artigo diz para instalar o lend tanto no laptop quanto no host remoto, para que as duas pontas tenham a mesma ferramenta disponível.
- A instalação vem primeiro, antes da política, das verificações do doctor ou de uma sessão.

Execute `lend doctor` no laptop para verificar a política, os executáveis, o agente upstream e a configuração de aprovação. Execute `lend doctor --remote` na VM para confirmar que o socket encaminhado é de fato do lend. O [guia de primeiros passos](https://lend.antonio.cx/getting-started.html) percorre uma primeira sessão, e a [documentação](https://lend.antonio.cx) cobre políticas, aprovação e MCP. O autor pede feedback no [GitHub](https://github.com/antoniomika/lend).

## 🧠 Modelos mentais
### O laptop é a fronteira de confiança
**O modelo:** A VM pode pedir coisas, mas somente o laptop decide o que acontece e guarda os segredos. Agentes remotos são clientes de uma política local.
**Como aplicar:** Ao projetar o acesso de agentes, pergunte onde o segredo fisicamente vive e se o lado remoto apenas solicita ações nomeadas.

### Aprovação decide o que roda, não o que ele faz
**O modelo:** Aprovar um comando é uma decisão sobre qual programa é iniciado, não uma garantia sobre seu comportamento. Um programa estreito com argumentos estreitos mantém essa decisão significativa.
**Como aplicar:** Aprove subcomandos específicos com `args_prefix`, nunca um shell, um interpretador ou um leitor de stdin, e verifique se o programa imprime o próprio ambiente antes de injetar segredos.

### Nomes estáveis sobre identificadores que mudam
**O modelo:** Recursos com escopo de conexão, como um caminho de socket SSH, mudam a cada reconexão. Um symlink fixo permite que processos de longa duração mantenham um mesmo nome enquanto o alvo muda por baixo.
**Como aplicar:** Aponte processos de longa duração para um caminho estável, reaponte-o a cada conexão e verifique o alvo antes de atualizar o link.

## 🔑 Principais conclusões
- Copiar um token para uma VM o expõe a todo processo ali enquanto ele for válido, então o artigo move a credencial para o laptop.
- O `ssh -A` permite que o lado remoto use todas as chaves carregadas sem pedir nem registrar, então o lend expõe apenas as chaves listadas na política.
- Uma política de negação por padrão, com executáveis estreitos e regras com `args_prefix`, faz com que apenas os comandos listados pelo autor possam rodar.
- Segredos podem ser injetados em um único comando aprovado como variáveis de ambiente a partir de um `secret_reader`, e a VM nunca escolhe os valores nem as referências.
- A aprovação cobre o executável exato, os argumentos e o diretório de trabalho mostrados na caixa de diálogo, enquanto o motivo e os detalhes do chamador permanecem não verificados.
- Aprovar um shell, um interpretador, o `env` ou a própria CLI do 1Password anula o design, porque cada um deles pode devolver segredos ou executar código arbitrário.

## 🏛️ Para o Kenji
Este padrão se aplica a qualquer arquitetura em que agentes rodam em ambientes menos confiáveis, mas precisam de acesso a credenciais ou ações privilegiadas, como monitoramento de CI, CLIs de nuvem e APIs internas. A questão de design que ele levanta é quais comandos que carregam segredos devem ser acessíveis por uma interface estreita e aprovada, em vez de serem entregues ao agente. *Esta é a sua interpretação, não a do autor.*

## 📚 Plano de leitura
### A partir do artigo
- [lend no GitHub](https://github.com/antoniomika/lend) — o código-fonte, as releases e o rastreador de issues da ferramenta que o artigo descreve.
- [guia de primeiros passos do lend](https://lend.antonio.cx/getting-started.html) — uma primeira sessão com o lend, passo a passo.
- [documentação do lend](https://lend.antonio.cx) — políticas, modos de aprovação e MCP, que o artigo menciona sem detalhar.
- [herdr no GitHub](https://github.com/herdrdev/herdr) — o multiplexador de terminal para agentes de programação que o autor executa na VM.
- [Ovis](https://ovis.sh) — o cliente de navegador e de celular para o herdr, útil para ver como os cartões de aprovação chegam a um celular.
- [picd](https://cd.pico.sh) — o serviço de hospedagem de VMs que o autor usa para agentes remotos.
- [página de manual do sshd, seção SSHRC](https://man.openbsd.org/sshd.8#SSHRC) — como o `~/.ssh/rc` roda antes do shell, algo de que a correção do socket estável depende.
- [ssh-rev-exec](https://github.com/KOBA789/ssh-rev-exec) — uma implementação anterior de execução remota por um proxy do agente SSH.
- [op-forward](https://github.com/ekovshilovsky/op-forward) — encaminhamento de comandos da CLI do 1Password para uma máquina com autenticação de desktop.
