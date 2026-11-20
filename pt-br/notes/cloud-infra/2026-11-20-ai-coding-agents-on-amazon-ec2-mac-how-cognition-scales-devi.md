---
type: Article Summary
title: 'Agentes de código com IA no Amazon EC2 Mac: como a Cognition escala o Devin para macOS'
description: A Cognition roda uma VM macOS nova para cada sessão do Devin em hosts bare-metal do EC2 Mac, permitindo que agentes de IA construam e testem apps da Apple.
resource: https://aws.amazon.com/blogs/architecture/ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devin-for-macos/
tags:
- cloud-infra
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devin-for-macos/
  title: 'AI coding agents on Amazon EC2 Mac: How Cognition scales Devin for macOS'
  author: Adam Hesch et al. (AWS Architecture Blog)
status: stable
source_url: https://aws.amazon.com/blogs/architecture/ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devin-for-macos/
source: AWS Architecture
source_type: blog
published: '2026-11-19'
domain: cloud-infra
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81c2aff8d9e451289421
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-20-ai-coding-agents-on-amazon-ec2-mac-how-cognition-scales-devi.md
---

## 🎯 Tese central
Um agente de código com IA só consegue construir, executar e verificar apps da Apple se tiver um ambiente macOS real, e a Cognition mostra um caminho para fornecê-lo em escala: uma máquina virtual macOS nova por sessão, rodando em hosts bare-metal do Amazon EC2 Mac, com as mesmas propriedades de disco, rede, configuração e visualização ao vivo que seus workspaces Linux e Windows já tinham.

## 🧭 Por que o autor escreveu
Antes deste lançamento, o Devin rodava apenas em VMs Linux e Windows. Ele conseguia escrever Swift, mas sem macOS e Xcode não conseguia compilar nem testar o app, então os clientes precisavam depender de seu próprio CI ou de frotas de Macs. O artigo explica como a Cognition construiu os workspaces macOS do Devin no EC2 Mac e oferece orientações para planejar uma frota macOS para agentes ou testes automatizados. Se a tese se sustenta, uma plataforma de agentes precisa do macOS como workspace de nuvem de primeira classe, e não como um complemento do CI de outra pessoa.

## Por que um agente de código com IA precisa de um Mac próprio
O objetivo da Cognition era permitir que o Devin construísse apps da Apple. Compilar, testar e assinar apps para iPhone, iPad e Mac exige hardware da Apple e macOS, independentemente de quem ou do que escreveu o código. A Cognition agora roda ambientes macOS em instâncias do Amazon EC2 Mac, e o Devin consegue construir um app iOS, executá-lo no iOS Simulator e verificar seu comportamento antes de devolver uma mudança pronta para merge.

### Build bem-sucedido não é prova de comportamento
Um build bem-sucedido mostra apenas que o código compila. O exemplo do artigo é um jogo para iPhone projetado para retomar de onde o jogador pausou, mesmo depois que o app é fechado. Para verificar isso, um agente precisa jogar, pausar, sair, reabrir e confirmar o estado salvo. Só um app em execução em um Mac em execução pode mostrar isso.

### Propriedades do workspace exigidas pela Cognition
A Cognition precisava que cada workspace macOS fosse igual aos seus outros workspaces de nuvem:
- Um disco que mantenha o trabalho da sessão, para que uma sessão ociosa não mantenha uma VM em execução.
- Acesso à rede que siga as regras de acesso da sessão.
- Um ambiente de desenvolvimento pronto antes de o Devin começar.
- Uma forma de o usuário e o Devin verem e operarem o app em execução.

🔎 Contexto adicional: uma máquina virtual (VM) é um computador emulado por software, que roda seu próprio sistema operacional sobre hardware físico compartilhado.

## Por que a Cognition escolheu a AWS
A Cognition precisava de máquinas macOS para escalar o Devin além de Linux e Windows, mas não queria comprar, montar em rack e manter Macs físicos. O artigo lista as propriedades do EC2 Mac que moldaram o design.

