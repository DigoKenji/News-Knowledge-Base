---
type: Article Summary
title: '15 milhões de servidores web em uma VM – smoltcp, DPDK, AF_XDP'
description: 'A memória de socket do kernel e o espaço de portas de 16 bits, não a CPU, limitam quantos servidores cabem em uma VM; a rede em userland remove a maior parte desse custo.'
resource: https://kapteko.com/15-million-webservers
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://kapteko.com/15-million-webservers
  title: 15M web servers on one VM – smoltcp, DPDK, AF_XDP
  author: Hacker News (100+ pts)
status: stable
source_url: https://kapteko.com/15-million-webservers
source: Hacker News (100+ pts)
source_type: blog
published: '2026-11-19'
domain: backend-systems
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d811a96f8f91ad36de359
lang: pt-br
translation_of: /notes/backend-systems/2026-11-19-15m-web-servers-on-one-vm-smoltcp-dpdk-af-xdp.md
---

## 🎯 Tese central
A memória por socket do kernel e o espaço de portas de 16 bits, não a CPU, limitam quantos servidores web uma VM consegue rodar, e mover a pilha TCP/IP e o escalonamento para o userland remove a maior parte desse custo.

## 🧭 Por que o autor escreveu
O autor abre com uma piada: se cada um dos 8 bilhões de pessoas tivesse um servidor web, a demanda cairia a zero e a cibersegurança estaria resolvida. O experimento real é saber se uma VM com 10 GB de RAM e um núcleo de CPU consegue hospedar 15 milhões de servidores web, e o artigo analisa cada limite que encontra. Se a tese estiver certa, a RAM por servidor passa a ser a restrição de projeto, e a pilha de rede passa para dentro da aplicação.

## Servidor web

O autor escolhe aprender a partir dos fundamentos "em vez de fazer vibe coding com tudo". Ele parte das poucas syscalls por trás de um servidor web e as escreve em C para que cada chamada fique explícita. Ele diz que qualquer biblioteca HTTP acaba se reduzindo a essas chamadas, com a ressalva "bem, sim, mas na verdade não", significando que a simplificação é apenas aproximada.

> ❌ **Anti-padrão — fazer vibe coding na infraestrutura:** Gerar código de rede a partir de prompts e executá-lo sem saber o que as chamadas fazem. **Por que as pessoas fazem isso:** As respostas são fluentes e, nas palavras do autor, é "fácil demais aceitar suas respostas sem pensar criticamente". **Consequência:** Limites como esgotamento de portas e falta de memória só aparecem quando o código roda em escala. **Correção:** Aprenda as poucas syscalls e teste cada premissa no sistema real, como o artigo faz.

### Syscall
Uma syscall é uma função executada pelo kernel do SO. O servidor do artigo usa cinco: `listen` pede ao kernel que aceite conexões em um socket, `accept` pega uma conexão, `recv` lê dados dela, `send` escreve dados nela, e `close` a encerra. O autor admite depois que esqueceu `socket`, que o loop também precisa. Toda biblioteca HTTP de nível mais alto, argumenta ele, é construída sobre essas chamadas.

> 🔎 Contexto adicional: um file descriptor é o pequeno inteiro que um processo usa para se referir a um socket ou arquivo aberto.

### Socket de escuta
A listagem em C cria um socket, chama `listen` com um backlog de 128 e depois fica em loop no `accept`. Para cada cliente, chama `recv`, envia uma resposta HTTP 200 fixa com corpo de dois bytes e fecha o socket do cliente. O autor estima que esse loop atende "cerca de 65k RPS".

> 🔎 Contexto adicional: o segundo argumento de `listen` é o backlog, o número de conexões pendentes que o kernel enfileira.

