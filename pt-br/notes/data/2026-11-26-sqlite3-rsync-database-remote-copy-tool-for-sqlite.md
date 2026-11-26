---
type: Article Summary
title: 'sqlite3_rsync: ferramenta de cópia remota de bancos de dados para SQLite'
description: sqlite3_rsync copia ou sincroniza bancos SQLite via SSH, enviando apenas as páginas que diferem e evitando duplicatas locais completas.
resource: https://simonwillison.net/2026/Nov/26/sqlite3-rsync
tags:
- data
- database-dbms
- replication-partitioning
- delta-sync
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/26/sqlite3-rsync
  title: 'sqlite3_rsync: Database Remote-Copy Tool For SQLite'
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/26/sqlite3-rsync
source: Simon Willison
source_type: blog
published: '2026-11-26'
domain: data
concepts:
- database-dbms
- replication-partitioning
- delta-sync
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d8106bb78d3472d9b70c7
lang: pt-br
translation_of: /notes/data/2026-11-26-sqlite3-rsync-database-remote-copy-tool-for-sqlite.md
---

## 🎯 Tese central
O sqlite3_rsync pode copiar um snapshot de um banco de dados SQLite para outra máquina via SSH sem antes criar uma cópia local completa, e pode ressincronizar uma cópia existente enviando apenas o conteúdo que difere.

## 🧭 Por que o autor escreveu
Simon Willison conhecia o sqlite3_rsync como uma ferramenta experimental do SQLite, mas diz que só entendeu uma de suas propriedades mais atraentes neste dia. O problema de partida é que as formas usuais de fazer snapshot de um banco exigem espaço livre em disco igual ao de uma segunda cópia inteira, o que é doloroso em instâncias pequenas. Se a tese estiver certa, a restrição de disco deixa de obrigar a escolher entre um snapshot e uma cópia local completa.