### Hardware Apple silicon na nuvem
Cada instância do EC2 Mac é um Mac mini ou Mac Studio físico. Como ela está conectada ao AWS Nitro System, a Cognition consegue rodar o framework Virtualization da Apple no EC2 Mac da mesma forma que o faria em um Mac local.

### Capacidade por meio de uma API
A Cognition aloca Dedicated Hosts e lança instâncias Mac pela API do Amazon EC2, em vez de encomendar e receber máquinas. A capacidade vira uma chamada de provisionamento, e não uma compra de hardware.

### Regiões ao redor do mundo
O EC2 Mac está disponível em várias Regiões da AWS. A Cognition roda em seis delas.

### Rede e armazenamento padrão da AWS
As instâncias Mac rodam em uma Amazon Virtual Private Cloud (VPC) e inicializam a partir de volumes do Amazon Elastic Block Store (EBS), então a Cognition as gerencia como qualquer outra instância EC2.

## Visão geral da solução
A Cognition constrói os workspaces macOS do Devin em instâncias do EC2 Mac. Elas rodam como instâncias bare-metal em Dedicated Hosts do Amazon EC2, construídos sobre computadores Mac da Apple e o AWS Nitro System.

### Dedicated Hosts e o mínimo de 24 horas
Cada instância do EC2 Mac roda em seu próprio Dedicated Host, e um host tem período mínimo de alocação de 24 horas. Por isso, a Cognition separa a vida de uma sessão do Devin da vida de um host. Uma sessão pode começar e terminar enquanto o host continua alocado, e o host é a unidade que custa dinheiro. Onde a Cognition não tem capacidade pré-alocada, ela usa um grupo do Amazon EC2 Auto Scaling para alocar e liberar hosts.

### O gerenciador de VMs e o framework Virtualization da Apple
Em cada host, um gerenciador de VMs da Cognition usa o framework Virtualization da Apple para rodar VMs convidadas macOS. As imagens dos convidados vêm de imagens que a Cognition prepara e gerencia.

🔎 Contexto adicional: o framework Virtualization da Apple é a API do macOS para criar e executar sistemas operacionais convidados em um Mac.

### Dois workspaces por host, pela licença
Cada host roda dois workspaces isolados do Devin por vez. O Contrato de Licença de Software (SLA) do macOS permite até duas cópias adicionais do macOS em ambientes virtuais em cada Mac, e o macOS aplica esse limite. Sessões ociosas liberam suas VMs para outras sessões, então um único host pode atender várias sessões do Devin por dia.

### Figura 1: um host, muitas sessões
O host roda uma instância Mac bare-metal em uma das seis Regiões, com uma VM convidada por sessão ativa. Cada convidado roda as ferramentas do Xcode e o iOS Simulator com o app em teste. Ele também roda uma visualização ao vivo e uma árvore de acessibilidade, fornecidas pelo accessibility-cli. O navegador do usuário troca vídeo e entrada com a visualização ao vivo, e o agente Devin consulta e age sobre a árvore de acessibilidade.

No host, o gerenciador de VMs executa os convidados. Um processo de snapshot com um frontend de Network Block Device (NBD) cuida da E/S de disco de cada convidado e troca snapshots com o armazenamento de snapshots. O agente Devin e o armazenamento de snapshots rodam como serviços do Devin Cloud na mesma Região, com as sessões em instâncias do Amazon EC2 e os snapshots no Amazon Simple Storage Service (Amazon S3). Um gateway Ethernet em user space cuida da rede, descrito a seguir.

> 📋 **Referência da Figura 1**: o diagrama tem o título "Devin macOS sessions on an Amazon EC2 Mac Dedicated Host" no artigo original.

### Manter o estado da sessão com snapshots de disco
Grande parte do trabalho com o Devin Cloud é assíncrona. Uma sessão ociosa não precisa de uma VM em execução, então a Cognition usa para o macOS o mesmo sistema de snapshots que usa para suas VMs Linux. O sistema de snapshots roda como um processo separado no host e cuida das leituras e gravações no disco da VM. O convidado vê apenas um disco virtual comum, então não precisa saber da existência de snapshots.