```c
int main(void)
{
    // We use port 0 which makes OS return any available port
    int server = socket(AF_INET, SOCK_STREAM, 0);
    if (listen(server, 128) < 0)
        return 1;
    const char reply[] = "HTTP/1.0 200 OK\r\nContent-Length: 2\r\n\r\nOK";
    for (int client; (client = accept(server, NULL, NULL)) >= 0;) {
        if (recv(client, NULL, 4096, MSG_TRUNC) > 0)
            send(client, reply, sizeof reply - 1, MSG_NOSIGNAL);
        close(client);
    }
}
```

**O que isso faz:**
- `socket(AF_INET, SOCK_STREAM, 0)` cria um socket de stream TCP IPv4, que é o listener.
- `listen(server, 128) < 0` retorna código de saída 1 se a escuta falhar.
- `reply` é uma resposta HTTP/1.0 completa; `sizeof reply - 1` remove o byte NUL final do tamanho enviado.
- `recv(client, NULL, 4096, MSG_TRUNC)` não passa buffer e apenas verifica se chegaram dados (valor de retorno maior que 0); os bytes da requisição são descartados.
- A listagem não tem chamada a `bind()`. O comentário diz que a porta 0 faz o SO escolher uma porta, e a próxima seção mostra o que essa escolha custa.

Agora o autor termina o artigo iniciando um processo por servidor em um loop de shell:

```bash
for i in $(seq 1 15_000_000); do
    echo $i
    ./server &;
done
```

Saída como impressa no artigo:

```text
...
...
28232
program exited with error 1
```

### Faixa de portas efêmeras
`listen` falhou porque o kernel ficou sem portas. O servidor usa `port=0`, então o kernel escolhe uma porta livre de `ip_local_port_range`, que é pequena por padrão:

```bash
sysctl net.ipv4.ip_local_port_range
```

Saída: `net.ipv4.ip_local_port_range = 32768    60999`

O autor explica por que tantas portas são puladas. A porta 0 era historicamente usada para conexões de saída: quando você chama `connect('example.com', 80)`, o Linux conecta para fora e então aloca uma porta local aleatória de `ip_local_port_range` para receber a resposta. As portas 1-1024 exigem root. As portas 1024-32768 são puladas para que portas alocadas dinamicamente não colidam com aplicações existentes, já que você não quer que `connect('example.com')` aloque a porta 3306, a porta do MySQL. As portas 60999-65535 ficam excluídas por razões históricas que remontam ao IP Masquerading. Ampliar a faixa para 1-65535 ainda fica longe de 15 milhões. Um servidor UDP e um TCP podem compartilhar uma porta, então o autor dobra esse número para cerca de 128 mil.

> 🔎 Contexto adicional: `sysctl` lê e define parâmetros do kernel, e o prefixo `net.ipv4` se refere a configurações de rede IPv4.

### Porta 0
O autor diz que a porta 0 é especial e que não é possível escutar nela, depois acrescenta entre parênteses que "na verdade, é possível", apontando para um post sobre o tema (veja o plano de leitura). Para o experimento, o ponto é que a porta 0 pede ao kernel que escolha uma porta, e isso é o que esgota a faixa.

### Tupla de conexão e o teto de 65k
Cada servidor de escuta é identificado por uma tupla `(protocolo, IP, porta)`. O limite de 65k vem do padrão TCP, onde a porta é um `u16`. O autor tenta `find '*.c' -exec sed -i 's/u16/u32/' \;` para ampliar o tipo no código-fonte do kernel, e observa que isso não ajudaria, porque clientes normais ainda não conseguiriam se conectar. Como a tupla inclui o IP, usar mais endereços IP também multiplica o número de listeners. Conectar 450 placas de rede (15M / 32k = 450) daria 450 IPs únicos.

```bash
find '*.c' -exec sed -i 's/u16/u32/' \;
```

