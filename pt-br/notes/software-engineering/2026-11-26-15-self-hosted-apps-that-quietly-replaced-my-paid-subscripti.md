---
type: Article Summary
title: 15 apps self-hosted que substituíram discretamente minhas assinaturas pagas
description: Quinze apps self-hosted em uma única máquina pequena podem substituir a maioria das assinaturas pagas, mas só se você aceitar executar e fazer backup do servidor.
resource: https://medium.com/@sarathm09/15-self-hosted-apps-that-quietly-replaced-my-paid-subscriptions-f84d6d4fc712
tags:
- software-engineering
- virtualization-containers
- network-security
- encryption
- least-privilege
- attack-surface
- large-language-model
- inference-tradeoffs
- database-dbms
- finops
- availability-reliability
- infrastructure-as-code
- reverse-proxy
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://medium.com/@sarathm09/15-self-hosted-apps-that-quietly-replaced-my-paid-subscriptions-f84d6d4fc712
  title: 15 self-hosted apps that quietly replaced my paid subscriptions
  author: Sarath S
status: stable
source_url: https://medium.com/@sarathm09/15-self-hosted-apps-that-quietly-replaced-my-paid-subscriptions-f84d6d4fc712
source: Medium (Sarath S)
source_type: medium
published: '2026-10-18'
domain: software-engineering
concepts:
- virtualization-containers
- network-security
- encryption
- least-privilege
- attack-surface
- large-language-model
- inference-tradeoffs
- database-dbms
- finops
- availability-reliability
- infrastructure-as-code
- reverse-proxy
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d81899744e60653f6f4e6
lang: pt-br
translation_of: /notes/software-engineering/2026-11-26-15-self-hosted-apps-that-quietly-replaced-my-paid-subscripti.md
---

## 🎯 Tese central
Quinze substitutos self-hosted para assinaturas pagas podem rodar em um mini PC e reduzir cerca de \$125 por mês no preço de tabela, mas só se você aceitar a tarefa de executar e fazer backup do servidor por conta própria.

## 🧭 Por que o autor escreveu
Pequenas cobranças recorrentes de armazenamento, senhas, chat de IA e ferramentas semelhantes se acumulam sem que ninguém perceba; a lista deste autor soma \$125.57 por mês. Se a tese se sustentar, um servidor doméstico barato substitui a maior parte dessas contas e se paga em poucos meses, enquanto você assume o papel de administrador de sistemas.

## Abertura: as assinaturas e o plano
O autor soma os preços de tabela e chega a \$125.57 por mês. ChatGPT Plus e Zapier custam cerca de \$20 cada, e o Acrobat custa \$14.99; a maioria das demais custa entre \$1.65 e \$12 cada. Dois itens valem \$0: o Home Assistant substitui apps gratuitos, e o autor não conseguiu confirmar um preço nos EUA para o Evernote. Quatorze substitutos rodam em Docker em uma única caixinha, e o último roda com um único comando `bunx`. O autor usa o Pi-hole e criou o mdb (item 15); os outros treze foram verificados em relação às versões mais recentes e aos preços nos EUA em 1º de outubro de 2026. Os dados de hardware descrevem uma configuração representativa, não uma que o autor tenha medido.

### Pequenas cobranças recorrentes
Cada cobrança é pequena o bastante para ser ignorada, e é assim que elas se acumulam. A lista vai das contas mais familiares (fotos e arquivos) até ferramentas de trabalho de nicho. Cada item segue o mesmo padrão: o que o app pago faz por você, o substituto, a economia mensal, o comando para iniciá-lo e uma desvantagem.

## O que você precisa primeiro
### Um mini PC
Qualquer mini PC com Intel N100 ou N150 e 16GB de RAM dá conta da lista, segundo o autor, que precifica uma configuração na seção de contas. A máquina roda Ubuntu Server ou Debian.

### Docker e Compose
O Docker executa cada app como um contêiner, e o Docker Compose descreve um app com vários contêineres em um único arquivo YAML. Instale o Docker Engine pelo guia do Docker e depois execute `sudo usermod -aG docker $USER` e faça login de novo para que o Docker funcione sem sudo.

> 🔎 Contexto adicional: ser membro do grupo `docker` dá a um usuário controle sobre o daemon do Docker, o que equivale, na prática, a poder de root no host. Trate essa conta de acordo.

### Endereços estáveis e uma pasta de dados
Reserve o endereço IP da máquina nas configurações DHCP do roteador para que ele nunca mude; todas as interfaces web do artigo são acessadas em `http://<ip-do-servidor>:<porta>`. Mantenha todas as pastas do Compose e os dados dos apps em `/srv`, de modo que um único backup de `/srv` cubra a maior parte deles. Os volumes nomeados do Docker ficam em `/var/lib/docker/volumes`, por isso o backup da seção final inclui esse caminho.

### Acesso externo por meio de VPN
Acesse a máquina de fora da sua rede por meio de uma VPN como Tailscale ou WireGuard, e não abrindo portas no roteador.

> ❌ **Anti-padrão — encaminhar portas para a máquina:** abrir portas no roteador para que a internet alcance um app self-hosted diretamente. **Por que as pessoas fazem isso:** é o jeito mais rápido de fazer o acesso remoto funcionar. **Consequência:** cada serviço exposto vira um alvo na internet aberta. **Correção:** acesse a máquina por uma VPN, como recomenda o artigo.