#### vhost-user e por que ele não foi transferido
No Linux, a Cognition conecta as VMs ao processo de armazenamento com `vhost-user`. O framework Virtualization não oferece suporte a `vhost-user`, mas oferece suporte a anexos NBD. A Cognition adicionou um frontend NBD ao seu sistema de snapshots e manteve por trás dele o mesmo design de armazenamento, de modo que a camada de armazenamento não precisou mudar.

#### Persistência no desligamento
Quando um convidado desliga, o processo de armazenamento continua rodando para persistir as gravações pendentes e enviar o snapshot atualizado. O gerenciador de VMs espera o envio terminar antes de concluir a limpeza, então parar uma sessão não interrompe o trabalho que a salva.

#### Liberação e retomada de VM ociosa
A Cognition informa que cerca de 90% das sessões liberam sua VM macOS quando ficam ociosas e retomam a partir de um snapshot em segundos. Em média, uma sessão libera e recupera sua VM várias vezes, para que a capacidade Mac vá para trabalho ativo e não para sessões ociosas.

> ❌ **Anti-pattern — manter a VM para uma sessão ociosa:** deixar uma VM macOS em execução anexada a uma sessão que espera pelo usuário. **Por que as pessoas fazem isso:** é o modelo mais simples, já que a VM nunca precisa ser reconstruída. **Consequência:** a capacidade Mac escassa fica ociosa, e o custo de um host continua correndo. **Correção:** persistir o estado do disco como snapshots e liberar a VM quando a sessão ficar ociosa, como faz a Cognition.

### Aplicar regras de acesso à rede com um gateway Ethernet em user space
O Devin Cloud aplica as regras de acesso de cada sessão ao tráfego de saída e bloqueia o acesso à infraestrutura protegida do host. O framework Virtualization oferece três formas de conectar um convidado à rede: rede em ponte (bridged), tradução de endereços de rede (NAT) e um anexo por file handle, que troca quadros Ethernet brutos por um socket Unix de datagramas local.

#### NAT e o filtro de pacotes compartilhado
O NAT exige a menor configuração, mas a implementação da Apple também gerencia regras no `pf`, o filtro de pacotes do macOS. A Cognition também usa o `pf` para seus próprios controles de rede, então os dois sistemas disputavam a configuração do mesmo filtro.

> ❌ **Anti-pattern — dois controladores para um único filtro de pacotes:** deixar o hypervisor e o seu próprio motor de políticas gravarem regras na mesma configuração do `pf`. **Por que as pessoas fazem isso:** a opção NAT embutida é o caminho mais rápido para uma rede de convidado funcionando. **Consequência:** os dois sistemas sobrescrevem ou entram em conflito com as regras um do outro, então a aplicação das políticas se torna imprevisível. **Correção:** escolher um único dono do filtro. A Cognition escolheu um tipo de anexo diferente para poder ser dona do caminho das políticas.

#### O gateway Ethernet em user space
A Cognition escolheu o anexo por file handle e construiu seu próprio gateway Ethernet, que roda em user space no host. Cada VM macOS recebe um gateway separado e sua própria aplicação de políticas de sessão. O gateway responde às requisições ARP (Address Resolution Protocol) do convidado e valida o endereçamento Ethernet. Ele também verifica se o endereço IP de origem de cada pacote corresponde ao endereço atribuído ao convidado. Os pacotes IP aceitos entram em uma interface de túnel no host, onde regras do `pf` e proxies encaminham o tráfego e aplicam a política de acesso da sessão.

🔎 Contexto adicional: ARP é o protocolo que mapeia um endereço IP para um endereço de camada de enlace (Ethernet) em uma rede local.