> ❌ **Anti-padrão — comprar hardware para um limite de software:** "sempre que você tem um problema de software, basta jogar dinheiro no hardware", acrescentando placas de rede para resolver a contagem de portas. **Por que as pessoas fazem isso:** É mais rápido do que mudar o software e parece uma solução. **Consequência:** A aritmética do artigo exige 450 placas só para as portas, e o autor então descobre que a memória do kernel por socket é o próximo limite. **Correção:** Use AnyIP, que dá a um único processo muitos endereços a partir de uma única rota, em vez de placas físicas.

### AnyIP
O AnyIP permite que um host escute em uma sub-rede inteira. Muitos provedores de VPS e servidores dedicados entregam uma sub-rede IPv6 inteira, e um único comando de rota torna toda a `/64` local à máquina. Isso dá 18 quintilhões de endereços "(mais ou menos um)". A mesma abordagem funciona para IPv4, mas o autor estima que uma sub-rede com 15 milhões de endereços custaria cerca de 250 milhões de dólares, então o comando precisa ser ajustado ao seu ambiente.

Uma nota de um leitor, como o autor a relata, diz que a placa de rede é o único componente de hardware que não ficou 100 vezes mais caro no último ano. O autor pergunta se o problema pode ser resolvido em software, e o AnyIP é a resposta.

```bash
sudo ip -6 route add local 2001:db8:1234:5678::/64 dev lo
```

O servidor agora escolhe o n-ésimo IP da faixa:

```c
// Read IP index from CLI, shift base IP by it
int i = read_cli_param(argv[1]);
const uint32_t base_ip = 0x0a400001;   // ex. 10.64.0.1
addr.sin_addr.s_addr = htonl(first_ip + i);   // shift, ex. 10.64.0.2
```

**O que isso faz:**
- O índice `i` vem do primeiro argumento de linha de comando, então cada processo recebe um endereço diferente.
- `base_ip` é o endereço inicial, `10.64.0.1` no comentário.
- `htonl(first_ip + i)` converte a soma de ordem de host para ordem de rede de bytes antes de armazenar em `sin_addr`.
- A listagem nomeia `base_ip`, mas soma `first_ip`, como impresso no artigo; os dois nomes são inconsistentes.

O loop completo com um processo por endereço:

```bash
for i in $(seq 1 15_000_000); do
    echo $i
    ./server $i &;
done
```

Saída: `Out of memory`

### Um processo por servidor
O autor mede um servidor em cerca de 1,5 MB de RAM. Rodar 15 milhões deles exigiria cerca de 20 TB, e ele chama isso de "alguns bytes a mais que nosso orçamento de 10 GB".

> 🔎 Contexto adicional: 15 milhões vezes 1,5 MB é 22,5 TB, então os 20 TB do artigo são aproximados. A diferença para o orçamento de 10 GB é de cerca de 2.000 vezes, bem mais que "alguns bytes".

## Runtime

O autor observa que já havia escrito sobre a sobrecarga de rodar milhões de processos ("Running 10 million Codex CLIs on one server - LFI processes") e sobre um escalonador especializado em userland ("Userland Linux scheduler in Rust"). A resposta é um único processo que roda todos os convidados (guests) dentro de um runtime personalizado, da mesma forma que o V8 consegue rodar milhões de convidados em um só processo.

### Runtime personalizado e sandbox LFI
O runtime usa o LFI para sandboxing. O LFI oferece isolamento de memória e substitui cada syscall por uma chamada ao handler do host, para que o host implemente a E/S como quiser. Para rodar muitos convidados, o runtime carrega todos eles na memória do processo principal e faz a CPU alternar entre eles. Nas palavras do autor, "nos tornamos nosso próprio micro SO".

### Orçamento de memória por convidado
O autor pergunta quanta memória cada convidado pode ter: "Dez Grã-Bretanhas divididas por 15 milhões dão cerca de meia kilo-Grã-Bretanha. Não é muito."

> 🔎 Contexto adicional: 10 GB divididos por 15 milhões dá cerca de 667 bytes por convidado, se GB significar 10^9 bytes.