## Fotos e arquivos
O autor começa pelo celular. As fotos enchem o armazenamento gratuito da nuvem em um ou dois anos, e documentos, calendários e contatos seguem as fotos para casa.

### 1. Immich (substitui o armazenamento do iCloud+ ou do Google One, \$9.99/mês)
O iCloud oferece 5GB gratuitos e o Google oferece 15GB. Alguns anos de fotos e vídeos ultrapassam os dois, então as pessoas pagam pelo plano de 2TB do iCloud+ ou do Google One, \$9.99 por mês em qualquer um dos casos, principalmente para que o rolo da câmera seja copiado automaticamente. O Immich é a alternativa self-hosted mais próxima do Google Fotos: o app do celular faz backup em segundo plano, e o app web oferece linha do tempo, reconhecimento facial, visualização em mapa e álbuns compartilhados.

```bash
mkdir -p /srv/immich-app && cd /srv/immich-app
wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
# edit .env: change DB_PASSWORD; if UPLOAD_LOCATION points at a separate photo drive, back that drive up too
docker compose up -d   # web UI on :2283
```

**O que isso faz:**
- `mkdir -p` e `cd` criam uma pasta em `/srv`, de modo que o arquivo do Compose e seus dados fiquem no caminho coberto pelo backup.
- As duas linhas `wget` baixam o arquivo oficial do Compose do projeto e o arquivo de ambiente de exemplo da release mais recente.
- O comentário manda trocar `DB_PASSWORD` no `.env` e fazer backup de um drive de fotos separado, caso `UPLOAD_LOCATION` aponte para um.
- `docker compose up -d` inicia os contêineres em segundo plano; a interface web escuta na porta 2283.

O Immich evolui rápido: a v3.2.4 saiu em 28 de setembro de 2026, e versões principais trazem mudanças que quebram compatibilidade. Leia as notas de release antes de cada atualização.

> ❌ **Anti-padrão — uma única cópia da biblioteca:** manter a única cópia da biblioteca de fotos em um só drive. **Por que as pessoas fazem isso:** é o padrão quando o servidor é o primeiro lugar para onde as fotos vão. **Consequência:** a falha de um drive perde a biblioteca inteira. **Correção:** mantenha uma segunda cópia em outro lugar, como recomenda o artigo.

### 2. Nextcloud (substitui Google Drive, Agenda e Contatos, \$1.99/mês)
O Google Drive guarda o contrato de aluguel e os documentos de identidade escaneados, e o Google mantém calendário e contatos sincronizados entre os dispositivos. Alguns anos de PDFs ultrapassam os 15GB gratuitos e passam para o plano de 100GB do Google One, a \$1.99 por mês. Se seus arquivos cabem no plano gratuito, este item não economiza nada, e se fotos e arquivos já dividem um plano de 2TB do Google One, conte apenas os \$9.99 do item 1.

O Nextcloud combina arquivos, calendário e contatos em um único servidor, e telefones e notebooks sincronizam com ele do mesmo jeito que sincronizam com o Google. O autor o escolhe em vez do Syncthing, que sincroniza pastas mas não tem calendário, contatos nem interface web de arquivos, e em vez do ownCloud, que passou a fazer parte da Kiteworks em 2023 e está substituindo seu servidor PHP por uma reescrita em Go; o Nextcloud mantém o servidor PHP e seu ecossistema de apps.

```bash
docker run --init --sig-proxy=false --name nextcloud-aio-mastercontainer --restart always \
  --publish 80:80 --publish 8080:8080 --publish 8443:8443 \
  --volume nextcloud_aio_mastercontainer:/mnt/docker-aio-config \
  --volume /var/run/docker.sock:/var/run/docker.sock:ro \
  ghcr.io/nextcloud-releases/all-in-one:latest
# setup UI on https://<server-ip>:8080
```

**O que isso faz:**
- O comando inicia apenas o "mastercontainer" do AIO, que depois instala e gerencia os contêineres do Nextcloud.
- `--restart always` o traz de volta após uma reinicialização.
- As três opções `--publish` abrem as portas 80, 8080 e 8443; a porta 8080 serve a interface de configuração.
- O volume nomeado mantém a configuração do AIO entre reinicializações.
- A montagem do socket do Docker permite que o contêiner controle outros contêineres no host.

> 🔎 Contexto adicional: o sufixo `:ro` no socket não restringe o que a API do Docker permite, por isso não torna seguro expor o socket.

O Nextcloud é o item mais pesado da lista, e suas atualizações principais são as mais trabalhosas. O AIO precisa de um domínio e de um certificado válido; o artigo aponta para o guia de instância local do AIO, que mostra como obter um domínio gratuito da deSEC, usar Tailscale ou o desafio DNS caso você queira acesso apenas pela LAN.

> ❌ **Anti-padrão — montar o socket do Docker:** dar a um contêiner de gerenciamento voltado para a web acesso de leitura a `/var/run/docker.sock`. **Por que as pessoas fazem isso:** o instalador precisa dele para gerenciar os outros contêineres. **Consequência:** o artigo alerta que isso dá ao contêiner controle de root sobre o host. **Correção:** mantenha a máquina fora da internet aberta e aceite o compromisso conscientemente.

## Papelada
Com os arquivos no servidor, vem a pilha de papel: cartas do imposto de renda, garantias, extratos e PDFs para assinar e devolver.