### Preparar as imagens dos convidados antes do primeiro boot
Um workspace do Devin precisa de mais do que uma instalação nova do macOS. Ele precisa de uma conta de usuário, uma área de trabalho utilizável, as ferramentas do Xcode e as permissões de que o Devin precisa para observar e controlar apps. Uma instalação nova pede permissões como acessibilidade e gravação de tela em caixas de diálogo, e cada diálogo bloquearia o Devin até que uma pessoa o aprovasse.

#### Preparar o estado no host
Para evitar os diálogos, a Cognition monta o disco da imagem do convidado no host e prepara o estado diretamente. Ela cria o usuário e atualiza os bancos de dados internos de configuração do macOS, para que as permissões estejam no lugar antes de o convidado iniciar.

#### Inicializar a imagem preparada
Em seguida, a Cognition inicializa a imagem preparada para instalar as ferramentas de desenvolvimento restantes, verificar o Xcode, aquecer o iOS Simulator e deixar terminar a indexação do Spotlight e outras configurações em segundo plano. Ela também desliga serviços em segundo plano que as sessões de desenvolvimento não precisam. Com a preparação feita antecipadamente, uma nova sessão pode começar a construir e testar assim que sua VM iniciar.

> ❌ **Anti-pattern — fazer a configuração no início da sessão:** deixar cada nova VM instalar ferramentas, aprovar permissões e indexar arquivos enquanto o agente espera. **Por que as pessoas fazem isso:** evita manter uma imagem dourada (golden image). **Consequência:** cada sessão paga o custo total da configuração, e os diálogos podem travar o agente indefinidamente. **Correção:** preparar a imagem uma vez, fora do caminho da sessão, e inicializar a partir dela.

### Oferecer ao usuário uma visualização ao vivo responsiva do app
Os usuários podem acompanhar a área de trabalho macOS em uma visualização VNC (Virtual Network Computing), e o app deles em um novo painel do iOS Simulator na sessão do Devin. De qualquer uma das duas visualizações, o usuário pode testar o app por conta própria e assumir o controle quando quiser.

🔎 Contexto adicional: VNC é um protocolo de área de trabalho remota que envia imagens da tela a um visualizador e devolve entrada de teclado e mouse.

A Figura 2 mostra uma sessão gravada do Devin em que ele constrói um jogo 3D para iPhone e o testa no iOS Simulator, enquanto o painel Computer da sessão mostra a área de trabalho macOS.

Duas etapas decidem se a visualização ao vivo responde rápido o bastante para que os usuários operem o app: a captura da tela e a entrega do vídeo.

#### ScreenCaptureKit e o pool de buffers
Para a área de trabalho macOS, o ScreenCaptureKit, a API de captura de tela da Apple, fornece imagens da tela a partir de um pool limitado de buffers reutilizáveis. Quando a Cognition precisa manter a última imagem capturada, ela guarda sua própria cópia em vez de reter um buffer do pool. Reter um buffer enquanto a tela está parada pode atrasar a próxima mudança de tela.

> ❌ **Anti-pattern — reter um buffer de captura do pool:** manter um buffer do pool de captura para usar como "último quadro" enquanto a tela está parada. **Por que as pessoas fazem isso:** parece mais barato do que copiar a imagem. **Consequência:** o pool de captura fica escasso, e a próxima mudança de tela espera. **Correção:** copiar a imagem que você precisa manter.

#### Pipelines de quadros e backlogs obsoletos
Depois da captura, o vídeo da área de trabalho e do Simulator é codificado, enviado ao navegador do usuário e exibido. Quando uma etapa não consegue acompanhar, os quadros esperam entre as etapas. O stream pode parecer fluido enquanto mostra um estado antigo do app. No exemplo do jogo para iPhone, um usuário pode apertar Pause e ainda ver o jogo se mover, porque o visualizador ainda mostra quadros anteriores ao toque. Para manter a visualização atual, a Cognition limita quantos quadros podem esperar entre as etapas e não reproduz um backlog obsoleto.