Só o estado necessário para suspender um convidado importa, então o autor revisa o loop de accept:

```c
for (int client; (client = accept(server, NULL, NULL)) >= 0;) {
    if (recv(client, NULL, 4096, MSG_TRUNC) > 0)
        send(client, reply, sizeof reply - 1, MSG_NOSIGNAL);
    close(client);
}
```

Ele tem duas variáveis de estado, `server` e `client`, ambas file descriptors. Um convidado suspenso precisa do seu endereço de instrução e desses dois descritores:

```rust
struct Context
{
    pc: u64,        // Instruction address to return to
    fd_server: i32, // server file descriptor
    fd_client: i32, // client file descriptor
}
```

**O que isso faz:**
- `pc` é o endereço da instrução onde o convidado retoma depois que o escalonador volta a ele.
- `fd_server` e `fd_client` são os dois file descriptors que o loop usa.
- O autor dimensiona isso em 16 bytes por convidado, mantendo deliberadamente os campos `u64` e `i32` completos como margem de segurança.
- Empacotamento de bits poderia reduzir isso para cerca de 8 bytes por convidado, segundo o autor.

Com 16 bytes por convidado, o autor diz que 15 milhões de convidados suspensos precisam de cerca de 220 MB.

> 🔎 Contexto adicional: 15 milhões vezes 16 bytes são 240 milhões de bytes, um pouco mais que os 220 MB declarados no artigo.

### Conexões ociosas e ativas
O autor pergunta a um LLM quanto ocupa uma conexão TCP. A resposta é que uma conexão ociosa ocupa zero bytes e uma ativa cerca de 5 kB. Ele monta o orçamento com base nisso: 14 milhões de conexões ociosas a zero bytes, 220 MB de estado de escalonamento para 15 milhões de convidados, e 1 milhão de conexões ativas a 5 kB, o que dá 5 GB. Isso é cerca de metade do orçamento de RAM, e ele diz que raramente supera as próprias expectativas.

Para validar, ele roda 14 milhões de listeners que não fazem nada:

```c
int main(void) {
    // Run 14M of idle connections - just listen and do nothing
    // SHOULD CONSUMER ZERO MEMORY!
    for (int i = 0; i < 14_000_000; i++) {
        int server = socket(AF_INET, SOCK_STREAM, 0);
        listen(server, 1)
    }
}
```

**O que isso faz:**
- Cada iteração do loop cria um socket e o coloca em estado de escuta com backlog de 1.
- O comentário declara a premissa testada, de que sockets ociosos não usam memória.
- A listagem é impressa sem o ponto e vírgula final após `listen(server, 1)`, como no artigo.
- O teste é a verificação do autor sobre a resposta do LLM, e ele falha.

```bash
./run_15m_idle_servers
dmesg | tail -n 1
```

Saída: `Out Of Memory`, e a última linha do log do kernel diz `'SHOULD CONSUMER ZERO MEMORY' - what are you, stupid?`. O autor observa que o kernel ainda tem comentários antigos do Linus.

> ❌ **Anti-padrão — confiar em uma estimativa não verificada:** Aceitar um número gerado, aqui "conexão ociosa custa zero", sem um teste. **Por que as pessoas fazem isso:** A resposta soa autoritativa e encaixa no plano. **Consequência:** A execução de 15 milhões fica sem memória, e o orçamento construído sobre a premissa de zero byte falha. **Correção:** Meça o custo da menor unidade, um listener ocioso, antes de dimensionar o projeto.

## Rede no kernel

O autor admite que esqueceu uma syscall no loop: `socket`. Este programa cria 10 milhões de sockets e fica sem memória em uma VM de 10 GB:

```c
int main(void)
{
    for (int i = 0; i < 10_000_000; ++i) {
        socket(AF_INET, SOCK_STREAM, 0);
    }
}
```