### 3. Paperless-ngx (substitui o plano pago do Evernote para papel escaneado, contado como \$0)
As pessoas pagam pelo Evernote para escanear papéis e pesquisar o texto depois, como em "onde está a garantia da geladeira?". A precificação do Evernote mostra preços regionais e o autor não conseguiu confirmar um valor nos EUA, então a conta considera este item como \$0. O Paperless-ngx observa uma pasta "consume": tudo que for colocado nela passa por OCR, fica pesquisável e recebe tags por remetente e tipo.

```bash
bash -c "$(curl -L https://raw.githubusercontent.com/paperless-ngx/paperless-ngx/main/install-paperless-ngx.sh)"
# asks a few setup questions; give it /srv/paperless as the install folder; web UI on :8000 by default
```

**O que isso faz:**
- `curl -L` baixa o script de instalação do projeto e segue redirecionamentos.
- `bash -c "$(...)"` executa o script imediatamente sem salvá-lo antes, então você está confiando naquela URL.
- O script faz perguntas de configuração; o comentário diz para responder `/srv/paperless` como pasta de instalação, o que mantém seus dados em `/srv`.
- A interface web escuta na porta 8000 por padrão.

O README do projeto diz que os documentos são armazenados em texto puro, então não o execute em um host em que você não confia.

### 4. Stirling-PDF (substitui o Acrobat Standard, \$14.99/mês)
O Acrobat Standard custa \$14.99 por mês em um plano anual ou \$24.99 mensal. O Stirling-PDF faz mesclagem, OCR e conversão de formatos no navegador, e os arquivos nunca saem da sua rede. É open-core, então alguns recursos empresariais são pagos, e a versão 3.0 mudou o suficiente para que guias antigos não correspondam mais.

```bash
docker run -d -p 8085:8080 --restart unless-stopped docker.stirlingpdf.com/stirlingtools/stirling-pdf   # web UI on :8085, since Nextcloud AIO has 8080
```

**O que isso faz:**
- `-d` executa o contêiner em segundo plano.
- `-p 8085:8080` mapeia a porta 8085 do host para a porta 8080 do app, evitando o conflito com o Nextcloud AIO.
- `--restart unless-stopped` o reinicia após uma reinicialização, a menos que você o tenha parado manualmente.
- A imagem vem do domínio de registro do próprio projeto.

## Mídia
Este grupo reúne filmes e audiolivros que você já possui, reproduzíveis em qualquer tela da casa.

### 5. Jellyfin (substitui o Plex Pass e algumas assinaturas de streaming, \$6.99/mês)
O Plex transforma uma pasta de filmes ripados ou vídeos caseiros em uma biblioteca no estilo streaming, mas assistir fora de casa é a parte paga: o Plex Pass custa \$6.99 por mês e o Remote Watch Pass custa \$2.99. A tabela usa o Plex Pass porque é o plano que permite que o chip gráfico do servidor faça transcodificação em tempo real para celulares e conexões lentas. O Jellyfin oferece a mesma biblioteca, com pôsteres e pontos de retomada, e não tem plano pago.

```bash
docker run -d --name jellyfin -p 8096:8096 \
  -v /srv/jellyfin/config:/config -v /srv/jellyfin/cache:/cache \
  -v /srv/media:/media --device /dev/dri:/dev/dri \
  --restart unless-stopped jellyfin/jellyfin   # /dev/dri lets the Intel iGPU transcode
# web UI on :8096
```

**O que isso faz:**
- `-p 8096:8096` expõe a interface web.
- As duas montagens em `/srv/jellyfin` mantêm configuração e cache na árvore de `/srv` coberta pelo backup.
- `-v /srv/media:/media` aponta a biblioteca para sua pasta de mídia.
- `--device /dev/dri:/dev/dri` passa a GPU integrada Intel para o contêiner, para que ela possa transcodificar.
- `--restart unless-stopped` o reinicia após uma reinicialização.

Depois que o contêiner inicia, ative a aceleração por hardware Intel QSV nas configurações de reprodução do Jellyfin. Os apps clientes são menos polidos que os do Plex, especialmente em algumas smart TVs, e a visualização remota exige uma VPN ou um reverse proxy.

### 6. Audiobookshelf (substitui o app do Audible, \$8.99/mês)
O app do Audible mantém o seu ponto de parada entre dispositivos, então você pode retomar um capítulo exatamente na mesma frase. O Audible Standard custa \$8.99 por mês. O Audiobookshelf serve seus próprios audiolivros e podcasts e sincroniza o progresso de reprodução do mesmo jeito por meio dos seus apps para celular.

```bash
docker run -d -p 13378:80 \
  -v /srv/audiobookshelf/config:/config -v /srv/audiobookshelf/metadata:/metadata \
  -v /srv/audiobooks:/audiobooks -v /srv/podcasts:/podcasts \
  --restart unless-stopped ghcr.io/advplyr/audiobookshelf:latest   # web UI on :13378
```

**O que isso faz:**
- `-p 13378:80` expõe a interface web na porta 13378 do host.
- As montagens de `/srv/audiobookshelf` mantêm configuração e metadados na árvore coberta pelo backup.
- `/srv/audiobooks` e `/srv/podcasts` guardam os arquivos que ele serve.
- `--restart unless-stopped` o reinicia após uma reinicialização.

A economia só é real se você comprar audiolivros sem DRM ou pegá-los emprestados, porque as compras no Audible ficam presas ao app do Audible.

## Casa e segurança
O Frigate alimenta o Home Assistant; os outros dois funcionam sozinhos.