> ❌ **Anti-pattern — reproduzir um backlog obsoleto:** enfileirar todos os quadros entre as etapas e reproduzi-los todos em ordem. **Por que as pessoas fazem isso:** parece completo e não perde nada. **Consequência:** o usuário vê um estado antigo, e a entrada parece não fazer nada. **Correção:** limitar a fila entre as etapas e descartar os quadros que já estão desatualizados.

#### Keyframes e recuperação
Quadros de vídeo comprimidos costumam depender de quadros anteriores, então descartar um deles pode deixar os quadros seguintes impossíveis de decodificar. Um keyframe não depende de quadros anteriores, então o decodificador pode começar do zero a partir dele. Quando o stream do Simulator fica para trás, ele reinicia a partir de um keyframe recente ou solicita um novo, e pula os quadros perdidos. Os keyframes pertencem ao stream de vídeo comprimido, então são separados das imagens de tela que a Cognition copia na captura.

### Deixar o Devin operar apps nativos pela árvore de acessibilidade
Assim como os usuários, o Devin precisa ver e operar o app. Ele também precisa de uma forma eficiente de verificar o resultado de cada ação. O Devin faz isso por meio de computer use. Na sua forma mais simples, o computer use tira uma captura de tela, um modelo encontra um controle na imagem, clica nele e tira outra captura. Esse ciclo é lento e caro nos muitos caminhos de pausa, saída e reabertura que o Devin testa. Apps web são mais baratos de automatizar, porque o navegador expõe a estrutura da página, então o Devin pode encontrar um botão pelo nome ou ler o valor de um campo sem trabalhar a partir de pixels.

#### A árvore de acessibilidade
Apps nativos do macOS e apps no iOS Simulator rodam fora do navegador, mas expõem uma estrutura própria: a árvore de acessibilidade. Criada para tecnologias assistivas, a árvore descreve cada controle por seu papel, nome, valor, estado e relações com outros controles. A Cognition expõe essa árvore à ferramenta de computer use do Devin, para que ele possa confirmar que um controle é um botão Resume em vez de adivinhar a partir dos pixels.

#### O ciclo observar, referenciar e agir
O Devin consulta um controle, age sobre a referência que a consulta devolve e então lê a árvore novamente para informar o que mudou. O exemplo do artigo, adaptado de um post anterior da Cognition, mostra uma consulta por um botão Resume e o toque que vem em seguida.

A consulta:

```json
{ "action": "query", "target": "ios", "role": "button", "name": "Resume" }
```

**O que isso faz:**
- `action: "query"` pede controles, não uma captura de tela. O agente lê a estrutura.
- `target: "ios"` seleciona a árvore do Simulator, e não a área de trabalho macOS.
- `role` e `name` restringem o resultado a um botão com o nome de acessibilidade "Resume". O agente diz o que quer, não onde estão os pixels.

O resultado resumido lista uma referência, o papel do controle, seu nome de acessibilidade e as ações que ele suporta:

```text
@i17 button "Resume" {press}
```

**O que isso faz:**
- `@i17` é uma referência que a próxima ação pode usar, então o agente não precisa identificar o controle de novo.
- `button "Resume"` confirma o papel e o nome de acessibilidade em uma linha.
- `{press}` lista a ação suportada, então o agente sabe o que pode fazer com esse controle.

Usando essa referência, o Devin toca no controle:

```json
{ "action": "act", "target": "ios", "reference": "@i17", "operation": "press" }
```

**O que isso faz:**
- `action: "act"` executa uma operação sobre uma referência existente, em vez de localizar o controle de novo.
- `reference: "@i17"` vincula o toque ao controle exato da consulta.
- `operation: "press"` é a mesma ação que o resultado anunciou, então o agente não adivinha as operações disponíveis.

A Cognition construiu essas capacidades sobre o projeto de código aberto accessibility-cli, da Dioxus, que agora faz parte da Cognition. O projeto oferece blocos de acessibilidade, entrada e captura de tela, e o Devin os transforma no ciclo observar, referenciar e agir.