A chamada `socket` aloca cerca de 3300 bytes, esteja o socket ativo, em escuta ou ocioso. O autor diz que o kernel foi escrito quando a RAM era grande e barata.

### Memória do kernel por socket
O autor pergunta a uma IA para onde vai a memória:

| Struct | sizeof | Descrição |
|---|---|---|
| struct tcp_sock | 2240B | Estado TCP, filas, timers, números de sequência, controle de congestionamento, contabilidade de retransmissão e entrega |
| struct socket_alloc | 768B | Um struct socket de 128 bytes com wait queue embutida, mais um inode sockfs de 608 bytes e preenchimento |
| struct file | 184B | Operações de arquivo aberto, flags, credenciais, contagem de referências e o ponteiro de volta para o socket |
| struct dentry | 192B | O nome pseudo do sockfs, o ponteiro de inode e a contabilidade de busca e vida útil do VFS |
| Total | 3384B | |

O texto diz "cerca de 3300 bytes" e o total da tabela é 3384B. O autor conclui que o uso dessa memória pelo kernel não pode ser reduzido muito "sem reescrever metade do kernel". Ele esperava armazenar um `buffer: [u8; 3300]` no lugar.

### Camadas OSI e encapsulamento
O autor desce pelo modelo OSI: HTTP na camada de aplicação (L7), TCP na de transporte (L4), IP na de rede (L3), Ethernet na de enlace (L2) e a camada física (L1). Cada camada envolve os dados da camada acima. O pacote de exemplo do artigo mostra os campos que cada camada acrescenta:

| Camada | Campos mostrados no artigo |
|---|---|
| TCP (L4) | Porta de origem 50123, porta de destino 80, Seq 1001, ACK 7001, Flags ACK |
| IP (L3) | IP de origem 192.0.2.10, IP de destino 198.51.100.20, TTL 64, Protocolo 6 (TCP) |
| Ethernet (L2) | MAC de destino 02:00:00:00:00:fe, MAC de origem 02:00:00:00:00:01, EtherType 0x0800 (IPv4), FCS CRC |

A carga útil é `GET /`, envolvida em cada camada.

> 🔎 Contexto adicional: um endereço MAC é o endereço de hardware de uma interface de rede, usado no segmento Ethernet local.

### Camadas com e sem estado
O TCP/IP tem estado, mas tudo abaixo dele é sem estado. As camadas inferiores não conhecem nem se importam com a carga útil, então o autor argumenta que é possível enviar pacotes IP ou quadros Ethernet com dados binários feitos à mão. Você então reimplementa TCP e IP por conta própria. Desenvolvedores embarcados fizeram isso "milhões de vezes". O Miniweb, de Adam Dunkels, combina uma implementação TCP/IP personalizada com um servidor HTTP que juntos precisam de apenas 28 bytes. Opções menos extremas citadas no artigo são o gVisor, que reimplementa o SO inteiro incluindo a pilha de rede, o Seastar, um runtime com pilha de rede própria da equipe do ScyllaDB, o lwIP, uma implementação C popular de TCP/IP, e o F-Stack, que usa a pilha TCP/IP do FreeBSD.

### smoltcp
O autor escolhe Rust e o smoltcp, descrito assim:

> smoltcp é uma pilha TCP/IP independente, orientada a eventos, projetada para
> sistemas bare-metal e de tempo real. smoltcp não precisa de nenhuma alocação em heap

Adicionar a biblioteca é uma linha, mas o smoltcp "é essencialmente um parser inteligente e não consegue fazer I/O". Ele precisa de um dispositivo por baixo:

```bash
cargo add smoltcp
```

O próprio smoltcp oferece duas opções de dispositivo: TUN e TAP (abaixo). O DPDK é a terceira.