### 7. Vaultwarden (substitui o Bitwarden Premium ou o 1Password, \$1.65/mês)
Um gerenciador de senhas guarda logins, preenche-os no celular e no navegador, e avisa sobre senhas vazadas. O Bitwarden Premium custa \$19.80 por ano, valor da tabela; o 1Password, a \$3.99 por mês, economiza mais. O Vaultwarden é um servidor leve que fala a API do Bitwarden, então você mantém os apps e extensões oficiais e os aponta para o seu próprio servidor.

```bash
docker run -d --name vaultwarden -e DOMAIN="https://vw.example.com" \
  -v /srv/vaultwarden:/data --restart unless-stopped -p 127.0.0.1:8222:80 vaultwarden/server:latest
# after creating your account: docker rm -f vaultwarden, then re-run with -e SIGNUPS_ALLOWED=false
```

**O que isso faz:**
- `-e DOMAIN` informa ao Vaultwarden o endereço HTTPS público sob o qual ele é servido.
- `-p 127.0.0.1:8222:80` vincula a porta somente ao localhost, então apenas um reverse proxy na mesma máquina consegue acessá-la.
- `-v /srv/vaultwarden:/data` mantém o banco do cofre na árvore coberta pelo backup.
- O comentário diz para executar de novo com `SIGNUPS_ALLOWED=false` depois de criar sua conta, o que fecha o cadastro.

O cofre se recusa a funcionar sem HTTPS, então um reverse proxy precisa ficar na frente dele. Um reverse proxy é um servidor que aceita requisições no nome público, cuida do HTTPS e as encaminha ao app interno.

```javascript
vw.example.com {
    reverse_proxy 127.0.0.1:8222
}
```

**O que isso faz:**
- O bloco de site nomeia o host público; o Caddy obtém e renova o certificado automaticamente, assim que o nome aponta para a máquina e a Let's Encrypt consegue alcançar as portas 80 e 443.
- `reverse_proxy 127.0.0.1:8222` encaminha cada requisição para a porta do Vaultwarden restrita ao localhost.
- A etapa de certificado do próprio Caddy exige que as portas 80 e 443 estejam acessíveis, o que normalmente significa encaminhá-las no roteador; a rota pela VPN evita isso, usando o desafio DNS.
- Recarregue o Caddy com `sudo systemctl reload caddy` depois de editar o arquivo.

Se o Nextcloud AIO também estiver rodando, os dois disputam as portas 80 e 443. O artigo aponta para o guia de reverse proxy do AIO, que move o AIO para `APACHE_PORT=11000` para que o Caddy possa ficar com as duas portas.

> ❌ **Anti-padrão — deixar o cadastro aberto:** rodar o cofre de senhas com o registro ainda habilitado. **Por que as pessoas fazem isso:** o fluxo de configuração padrão deixa isso ligado até você criar a primeira conta. **Consequência:** qualquer pessoa que consiga alcançar o servidor pode criar uma conta nele. **Correção:** execute o contêiner de novo com `SIGNUPS_ALLOWED=false`, como diz o artigo.

O projeto é não oficial e mantido por voluntários, e os backups são responsabilidade sua. Com \$1.65 por mês o dinheiro quase não importa; o ponto é ter um cofre em hardware que você mesmo possui, ao custo de configurar o HTTPS.

### 8. Pi-hole (substitui o NextDNS Pro, \$1.99/mês)
Apps e páginas web consultam dezenas de domínios de anúncios e rastreadores. Um filtro de DNS recusa essas consultas, então os anúncios nunca carregam em nenhum dispositivo, incluindo uma smart TV que não consegue rodar um bloqueador de anúncios. O NextDNS vende isso como serviço hospedado por \$1.99 por mês. O autor roda o Pi-hole na própria rede: ele responde ao DNS de todos os dispositivos sem nenhuma configuração individual, e o painel mostra qual dispositivo pediu o quê.

```yaml
# /srv/pihole/docker-compose.yml, then: docker compose up -d
services:
  pihole:
    image: pihole/pihole:latest
    ports: ["53:53/tcp", "53:53/udp", "8081:80/tcp"]
    environment:
      TZ: "America/Los_Angeles"
      FTLCONF_webserver_api_password: "change-me"
      FTLCONF_dns_listeningMode: "ALL"
    volumes: ["./etc-pihole:/etc/pihole"]
    restart: unless-stopped
```

**O que isso faz:**
- `ports` mapeia o DNS na porta 53, tanto TCP quanto UDP, para o host, e mapeia a interface web da porta 8081 do host para a porta 80 do contêiner.
- `TZ` define o fuso horário dos logs e agendamentos.
- `FTLCONF_webserver_api_password` define a senha do painel; `"change-me"` é um marcador que você deve substituir.
- `FTLCONF_dns_listeningMode` é uma opção do Pi-hole que o artigo não explica, então consulte a documentação do Pi-hole antes de alterá-la.
- O volume `./etc-pihole` mantém a configuração do Pi-hole na pasta `/srv/pihole`.

No Ubuntu, o systemd-resolved já ocupa a porta 53; a documentação do Pi-hole descreve a correção. Depois, aponte a configuração de DNS do roteador para a máquina. A limitação é que o bloqueio de DNS não consegue atingir anúncios servidos pelo mesmo domínio do conteúdo, por isso os anúncios do YouTube continuam tocando.