#### Os pixels ainda importam
Mesmo com a árvore de acessibilidade, o Devin depende de capturas de tela para verificar a aparência do app. No exemplo do jogo, um menu de pausa fechado não prova que o jogo restaurou a posição do jogador, porque o app pode desenhar o gameplay diretamente, em vez de descrevê-lo na árvore.

> ❌ **Anti-pattern — confiar apenas na árvore:** tratar o estado de um controle na árvore de acessibilidade como prova do estado inteiro do app. **Por que as pessoas fazem isso:** a árvore é estruturada e barata de ler. **Consequência:** o agente relata sucesso para um estado que o app nunca restaurou, como um jogo desenhado diretamente na tela. **Correção:** confirmar estados importantes com uma verificação visual, além da árvore.

## Resultados
A Cognition lançou o suporte a macOS no Devin em 15 de setembro de 2026. Em seis semanas, quase 1.000 organizações estavam rodando o Devin no macOS. O Devin não precisa mais do sistema de CI de um cliente nem de computadores Mac para construir e testar apps da Apple, porque ele constrói, executa e verifica apps para iPhone, iPad e Mac em seu próprio workspace de nuvem. A partir desse workspace, o Devin também envia builds ao TestFlight, o serviço de testes beta da Apple.

### Tempo de inicialização
A Cognition inicia uma VM macOS nova para cada sessão do Devin em menos de um minuto, usando uma frota de hosts EC2 Mac em seis Regiões da AWS. Pela estimativa da Cognition, esse tempo de inicialização coloca os clientes em um ambiente macOS funcional de 15 a 20 vezes mais rápido do que configurar um host Mac por conta própria.

### Resultados dos clientes
Evan Almloff, Member of Technical Staff da Cognition, diz que o EC2 Mac deu ao Devin ambientes macOS na nuvem e destravou o desenvolvimento para iOS e macOS. A frota da Cognition usa instâncias mac-m4pro.metal e mac-m4.metal em seis Regiões, e consegue criar VMs macOS novas para o Devin em menos de um minuto.

A Wispr, que produz o app de ditado por voz Wispr Flow, usa o Devin para macOS para testar seu app iOS de ponta a ponta. Seu Lead iOS Engineer, Sumit Oberoi, diz que os testes de gravação e transcrição eram o maior bloqueio, e que o suporte do Devin ao macOS reduziu em 70% o tempo de revisão de código e melhorou a velocidade em quase 2x.

## Planeje sua própria frota de macOS no EC2 Mac
O artigo lista estes pontos da documentação do usuário do Amazon EC2 como fatores para o design de uma frota macOS.

> 📋 **Checklist — pontos de design da frota macOS**
> - [ ] Cada Dedicated Host roda uma instância Mac, e você só pode liberar um host depois do seu período mínimo de alocação de 24 horas.
> - [ ] Compartilhe Dedicated Hosts com outras contas da AWS, ou dentro de uma organização no AWS Organizations, usando o compartilhamento de Dedicated Hosts entre contas.
> - [ ] Instâncias Mac estão disponíveis como On-Demand Instances, e um Savings Plan pode reduzir o custo delas.
> - [ ] Os tipos de instância Mac estão disponíveis em Regiões específicas, então verifique os tipos por Região antes de planejar a capacidade.

🔎 Contexto adicional: a regra "cada host roda uma instância Mac" é a razão pela qual o design de dois workspaces por host da Cognition depende de rodar VMs convidadas no host, e não de o host rodar uma instância Mac por sessão.

## 🧠 Modelos mentais
### Separe a vida da sessão da vida do host
**O modelo:** A unidade que você aluga (um host com mínimo de 24 horas) e a unidade que faz o trabalho (uma sessão de agente) rodam em relógios diferentes. A Cognition mantém os hosts alocados e move as sessões para dentro e para fora deles.
**Como aplicar:** Quando a sua unidade de cobrança ou alocação é maior do que a sua unidade de trabalho, agende o trabalho dentro da unidade que você já paga, e libere a unidade só quando o mínimo tiver passado.