```rust
fn main() -> std::io::Result<()> {
    let (mut device, config) = match std::env::args().nth(1).as_deref() {
        Some("tun") => (
            TunTapInterface::new("smoltun", Medium::Ip)?,
            Config::new(HardwareAddress::Ip),
        ),
        Some("tap") => (
            TunTapInterface::new("smoltap", Medium::Ethernet)?,
            Config::new(EthernetAddress([0x02, 0, 0, 0, 0, 2]).into()),
        ),
        _ => panic!("pass tun or tap"),
    };
    let mut iface = Interface::new(config, &mut device, Instant::now());
    iface.update_ip_addrs(|addrs| {
        addrs
            .push(IpCidr::new(IpAddress::v4(192, 0, 2, 2), 24))
            .unwrap();
    });
    let mut sockets = SocketSet::new(Vec::new());
    // Main network loop
    loop {
        let now = Instant::now();
        iface.poll(now, &mut device, &mut sockets);
        wait(device.as_raw_fd(), iface.poll_delay(now, &sockets))?;
    }
}
```

**O que isso faz:**
- O primeiro argumento de linha de comando escolhe o dispositivo: `tun` para o nível IP, `tap` para o nível Ethernet; qualquer outro valor causa panic.
- O ramo TAP usa um endereço MAC fixo localmente administrado, `02:00:00:00:00:02`, porque quadros Ethernet precisam de um.
- `update_ip_addrs` dá à interface o endereço 192.0.2.2/24.
- O loop chama `poll` para mover pacotes pelo smoltcp e depois espera no file descriptor do dispositivo até que `poll_delay` do smoltcp indique que deve rodar de novo.
- O design é orientado a eventos: nenhuma thread espera em um socket, e a pilha só roda quando o dispositivo ou um timer precisa dela.

### TUN e TAP
TUN é uma interface de rede virtual fornecida pelo kernel que opera na camada IP. TAP opera uma camada abaixo, na Ethernet. Ambas são fáceis de usar com o smoltcp, que, segundo o artigo, as oferece prontas.

### DPDK
O DPDK permite que um programa se comunique com a placa de rede usando quadros Ethernet, contornando o kernel. O autor dá o custo de um caminho pelo kernel: uma syscall pode levar cerca de 500 ns, e copiar dados do kernel para o espaço do usuário pode levar mais 1000 ns, dependendo do tamanho do buffer. Empresas de high-frequency trading que saturam uma interface de 400 Gbps precisam processar até 600 milhões de pacotes por segundo, cerca de 1,7 ns por pacote. Com o DPDK, você escreve em um buffer mapeado em memória para enviar e lê de outro para receber, sem syscalls.

O DPDK também evita interrupções. Ele funciona com um busy loop que usa um núcleo inteiro da CPU, movendo dados nas duas direções:

```text
loop {
    send_from_app_memory_to_network();
    receive_from_network_to_app_memory();
}
```

**O que isso faz:**
- O pseudocódigo é o esboço do autor, não uma API real; o código real do driver está na biblioteca DPDK.
- Cada iteração envia dados de saída para a placa e puxa os dados de entrada dela.
- Como o loop nunca dorme, ele custa um núcleo inteiro, que é o preço da latência previsível.

> ❌ **Anti-padrão — vincular a única NIC ao DPDK via SSH:** Ativar o driver DPDK na interface de rede pela qual você está conectado. **Por que as pessoas fazem isso:** O artigo não diz; o DPDK é a opção mais rápida, então é tentador testar na máquina que você tem. **Consequência:** O driver tira o dispositivo do SO, então a interface desaparece do `ifconfig` e o acesso SSH é perdido. **Correção:** O autor sugere o AF_XDP, que mantém o kernel como dono do dispositivo para que o SSH continue funcionando, e alerta que o DPDK exige acesso exclusivo.

### Interrupções e livelock
Cada chegada de pacote dispara uma interrupção. A CPU para a aplicação, entra no kernel, processa a interrupção e retorna. Com uma taxa de pacotes muito alta, a aplicação é interrompida constantemente. O autor chama isso de livelock e diz que depurá-lo em produção é "um nível próprio de inferno". O DPDK evita interrupções por completo, ao custo descrito acima.