### 9. Home Assistant (substitui os apps de casa inteligente de cada marca, contado como \$0)
Cada marca de lâmpada, tomada e sensor vem com seu próprio app gratuito, e uma casa com itens de marcas diferentes acaba com vários apps e sem jeito de fazer o sensor de uma marca acionar a luz de outra. O Home Assistant reúne todas as marcas em um único painel e executa as automações localmente. Sua nuvem opcional, a Nabu Casa, custa \$6.50 por mês se você quiser acesso remoto fácil, mas a conta considera o Home Assistant como \$0.

```bash
docker run -d --name homeassistant --privileged --restart=unless-stopped \
  -e TZ=America/Los_Angeles -v /srv/homeassistant:/config \
  -v /run/dbus:/run/dbus:ro --network=host ghcr.io/home-assistant/home-assistant:stable
# web UI on :8123
```

**O que isso faz:**
- `-v /srv/homeassistant:/config` armazena a configuração na árvore coberta pelo backup.
- `-v /run/dbus:/run/dbus:ro` dá ao contêiner o socket D-Bus do host, somente leitura.
- `--privileged` concede amplo acesso ao host; o artigo não explica por que isso é necessário, então consulte os documentos de instalação do Home Assistant.
- `--network=host` compartilha a pilha de rede do host.

> 🔎 Contexto adicional: o modo de rede do host permite que o contêiner alcance diretamente os dispositivos da rede local, o que a descoberta de dispositivos normalmente exige.

A instalação via Docker não inclui a loja de add-ons do Home Assistant, então extras como o Frigate rodam como contêineres separados que você gerencia.

### 10. Frigate (substitui o Ring Protect ou o Google Home Premium, \$9.99/mês)
O Ring não guarda gravações sem um plano, e não consegue distinguir uma pessoa de um carro que passa. O Ring Protect Multi, \$9.99 por mês, adiciona clipes salvos e alertas de pessoa; o Google Home Premium, que substituiu o Nest Aware, começa em \$10. O Frigate é um NVR (gravador de vídeo em rede) local: ele grava suas câmeras e detecta pessoas, carros e animais na própria máquina, não envia vídeo para lugar nenhum e alimenta esses eventos no Home Assistant.

```yaml
# /srv/frigate/docker-compose.yml, then: docker compose up -d
services:
  frigate:
    image: ghcr.io/blakeblackshear/frigate:stable
    shm_size: "512mb"
    devices: ["/dev/dri/renderD128:/dev/dri/renderD128"]   # Intel iGPU for OpenVINO
    volumes: ["./config:/config", "./storage:/media/frigate"]
    ports: ["8971:8971"]   # https://<server-ip>:8971, self-signed cert; admin password is in docker compose logs
    restart: unless-stopped
```

**O que isso faz:**
- `shm_size: "512mb"` define a memória compartilhada do contêiner; o artigo não explica por que o Frigate precisa dela.
- `devices` passa o dispositivo de renderização Intel para o contêiner, para que a detecção possa usar a GPU integrada.
- Os volumes `./config` e `./storage` guardam configurações e gravações na pasta do Compose.
- A porta 8971 serve a interface web por HTTPS com um certificado autoassinado; a senha de administrador aparece nos logs do contêiner.

Passar a GPU integrada não basta por si só. Sem um bloco `detectors`, o Frigate usa o detector de CPU, então adicione isto em `./config/config.yml`:

```yaml
detectors:
  ov:
    type: openvino
    device: GPU
```

**O que isso faz:**
- `detectors` define os detectores de objetos que o Frigate executa.
- `ov` é um nome para este detector; `type: openvino` seleciona o runtime OpenVINO da Intel.
- `device: GPU` diz ao OpenVINO para rodar na GPU integrada, e não na CPU.
- Também é preciso um bloco de modelo para o detector; a seção OpenVINO da documentação de detectores do Frigate traz um para cada modelo, e cada câmera recebe sua própria entrada RTSP na documentação de configuração de câmeras.

> ❌ **Anti-padrão — depender do detector de CPU:** rodar o Frigate com o detector de CPU padrão. **Por que as pessoas fazem isso:** funciona sem configuração extra. **Consequência:** a documentação do Frigate diz que o detector de CPU não é recomendado para uso geral. **Correção:** use a GPU integrada Intel via OpenVINO, que todo N100 e N150 possui, ou adicione uma TPU Coral, uma placa Hailo ou uma GPU NVIDIA.

O Frigate lê um stream local de cada câmera, geralmente RTSP (o protocolo padrão que as câmeras usam para transmitir vídeo pela rede). As câmeras Ring não oferecem esse recurso. O projeto ring-mqtt pode retransmitir um stream ao vivo sob demanda, mas esses streams passam pela nuvem da Ring e não foram feitos para gravação NVR contínua. Sair da Ring significa comprar câmeras RTSP, e esse custo não está na tabela.

## Escrita, desenvolvimento e automação
O n8n se conecta a qualquer coisa que tenha uma API HTTP; Immich, Paperless-ngx e Home Assistant têm todos uma, então ele pode conectar os itens anteriores.

### 11. LanguageTool (substitui o Grammarly Pro, \$12/mês)
O Grammarly Pro verifica ortografia, gramática e tom em e-mails, documentos e no navegador, a \$12 por mês com cobrança anual ou \$30 mensal. Um servidor LanguageTool self-hosted faz a mesma sublinhação: aponte a extensão do navegador ou o app de desktop para o seu servidor e o texto permanece na sua rede.