### Persista o estado como snapshots, não como máquinas em execução
**O modelo:** Uma VM é descartável. O que importa é o disco, e o disco vive em snapshots que podem ser restaurados em segundos.
**Como aplicar:** Libere a computação ociosa assim que o estado estiver salvo. Faça com que retomar seja uma restauração, e não uma reconstrução.

### Estrutura antes de pixels
**O modelo:** Quando a plataforma expõe estrutura (a árvore de acessibilidade), um agente deve consultar e agir sobre referências nomeadas, e não sobre capturas de tela.
**Como aplicar:** Use capturas de tela para verificação, não para localizar controles. Mantenha uma verificação visual para estados que a estrutura não consegue mostrar.

### Descarte quadros obsoletos e ressincronize nos keyframes
**O modelo:** Uma visualização ao vivo só é útil quando está atual. Uma etapa lenta deve perder os quadros antigos, não atrasar os novos, e o decodificador se recupera a partir de um keyframe.
**Como aplicar:** Limite a fila entre as etapas, nunca reproduza um backlog, e faça com que o stream possa ser reiniciado a partir de um keyframe recente.

## 🔑 Principais conclusões
- A Cognition roda cada sessão macOS do Devin em uma VM nova, iniciada em menos de um minuto em hosts EC2 Mac em seis Regiões da AWS.
- Cada host EC2 Mac roda dois workspaces isolados do Devin por vez, limite definido pelo SLA do macOS, e sessões ociosas liberam suas VMs.
- Cerca de 90% das sessões liberam sua VM Mac quando ficam ociosas e retomam a partir de um snapshot em segundos.
- A Cognition escolheu um anexo de rede por file handle e seu próprio gateway Ethernet em user space, para que um único sistema seja dono da política do filtro de pacotes.
- As imagens dos convidados são preparadas no host antes do primeiro boot, então diálogos de permissão nunca bloqueiam o agente.
- O Devin opera apps nativos pela árvore de acessibilidade e usa capturas de tela apenas para verificar a aparência.

## 🏛️ Para o Kenji
*Esta é a sua interpretação, não a do autor.* Para um arquiteto, a parte útil é o modelo de capacidade: um mínimo de 24 horas na unidade bare-metal força você a empacotar sessões curtas de agentes em hosts de longa duração, e a camada de snapshots é o que torna esse empacotamento possível. A pergunta que isso levanta é como o modelo de custos da sua própria plataforma mudaria se a menor unidade de cobrança fosse bem maior do que a duração média de uma sessão.

## 📚 Plano de leitura
### Do artigo
- [Instâncias do Amazon EC2 Mac](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html) — o guia oficial do usuário para planejar e executar o EC2 Mac.
- [Bringing macOS to Devin](https://devin.ai/blog/devin-gets-a-mac) — o relato mais detalhado da própria Cognition sobre como construiu o Devin para macOS.
- [accessibility-cli](https://github.com/DioxusLabs/accessibility-cli) — o projeto de código aberto por trás do ciclo observar, referenciar e agir.
- [Compartilhamento de Dedicated Hosts do Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dh-sharing.html) — como compartilhar hosts entre contas e dentro de uma organização.
- [Tipos de instância por Região](https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-regions.html) — verifique a disponibilidade de instâncias Mac antes de planejar a capacidade.
- [Apple Virtualization framework](https://developer.apple.com/documentation/virtualization) — a API que executa os convidados macOS.
### Para aprofundar
- ScreenCaptureKit (Apple) — a API de captura por trás do trade-off do pool de buffers na visualização ao vivo.
- Protocolo Network Block Device (NBD) — o anexo de disco que permitiu à Cognition manter seu design de armazenamento existente.
- Virtual Network Computing (VNC) — o protocolo de área de trabalho remota por trás da visualização da área de trabalho.
- Keyframes de vídeo e recuperação do decodificador — por que um quadro perdido pode tornar os quadros seguintes indecodificáveis.