## A ferramenta e sua história
O artigo descreve o sqlite3_rsync como uma ferramenta adicional do SQLite. Ela foi adicionada como "experimental" no [SQLite 3.47.0, em outubro de 2024](https://www.sqlite.org/releaselog/3_47_0.html) e aprimorada no [SQLite 3.50.0, em maio de 2025](https://www.sqlite.org/releaselog/3_50_0.html). A descoberta do próprio autor não foi o lançamento da ferramenta, mas uma de suas características, que ele só entendeu recentemente.

### sqlite3_rsync
A ferramenta leva o nome do rsync, o utilitário Unix que sincroniza arquivos transferindo apenas o que mudou. A afirmação do artigo é que a mesma ideia se aplica a um arquivo de banco SQLite, com dois usos: tirar um snapshot remoto de um banco ativo e atualizar, de forma barata, uma cópia remota existente. A ferramenta está documentada integralmente em [a página do sqlite3_rsync no sqlite.org](https://www.sqlite.org/rsync.html).
> 🔎 Contexto adicional: o rsync é uma ferramenta de sincronização de arquivos Unix de longa data. Ele compara origem e destino e envia apenas as diferenças, em vez de copiar tudo de novo.

## O problema do snapshot nos métodos existentes
Normalmente, diz o artigo, uma cópia snapshot de um banco SQLite é feita por um de dois métodos. Ambos exigem espaço suficiente na máquina que faz a cópia para conter uma duplicata completa do banco. Em uma instância pequena que já usa mais da metade do disco, segundo o autor, você está "simplesmente sem sorte".

### API de backup
O artigo linka [a API de backup do SQLite](https://www.sqlite.org/backup.html) como a primeira forma de fazer snapshot de um banco. O autor a trata como uma rota normal e suportada, e sua limitação é o espaço, não a correção.
> 🔎 Contexto adicional: a API de backup é a interface do SQLite para copiar um banco para outro, e funciona em um banco ativo. O artigo não descreve seus detalhes internos, então só se deve confiar na limitação de espaço em disco declarada no artigo.

### VACUUM INTO
O segundo método é o [VACUUM INTO](https://www.sqlite.org/lang_vacuum.html#vacuuminto), que grava uma cópia do banco em um novo arquivo. Assim como a API de backup, ele precisa de espaço para esse novo arquivo, portanto tem o mesmo custo de disco.
> 🔎 Contexto adicional: o VACUUM INTO é um comando SQL. Você o executa em um banco aberto e informa um caminho de arquivo, e o SQLite grava a cópia ali.

> ❌ **Anti-padrão — preparar uma cópia local completa antes de mover:** criar uma duplicata completa do banco na mesma máquina e depois transferir esse arquivo para outro lugar. **Por que as pessoas fazem isso:** as duas rotas de snapshot nativas (a API de backup e o VACUUM INTO) gravam um arquivo completo, então parece ser a única opção. **Consequência:** em uma instância pequena com pouco disco livre, a cópia simplesmente não pode ser feita, como o artigo alerta. **Correção:** transmitir o snapshot diretamente ao destino via SSH com o sqlite3_rsync, de modo que nenhuma duplicata local completa seja necessária.
> 🔎 Contexto adicional: o artigo não rotula isso como anti-padrão. A classificação é inferida da restrição de espaço em disco descrita pelo autor.

## Snapshot via SSH sem cópia local
A principal afirmação do autor é que o sqlite3_rsync "pode copiar um snapshot do seu banco de dados por uma conexão SSH para outra máquina, sem precisar primeiro criar uma cópia local completa". O valor fica mais claro em hosts com recursos limitados, nos quais a máquina de origem não pode sequer manter por um instante uma segunda cópia do banco.

### Snapshot remoto via SSH
Um snapshot remoto, aqui, significa uma cópia consistente do banco, tirada em um único instante e entregue a outra máquina. O SSH (Secure Shell) é o canal criptografado entre as duas máquinas que o transporta.
> 🔎 Contexto adicional: o SSH é um protocolo criptografado para executar comandos e mover dados entre computadores por uma rede. O artigo só diz que a conexão é SSH e não descreve suas propriedades de segurança além disso.

## Sincronização incremental de uma cópia existente
O segundo uso é manter atualizada uma cópia que já existe na outra máquina. O artigo diz que isso "funciona de modo semelhante ao rsync comum" e apresenta a ideia central do protocolo em um bloco de citação:

> A ideia central por trás do protocolo é que a réplica envia hashes criptográficos de páginas ou grupos de páginas para o lado de origem. A origem devolve o conteúdo das páginas que diferem, ou solicita hashes de granularidade mais fina se um hash de múltiplas páginas não coincidir. Se a origem e a réplica começarem muito diferentes, a largura de banda total da rede pode exceder o tamanho do banco inteiro, devido ao custo de trocar hashes, mas o excedente não é grande: no máximo alguns por cento.

### Sincronização delta baseada em hash
Sincronização delta significa transferir apenas as partes dos dados que diferem entre duas cópias. Aqui, a réplica (a cópia de destino) calcula hashes sobre páginas, que são as unidades de armazenamento de tamanho fixo do arquivo do banco. Um hash é uma impressão digital curta do conteúdo de um bloco, e qualquer alteração no bloco muda essa impressão. A réplica envia essas impressões para a origem, que as compara com as suas e responde com o conteúdo das páginas que diferem. Quando o hash que cobre um grupo de páginas não coincide, o protocolo pede hashes mais finos desse grupo, estreitando a busca até que apenas as páginas alteradas sejam enviadas.
> 🔎 Contexto adicional: "hash criptográfico", aqui, significa uma impressão digital praticamente impossível de falsificar ou colidir. Sua força importa menos para o argumento de largura de banda do que o fato de ser compacta e mudar sempre que o conteúdo muda.

Os números do artigo são a parte importante. Quando as duas cópias são quase idênticas, o tráfego é apenas os hashes mais as poucas páginas alteradas. Quando começam muito diferentes, a troca de hashes em si adiciona overhead, e o total pode exceder o tamanho do banco. O artigo argumenta que esse excedente é pequeno, de no máximo alguns por cento, de modo que o protocolo nunca é um mau negócio no pior caso que descreve.

> ❌ **Anti-padrão — reenviar o banco inteiro para atualizar uma réplica:** copiar todo o arquivo do banco pela rede sempre que a cópia remota precisa de atualização. **Por que as pessoas fazem isso:** é a abordagem mais simples e não exige conhecimento do protocolo. **Consequência:** o tráfego de rede e o tempo de transferência crescem com o tamanho total do banco, mesmo quando apenas algumas páginas mudaram. **Correção:** deixar a réplica enviar hashes de páginas e transferir apenas as páginas que diferem, como faz o sqlite3_rsync.
> 🔎 Contexto adicional: o artigo não rotula isso como anti-padrão. A classificação é inferida da afirmação do autor de que o sqlite3_rsync evita retransmitir o banco inteiro.

## Atualização: a cobertura anterior do autor
O autor acrescenta que já havia escrito sobre esse uso de sincronização em outubro de 2024, quando a ferramenta foi lançada. O [post anterior](https://simonwillison.net/2024/Oct/4/sqlite-rsync/) é a referência para essa discussão anterior. O artigo não diz o que o post continha além do fato de ter tratado dessa aplicação do sqlite_rsync, então nenhum detalhe adicional é dado aqui.

## 🧠 Modelos mentais
### Comparar impressões digitais, depois aprofundar
**O modelo:** duas cópias primeiro trocam hashes compactos de grupos grossos de dados. Somente os grupos cujos hashes divergem são examinados mais a fundo, até páginas individuais, de modo que o trabalho e o tráfego seguem as diferenças e não o tamanho total.
**Como aplicar:** quando você precisa saber o que difere entre dois grandes armazenamentos, compare resumos primeiro e desça apenas nas partes que divergem.

### Transmitir em vez de preparar
**O modelo:** mover um snapshot pela rede enquanto ele é produzido, em vez de construir uma cópia intermediária completa na origem e depois mover essa cópia.
**Como aplicar:** quando a folga de disco é apertada, verifique se uma ferramenta de transferência pode ler a origem e gravar diretamente no destino antes de presumir que é preciso uma cópia de preparação.

## 🔑 Principais conclusões
- O sqlite3_rsync pode tirar um snapshot de um banco via SSH sem criar antes uma cópia local completa, onde a API de backup e o VACUUM INTO precisam de espaço para uma duplicata completa.
- Sincronizar uma cópia existente evita retransmitir o banco inteiro, porque a origem devolve apenas o conteúdo das páginas que diferem.
- O protocolo troca hashes de páginas ou grupos de páginas e pede hashes de granularidade mais fina quando o hash de um grupo não coincide.
- Quando duas cópias começam muito diferentes, a largura de banda total da rede pode exceder o tamanho do banco inteiro, por causa do overhead da troca de hashes.
- O artigo coloca o excedente de largura de banda além do tamanho do banco em no máximo alguns por cento.
- A ferramenta foi adicionada como experimental no SQLite 3.47.0 (outubro de 2024) e aprimorada no SQLite 3.50.0 (maio de 2025).
- O autor já havia escrito sobre esse uso de sincronização em outubro de 2024, quando a ferramenta foi lançada.

## 🏛️ Para o Kenji
> 🔎 Esta é a sua interpretação, não a do autor. No trabalho de arquitetura, isso é relevante para backup, recuperação de desastres e atualização de réplicas de arquivos SQLite em pequenos serviços ou nós de borda, onde uma segunda cópia completa pode não caber no disco. Uma pergunta que levanta: quanto custa a sincronização delta para um banco que recebe escritas contínuas, em que as mudanças atingem muitas páginas e as cópias frequentemente se afastam muito?

## 📚 Plano de leitura
### Do artigo
- [sqlite3_rsync: ferramenta de cópia remota de bancos de dados para SQLite](https://www.sqlite.org/rsync.html) — a documentação oficial da própria ferramenta.
- [Notas de versão do SQLite 3.47.0](https://www.sqlite.org/releaselog/3_47_0.html) — onde o sqlite3_rsync foi adicionado como experimental.
- [Notas de versão do SQLite 3.50.0](https://www.sqlite.org/releaselog/3_50_0.html) — os aprimoramentos de maio de 2025.
- [API de backup do SQLite](https://www.sqlite.org/backup.html) — o primeiro método de snapshot com que o artigo faz comparação.
- [VACUUM INTO](https://www.sqlite.org/lang_vacuum.html#vacuuminto) — o segundo método de snapshot, que grava uma cópia em um novo arquivo.
- [Post de outubro de 2024 de Simon Willison sobre o sqlite-rsync](https://simonwillison.net/2024/Oct/4/sqlite-rsync/) — a cobertura anterior do autor sobre o uso de sincronização.

### Para aprofundar
- O algoritmo de transferência delta do rsync — a ideia no nível de arquivo que o sqlite3_rsync adapta para páginas de banco.
- Árvores de hash (árvores de Merkle) — a estrutura geral por trás da comparação de dados por meio de hashes hierárquicos.
- Estratégias de snapshot para bancos embarcados — quando preparar uma cópia e quando transmiti-la.

## Conceitos
- [database-dbms](/pt-br/concepts/data/database-dbms.md)
- [replication-partitioning](/pt-br/concepts/backend-systems/replication-partitioning.md)
- [delta-sync](/pt-br/concepts/data/delta-sync.md)