```bash
docker run -d -p 8010:8010 --restart unless-stopped erikvl87/languagetool
```

**O que isso faz:**
- `-p 8010:8010` expõe o servidor na porta 8010 do host.
- `--restart unless-stopped` o reinicia após uma reinicialização.
- A imagem é uma imagem da comunidade, que é o caminho habitual descrito pelo artigo.

O servidor self-hosted é a versão básica. A documentação do servidor do LanguageTool diz que suas regras baseadas em IA "estão disponíveis apenas na nuvem", então você perde as verificações extras do LanguageTool Premium. Os dados de n-gramas que melhoram a precisão são um download de vários gigabytes à parte.

### 12. n8n (substitui o Zapier Professional, \$19.99/mês)
O Zapier conecta apps com regras do tipo "quando isto acontecer, faça aquilo", e o Professional custa \$19.99 por mês em plano anual para 750 tarefas. O n8n é um construtor visual de fluxos de trabalho com centenas de integrações e um nó de código para etapas personalizadas. Self-hosted, não tem limite de tarefas. O exemplo do artigo verifica a API do Paperless-ngx a cada hora em busca de novos documentos com a tag "bill" e envia uma notificação ao celular.

```bash
docker run -d --name n8n -p 5678:5678 --restart unless-stopped -v n8n_data:/home/node/.n8n n8nio/n8n
# web UI on :5678; over plain HTTP on the LAN, add -e N8N_SECURE_COOKIE=false or the login page refuses you
```

**O que isso faz:**
- `-p 5678:5678` expõe a interface web.
- `-v n8n_data:/home/node/.n8n` mantém os fluxos de trabalho e credenciais em um volume nomeado.
- `--restart unless-stopped` o reinicia após uma reinicialização.
- A flag `N8N_SECURE_COOKIE=false` do comentário é necessária apenas para acesso HTTP simples na LAN; sobre HTTPS o cookie de login funciona normalmente.

O n8n está sob a Sustainable Use License: gratuito para uso pessoal e interno, mas SSO e sincronização com Git exigem licença paga.

### 13. Open WebUI + Ollama (substitui o ChatGPT Plus para perguntas do dia a dia, \$20/mês)
O ChatGPT Plus custa \$20 por mês e oferece limites maiores e modelos mais fortes. Se você paga principalmente porque bate no limite gratuito com resumos, reescritas de e-mail e "explique este erro", um modelo local cobre essas tarefas sem limite. O Open WebUI é uma interface no estilo ChatGPT e o Ollama executa os modelos na sua máquina, então os chats e os arquivos enviados ficam locais.

```bash
docker run -d -p 3000:8080 -v ollama:/root/.ollama -v open-webui:/app/backend/data \
  --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
# web UI on :3000; the first account you create becomes the admin, so sign up right away
# it starts with no models: pull a small model such as qwen3:4b, or a newer one from the Ollama library, in the admin settings
```

**O que isso faz:**
- `-p 3000:8080` expõe a interface web na porta 3000 do host.
- `-v ollama:/root/.ollama` armazena os modelos baixados em um volume nomeado.
- `-v open-webui:/app/backend/data` armazena chats e contas.
- `--restart always` o reinicia após qualquer reinicialização.

> 🔎 Contexto adicional: a tag `:ollama` é a variante que empacota o Ollama junto com a interface, de modo que um único contêiner atende aos dois.

Um mini PC limita você a modelos pequenos, que são mais lentos que o ChatGPT e bem mais fracos em raciocínio difícil. Mantenha o plano pago se é para isso que você o usa.

> ❌ **Anti-padrão — deixar a primeira conta sem reivindicar:** instalar a interface e adiar o primeiro cadastro. **Por que as pessoas fazem isso:** a primeira tela de login parece uma etapa que pode esperar. **Consequência:** a primeira conta criada vira a de administrador, então quem alcançar a porta primeiro fica com esse papel. **Correção:** crie a conta de administrador logo após a instalação, como diz o artigo.

### 14. Umami (substitui o Plausible ou o PostHog, \$9/mês)
Um blog ou um site paralelo precisa de contagem de visitantes, páginas populares e origens de tráfego. O Google Analytics é gratuito, mas usa cookies, o que traz o banner de consentimento. O plano Starter do Plausible custa \$9 por mês e o Fathom começa em \$15; ambos não usam cookies e têm um painel de uma página. O Umami é a versão self-hosted dessa ideia: uma tag de script no site e um painel mostrando visitantes, páginas e origens.

```bash
mkdir -p /srv/umami && cd /srv/umami
curl -O https://raw.githubusercontent.com/umami-software/umami/master/docker-compose.yml
# edit it first: set APP_SECRET and TWO_FACTOR_ENCRYPTION_KEY (openssl rand -hex 32 for each),
# change POSTGRES_PASSWORD and the matching password in DATABASE_URL,
# and change "3000:3000" to "3001:3000", since Open WebUI has 3000
docker compose up -d   # web UI on :3001; log in as admin / umami, then change the password
```

**O que isso faz:**
- `mkdir` e `cd` colocam a stack em `/srv`, junto com o restante dos dados cobertos pelo backup.
- `curl -O` baixa o arquivo do Compose do projeto, que você edita antes de iniciar.
- Os comentários dizem para gerar `APP_SECRET` e `TWO_FACTOR_ENCRYPTION_KEY` com `openssl rand -hex 32`, e para trocar a senha do banco em dois lugares.
- A porta muda de 3000 para 3001 porque o Open WebUI já usa a 3000.
- `docker compose up -d` inicia a stack; o login padrão é `admin` / `umami`, que você troca imediatamente.