### AF_XDP
O subsistema AF_XDP do Linux faz algo parecido com menos perturbação. Ele contorna a pilha de rede do kernel e usa buffers mapeados em memória para E/S de zero cópia. O kernel continua dono do dispositivo, e as interrupções continuam acontecendo. Quando um pacote chega, o kernel executa um filtro eBPF simples que o envia direto ao buffer do espaço do usuário ou o processa normalmente, então o SSH continua funcionando. Um modo alternativo, `SO_PREFER_BUSY_POLL`, oferece um busy loop sem interrupções ao custo de um núcleo da CPU. O DPDK ainda é mais rápido porque contorna o kernel por completo, mas o AF_XDP fica muito próximo e é mais fácil de gerenciar.

> 🔎 Contexto adicional: eBPF é um recurso do kernel para executar pequenos programas verificados em pontos de hook, como a recepção de pacotes.

### Trait Device
Para dar suporte a DPDK e AF_XDP no smoltcp, você implementa a trait `Device` do smoltcp, que tem três métodos para controle total. Alternativamente, os crates existentes `rust-dpdk-net` (para DPDK) e `xsk-rs` (para AF_XDP) podem fazer o trabalho.

### Fluxo final
O design do autor, em ordem:
1. O runtime carrega os 15 milhões de convidados servidores web na sua memória.
2. O escalonador do runtime começa a executar os convidados um a um.
3. As syscalls dos convidados são interceptadas pelo runtime e substituídas pela pilha de rede smoltcp e seus adaptadores.
4. Todos os 15 milhões de convidados começam a escutar, cada um em seu próprio endereço.
5. O escalonador consulta os adaptadores smoltcp por dados de entrada, acorda um convidado, faz com que ele processe os dados e passa para o próximo.

### Resultado do benchmark
O gráfico de benchmark do artigo não está no texto extraído, então os números abaixo são os declarados no texto. A pilha de rede do kernel esgota a RAM em cerca de 2,5 milhões de servidores web, e as requisições por segundo caem à medida que mais servidores rodam. Com o smoltcp, a RAM cresce mais devagar: cerca de 620 bytes por servidor ocioso e cerca de 1 kilobyte por servidor ativo, chegando a 16 milhões de servidores. O DPDK é o mais rápido neste benchmark, com o AF_XDP logo atrás.

A conclusão do autor é que levar a pilha de rede para o userland dá controle total sobre RAM e desempenho, e que um runtime personalizado pode rodar convidados mais rápido. Ele faz a ressalva de que "simplesmente" é um exagero: uma prova de conceito é fácil, mas um runtime Linux compatível com syscalls 100% POSIX dá mais trabalho, "E isso é outro artigo".

## 🧠 Modelos mentais
### Nível de abstração errado
**O modelo:** Quando um problema não pode ser resolvido na camada atual, o artigo argumenta que você provavelmente está trabalhando no nível de abstração errado, e a correção é descer uma camada.
**Como aplicar:** Quando um limite de recurso resiste ao ajuste, liste as camadas abaixo do seu código e pergunte qual delas é dona desse recurso.

### Cada camada envolve a de cima
**O modelo:** Cada camada de protocolo adiciona seu próprio cabeçalho em volta dos dados da camada acima, então HTTP fica dentro de TCP, dentro de IP, dentro de Ethernet.
**Como aplicar:** Ao projetar um bypass, identifique quais cabeçalhos seu código precisa produzir e quais camadas inferiores você pode assumir.

### Seu próprio micro SO
**O modelo:** Um runtime que mantém muitos convidados em um processo e alterna entre eles age como um pequeno sistema operacional, e cada convidado suspenso precisa apenas do estado que seu loop de syscalls usa.
**Como aplicar:** Conte o estado que um loop realmente precisa antes de escolher entre processos, threads e escalonamento dentro do processo.