No painel, adicione seu site e cole o script de rastreamento que ele fornece na `<head>` do site. A desvantagem é que agora você executa e faz backup de um banco Postgres.

> ❌ **Anti-padrão — manter as credenciais padrão:** rodar o painel de analytics com o login padrão `admin` / `umami`. **Por que as pessoas fazem isso:** os padrões estão impressos no guia de configuração. **Consequência:** qualquer pessoa que conheça os padrões pode ler as análises e entrar. **Correção:** troque a senha imediatamente após o primeiro login.

### 15. mdb (substitui o Obsidian Publish para ler as próprias notas, \$8/mês)
Sincronizar notas em markdown é gratuito via git, iCloud Drive ou uma pasta do Nextcloud. A parte paga é ler as notas em outro lugar além do editor: o Obsidian Publish transforma uma pasta de notas em um site, a \$8 por mês com cobrança anual ou \$10 mensal. Esta troca só faz sentido se você usa o Publish para ler as próprias notas em outros dispositivos, e não para manter um site público. O autor criou o mdb, então considere esse item com isso em mente. O mdb é um explorador de arquivos web para uma pasta de markdown, com editor, pré-visualização renderizada, diagramas Mermaid, comentários de revisão inline e exportação para HTML e PDF.

```bash
bunx @sarathm09/mdb /srv/notes
```

**O que isso faz:**
- `bunx` executa o pacote pelo gerenciador de pacotes do Bun, sem instalação global.
- `/srv/notes` é a pasta a ser servida; o artigo sugere apontar para a pasta onde suas notas são sincronizadas.
- O comando imprime `Open in browser: http://localhost:<porta>`; a porta é aleatória e muda a cada execução.
- Bun e Node são ambos necessários: o bunx baixa o mdb, mas a CLI e seu pacote SQLite rodam sob o Node.

Execute dentro do tmux se quiser que ele sobreviva ao logout, já que ele fica em primeiro plano. Na primeira execução ele também grava um arquivo de comando do Claude Code em `~/.claude/commands/mdb-review.md` e informa isso no terminal.

> ❌ **Anti-padrão — um servidor de notas aberto:** rodar o mdb sem login em todas as interfaces de rede. **Por que as pessoas fazem isso:** funciona de imediato, sem configuração. **Consequência:** qualquer pessoa na rede pode ler e editar suas notas, e, se o Claude Code estiver instalado, iniciar uma revisão por IA que roda com a sua conta. **Correção:** rode-o apenas em uma LAN em que você confia e acesse-o de fora pela VPN.

## A conta
A lista soma \$125.57 por mês, ou \$1,506.84 por ano nos preços acima, e mais onde você paga mensalmente.

- **Hardware:** um Beelink EQ14 com Intel N150, 16GB de RAM e SSD de 500GB sai por \$385. Os drives para fotos e mídia são à parte e não estão incluídos. 16GB dão conta da maior parte da lista, embora reconhecimento facial do Immich, Frigate e um modelo local rodando ao mesmo tempo seja onde a coisa aperta.
- **Energia:** máquinas da classe N100 ficam em torno de 6 W ociosas, e o Frigate e o Ollama elevam a média. O autor estima 20 W sem medir, o que dá 14.4 kWh por mês, ou \$2.64 pela média residencial dos EUA de julho de 2026, de 18.31 centavos por kWh.
- **Retorno do investimento:** \$385 contra \$122.93 líquidos por mês dá cerca de três meses, sem contar os drives e as câmeras RTSP que substituem a Ring.

Ninguém paga pelos quinze. O número real é o subconjunto que você paga hoje. Por exemplo, iCloud+ 2TB, Bitwarden Premium e ChatGPT Plus somam \$31.64 por mês, e a máquina se paga em cerca de 13 meses depois da energia, desde que um pequeno modelo local cubra o que você usava o ChatGPT para fazer.

## Seu tempo e backups
A tabela deixa de fora o seu tempo. Agora você é o administrador de sistemas, e os backups são a parte que você não pode pular. Comece pelo Immich e pelo Pi-hole, já que nenhum dos dois precisa de domínio ou HTTPS, e configure os backups no mesmo fim de semana com restic e um drive USB montado em `/mnt/usb`.

```bash
sudo apt install restic
sudo sh -c 'umask 077; echo "pick-a-long-password" > /root/.restic-pass'   # keep a copy off this box too
sudo restic -r /mnt/usb/restic --password-file /root/.restic-pass init
```

**O que isso faz:**
- `apt install restic` instala a ferramenta de backup.
- `umask 077` torna o arquivo de senha legível apenas pelo root; o comentário diz para manter uma cópia fora desta máquina também.
- `restic ... init` cria um repositório de backup criptografado no drive USB.

Salve isto como `/usr/local/bin/backup.sh` e torne-o executável com `sudo chmod +x`:

```bash
#!/bin/sh
# stop every running container so no database is mid-write, back up, start the same ones again
running=$(docker ps -q)
[ -n "$running" ] && docker stop $running
restic -r /mnt/usb/restic --password-file /root/.restic-pass backup /srv /var/lib/docker/volumes \
  --exclude /srv/media --exclude /srv/frigate/storage --exclude /var/lib/docker/volumes/ollama   # movies, footage, models: big and replaceable
[ -n "$running" ] && docker start $running
```

**O que isso faz:**
- Registra os contêineres em execução, os para para que nenhum banco esteja no meio de uma escrita, faz o backup e inicia novamente os mesmos contêineres.
- O comando `backup` cobre `/srv` e os volumes do Docker, os dois caminhos que guardam os dados dos itens anteriores.
- As flags `--exclude` deixam de fora mídia, gravações do Frigate e modelos do Ollama, que são grandes e substituíveis.
- Como o script para todos os contêineres, o DNS do Pi-hole fica fora do ar até o backup terminar.

Execute `sudo crontab -e` e adicione `0 3 * * 0 /usr/local/bin/backup.sh` para rodar às 3h da manhã de todo domingo. Execute uma vez manualmente antes do primeiro domingo, já que o primeiro backup é o mais lento, e depois verifique com `sudo restic -r /mnt/usb/restic --password-file /root/.restic-pass snapshots`.

> ❌ **Anti-padrão — um único local para os backups:** manter o backup em um drive ao lado do servidor. **Por que as pessoas fazem isso:** é o drive que já está conectado. **Consequência:** um incêndio ou um roubo leva o servidor e o backup juntos. **Correção:** copie o repositório para outro local de tempos em tempos, como recomenda o artigo.

## 🧠 Modelos mentais
### O que você paga, depois a troca
**O modelo:** cada item precifica o produto pago pelo que ele faz por você (armazenamento, busca, compartilhamento, alertas) e depois o associa ao substituto aberto que faz o mesmo trabalho.
**Como aplicar:** antes de hospedar você mesmo, liste os recursos que de fato usa. Se um plano gratuito os cobre, a economia é zero, como mostra o item do Nextcloud.

### Conte o seu próprio tempo
**O modelo:** a tabela de custos mensais deixa de fora o tempo de administração que o self-hosting acrescenta: atualizações, backups, certificados e solução de problemas.
**Como aplicar:** antes de trocar um serviço, decida quem cuida das atualizações e dos backups dele. Comece por serviços que não precisam de domínio nem de HTTPS, como recomenda o artigo.

## 🔑 Principais conclusões
- Os 15 produtos pagos somam \$125.57 por mês nos preços de tabela dos EUA verificados em 1º de outubro de 2026, ou \$1,506.84 por ano.
- Um mini PC com Intel N150 e 16GB de RAM, listado a \$385, cobre a lista inteira na configuração do autor; drives e câmeras custam à parte.
- Uma troca só economiza dinheiro naquilo que você usa: o item do Nextcloud não economiza nada se seus arquivos cabem no plano gratuito.
- Acesse a máquina de fora por uma VPN, e não por portas encaminhadas, e coloque o Vaultwarden atrás de HTTPS.
- O Frigate precisa da GPU integrada Intel via OpenVINO, porque o detector de CPU não é recomendado para uso geral.
- Configure os backups primeiro: o script do autor para todos os contêineres, faz o backup e os reinicia, e o autor recomenda uma cópia fora do local.

## 🏛️ Para Kenji
> 🔎 Esta é a sua interpretação, não a do autor. Um arquiteto que pesa SaaS contra ferramentas internas self-hosted enfrenta a mesma troca: quem aplica os patches no serviço, quem guarda os backups e quanta exposição a rede carrega. A pergunta que isso levanta é quais desses serviços você deixaria carregar dados pelos quais sua organização responde, e quem ficaria de plantão quando a máquina falhasse.

## 📚 Plano de leitura
### Do artigo
- [blog.s10n.dev](http://blog.s10n.dev) — a publicação do autor, para mais posts na mesma série sobre self-hosting.
### Para ir além
- Notas de release do Immich — leia antes de cada atualização, porque versões principais trazem mudanças que quebram compatibilidade.
- Guia de instância local do Nextcloud All-in-One — para acesso apenas pela LAN com domínio, Tailscale ou o desafio DNS.
- Wiki do Vaultwarden, página de HTTPS — para as rotas de desafio DNS e de certificados privados.
- Documentação de detectores do Frigate, seção OpenVINO — para o bloco de modelo que cada detector precisa.
- Documentação do Pi-hole sobre systemd-resolved — para liberar a porta 53 no Ubuntu.
- Documentação do Restic — para manutenção do repositório e cópias fora do local.

## Conceitos
- [virtualization-containers](/pt-br/concepts/cloud-infra/virtualization-containers.md)
- [network-security](/pt-br/concepts/security/network-security.md)
- [encryption](/pt-br/concepts/security/encryption.md)
- [least-privilege](/pt-br/concepts/security/least-privilege.md)
- [attack-surface](/pt-br/concepts/security/attack-surface.md)
- [large-language-model](/pt-br/concepts/ai-ml/large-language-model.md)
- [inference-tradeoffs](/pt-br/concepts/ai-ml/inference-tradeoffs.md)
- [database-dbms](/pt-br/concepts/data/database-dbms.md)
- [finops](/pt-br/concepts/cloud-infra/finops.md)
- [availability-reliability](/pt-br/concepts/backend-systems/availability-reliability.md)
- [infrastructure-as-code](/pt-br/concepts/cloud-infra/infrastructure-as-code.md)
- [reverse-proxy](/pt-br/concepts/backend-systems/reverse-proxy.md)