### Busy polling troca um núcleo por previsibilidade
**O modelo:** Um busy loop que consulta o dispositivo evita interrupções e livelock, mas ocupa um núcleo inteiro da CPU.
**Como aplicar:** Antes de escolher DPDK ou busy polling com AF_XDP, decida se o ganho de latência justifica um núcleo dedicado.

## 🔑 Principais conclusões
- O espaço de portas limita um único IP a cerca de 64 mil listeners, então o autor escala adicionando endereços (AnyIP), e não adicionando portas.
- Cada socket do kernel custa cerca de 3384 bytes antes de qualquer tráfego, então cerca de 10 milhões de sockets esgotam uma VM de 10 GB.
- Uma conexão ociosa não é gratuita: o teste do autor contradisse a resposta do LLM de que conexões ociosas custam zero byte.
- Um estado de escalonamento de 16 bytes por convidado (um endereço de instrução e dois file descriptors) faz 15 milhões de convidados suspensos custarem cerca de 220 MB no modelo do autor.
- O smoltcp no userland com um escalonador personalizado chegou a 16 milhões de servidores no benchmark do artigo, com cerca de 620 bytes por servidor ocioso, contra o esgotamento da pilha do kernel perto de 2,5 milhões.
- O DPDK é o bypass mais rápido, mas tira a NIC do SO; o AF_XDP mantém o kernel no comando e fica logo atrás.
- Uma prova de conceito é fácil de construir; um runtime compatível com POSIX é um projeto separado e mais difícil, segundo o autor.

## 🏛️ Para o Kenji
> Interpretação, não do autor: quando um limite de escala aparece em uma revisão de arquitetura, este artigo lembra de localizá-lo com precisão (portas, memória do kernel, interrupções ou o runtime) antes de escolher uma correção, e de avaliar o risco operacional de cada correção, como perder o acesso de gerenciamento quando uma NIC é assumida pelo DPDK. A pergunta que ele levanta para uma decisão de plataforma é: se uma equipe propõe um caminho de dados com bypass do kernel, como DPDK ou AF_XDP, qual ganho medido de latência ou memória justificaria abrir mão da visibilidade do kernel e de um fallback para o tráfego de gerenciamento?

## 📚 Plano de leitura
### A partir do artigo (seção de referências)
- [How we handle 15M websocket connections on a single server](https://medium.com/beyond-localhost/how-we-handle-15M-websocket-connections-on-a-single-server-8917c3952f59) — a fonte que o autor cita para a ideia de 15 milhões de conexões WebSocket.
- [The port that does not exist](https://acid.vegas/blog/the-port-that-does-not-exist/) — o link do autor sobre por que a porta 0 é especial e se é possível escutar nela.
- [DPDK documentation](https://doc.dpdk.org/guides/index.html) — o site principal da documentação do DPDK, que o autor chama de ótima visão geral.
- [AF_XDP overview](https://docs.kernel.org/networking/af_xdp.html) — a documentação do kernel sobre o subsistema AF_XDP.
- [Ten million web servers proof of concept](https://github.com/artemyarulin/AI10M/tree/main/poc/ten-million-webservers) — o código de prova de conceito do autor para o experimento.

### Para aprofundar
- Running 10 million Codex CLIs on one server - LFI processes — post anterior do autor sobre por que milhões de processos adicionam sobrecarga (só o título, sem link no artigo).
- Userland Linux scheduler in Rust — texto anterior do autor sobre um escalonador especializado em userland com RAM mínima por tarefa (só o título).
- smoltcp, rust-dpdk-net e xsk-rs — a pilha TCP/IP em Rust e os crates DPDK e AF_XDP citados no artigo.
- Miniweb, gVisor, Seastar, lwIP e F-Stack — as pilhas de rede e runtimes alternativos que o autor lista.
