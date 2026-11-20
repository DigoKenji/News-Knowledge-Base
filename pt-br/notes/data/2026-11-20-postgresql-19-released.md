---
type: Article Summary
title: PostgreSQL 19 lançado
description: O PostgreSQL 19 permite que clientes em um standby aguardem uma posição conhecida do WAL, oferecendo consistência de leitura das próprias escritas sem encaminhar todas as leituras ao primário.
resource: https://simonwillison.net/2026/Nov/19/postgresql-19/
tags:
- data
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/19/postgresql-19/
  title: PostgreSQL 19 Released
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/19/postgresql-19/
source: Simon Willison
source_type: blog
published: '2026-11-19'
domain: data
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d814a8f2dede89040e2df
lang: pt-br
translation_of: /notes/data/2026-11-20-postgresql-19-released.md
---

## 🎯 Tese central
O PostgreSQL 19 permite que uma aplicação leia as próprias escritas recentes a partir de um standby, fazendo com que o standby aguarde até ter reproduzido o write-ahead log (WAL) do primário até um número de sequência de log (LSN) específico, usando o novo comando `WAIT FOR LSN`.

## 🧭 Por que o autor escreveu
O tráfego de leitura costuma ser enviado para réplicas, mas uma réplica pode ficar atrás do primário, então um usuário pode não ver uma mudança que acabou de fazer. O ponto de Simon Willison é que o PostgreSQL agora oferece uma resposta direta para esse problema, e que um padrão antigo do MediaWiki, da Wikipédia, tem um equivalente limpo no PostgreSQL. Se a tese se sustenta, as aplicações podem continuar escalando leituras em standbys sem abrir mão da garantia de "vejo a minha própria edição".

## O lançamento e o atraso
O artigo é um link post. Ele informa que o PostgreSQL 19 foi lançado em 19 de novembro de 2026 (com link para o anúncio oficial de lançamento, também discutido no Lobsters). Na maioria dos anos, o PostgreSQL é lançado em setembro. Desta vez, o lançamento chegou dois meses depois, o que o autor atribui principalmente ao volume de problemas que a equipe do PostgreSQL precisou tratar durante o que ele chama de "apocalipse de vulnerabilidades reportadas por IA". Um outro post de blog, no rhaas.blogspot.com, traz mais detalhes sobre o atraso. O autor diz que o lançamento "valeu totalmente a espera" e aponta este recurso como o seu favorito.

> 🔎 Contexto adicionado: Um standby é uma réplica que aplica continuamente as mudanças vindas do primário. O atraso do lançamento em si não é um tema técnico no artigo; o restante da nota se concentra no recurso.

## Leitura consistente nos standbys com WAIT FOR LSN
O ponto central do autor é que o PostgreSQL 19 acrescenta consistência de "leitura das próprias escritas" para aplicações que enviam leituras a standbys. O novo comando `WAIT FOR LSN` é o mecanismo. Uma sessão em um standby pode executar `WAIT FOR LSN` para pausar até que o standby tenha reproduzido o write-ahead log (WAL) até um determinado número de sequência de log (LSN). Só depois desse ponto a sessão executa a consulta. O resultado é que a aplicação consegue ler dados que acabou de escrever no primário, mesmo que a leitura aconteça em uma réplica.

> 🔎 Contexto adicionado: Leitura das próprias escritas (read-your-writes) é uma garantia de sessão: um cliente sempre vê os efeitos de suas próprias escritas anteriores. O WAL é o log ordenado de todas as mudanças; um LSN é uma posição nesse log, então esperar por um LSN significa esperar por um ponto preciso do histórico de mudanças, e não por um momento aproximado.

## Usando WAIT FOR LSN em uma aplicação
O artigo mostra o padrão em dois passos. Primeiro, depois de uma escrita no primário, a aplicação captura a posição atual de inserção no WAL. O exemplo faz uma atualização e depois pede a posição:

```sql
UPDATE movie SET genre = 'Dramatic' WHERE genre = 'Drama';
SELECT pg_current_wal_insert_lsn();
```

O autor observa que isso devolve um valor como `0/0306EE20`, e sugere guardá-lo em algum lugar que a próxima requisição consiga acessar, como um cookie assinado. Segundo, em uma requisição posterior, a aplicação aguarda no standby antes de ler:

```sql
WAIT FOR LSN '0/0306EE20' WITH (TIMEOUT '0.1s');
SELECT * FROM movie WHERE genre = 'Drama';
```

**O que isso faz:**
- A primeira instrução altera os dados no primário e captura a posição do WAL que identifica essa mudança, então a posição é conhecida antes da leitura.
- O LSN é passado com a requisição seguinte, em vez de ser mantido no servidor, e por isso o autor sugere um cookie assinado. O token acompanha o cliente.
- `WAIT FOR LSN` bloqueia a sessão do standby até que a reprodução alcance a posição guardada. A consulta então roda sobre um estado que inclui a escrita.
- `WITH (TIMEOUT '0.1s')` limita a espera. O timeout é opcional. Sem ele, a espera é indefinida. Com ele, segundo o autor, você recebe um erro se a réplica não alcançar a posição a tempo, então a aplicação precisa decidir o que fazer nesse caso.

> 🔎 Contexto adicionado: O artigo não diz o que a aplicação deve fazer depois do erro de timeout. Uma escolha comum é recorrer ao primário, mas isso é uma decisão de design da aplicação, e não algo que o artigo especifique.

## Referência anterior: a espera de replicação do MediaWiki
O autor conheceu esse padrão primeiro no software da Wikipédia. O MediaWiki tem uma versão feita à mão do padrão, sobre a replicação do MySQL. Sua garantia diz respeito à ordem cronológica: um usuário deve ver os eventos da wiki na ordem certa entre requisições. Um atraso de alguns segundos é aceitável, desde que as requisições seguintes mostrem um quadro consistente. O mecanismo guarda a posição do binlog do primário na sessão do usuário. No início de cada requisição, a aplicação espera que a réplica alcance essa posição guardada antes de fazer qualquer leitura nela.

O autor linka o código do MediaWiki, que agora usa `WAIT_FOR_EXECUTED_GTID_SET()` do MySQL e recorre a `SOURCE_POS_WAIT()` em instalações mais antigas. A estrutura é a mesma do comando do PostgreSQL: guardar uma posição na escrita, esperar que uma réplica a alcance antes de ler. O PostgreSQL 19 traz essa estrutura para dentro do próprio banco, em forma de comando.

> 🔎 Contexto adicionado: A posição do binlog do MySQL e os conjuntos GTID, assim como o LSN do PostgreSQL, nomeiam um ponto no fluxo de replicação. O formato da solução é o mesmo, embora os detalhes sejam diferentes.

## 🧠 Modelos mentais
### Capture uma posição na escrita, espere por ela na leitura
**O modelo:** Uma escrita produz uma posição no log de replicação. Um cliente que carrega essa posição com a próxima requisição pode pedir a uma réplica que espere até alcançar esse ponto antes de ler.
**Como aplicar:** Depois de uma escrita no primário, leia a posição atual do WAL e guarde-a na sessão do cliente ou em um cookie assinado. Antes de ler de um standby na requisição seguinte, espere por essa posição, com um timeout limitado.

### A espera deve ser limitada, e o fallback deve ser decidido
**O modelo:** Esperar por uma réplica só é útil enquanto a espera é curta. Um timeout transforma uma espera sem limite em um modo de falha conhecido, que a aplicação consegue tratar.
**Como aplicar:** Escolha um timeout compatível com o orçamento de latência voltado ao usuário. Decida de antemão se uma leitura que estourou o timeout vai para o primário, devolve um resultado desatualizado ou mostra um erro.

## 🔑 Principais conclusões
- O PostgreSQL 19 oferece consistência de leitura das próprias escritas para leituras enviadas a standbys, por meio do comando `WAIT FOR LSN`.
- A aplicação captura a posição da escrita com `pg_current_wal_insert_lsn()` depois da escrita e a reutiliza na requisição seguinte.
- `WAIT FOR LSN` bloqueia a sessão do standby até que a reprodução alcance o LSN informado, para que a consulta seguinte veja a escrita.
- A cláusula de timeout é opcional. Com timeout, uma réplica que não alcança a posição a tempo produz um erro que a aplicação precisa tratar.
- O padrão não é novo: o MediaWiki já tem uma versão feita à mão, baseada em uma posição guardada do binlog do MySQL e numa espera pela réplica.
- O lançamento do PostgreSQL 19 veio dois meses depois do habitual, principalmente por causa do volume de problemas de segurança descrito pelo autor.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Em uma arquitetura de escala de leitura com standbys, a diferença de consistência entre primário e réplica é uma questão de design que costuma ser tratada de forma ad hoc em cada serviço. Se as suas equipes enviam leituras para réplicas, o `WAIT FOR LSN` move parte dessa lógica para o banco e dá a vocês um mecanismo padrão para governar. A pergunta que isso levanta: onde o token do LSN deve ficar (cookie do cliente, armazenamento de sessão ou cabeçalho da requisição), e o que uma leitura que estourou o timeout deve fazer, dado que a escolha do fallback muda tanto a latência quanto a consistência?

## 📚 Plano de leitura
### A partir do artigo
- [PostgreSQL 19 Released](https://www.postgresql.org/about/news/postgresql-19-released-3449/) - o anúncio oficial de lançamento, a fonte primária sobre o lançamento em si.
- [via](https://lobste.rs/s/pqyuhk/postgresql_19_released) - a discussão no Lobsters pela qual o autor encontrou o lançamento.
- [principalmente graças a](https://rhaas.blogspot.com/2026/10/postgres-19-is-late-and-its-ais-fault.html) - o post de blog que explica por que o lançamento atrasou.
- [`WAIT FOR LSN`](https://www.postgresql.org/docs/19/sql-wait.html) - a referência do comando, centro do recurso tratado no artigo.
- [log sequence number (LSN)](https://www.postgresql.org/docs/19/wal-internals.html) - a documentação dos internals do WAL necessária para entender as posições de LSN.
- [na Wikipédia](https://www.mediawiki.org/wiki/Manual:Database_access#Lag) - a documentação do MediaWiki sobre acesso ao banco e atraso de replicação, a referência anterior.
- [o código relevante do MediaWiki](https://github.com/wikimedia/mediawiki/blob/1ff78194b182ee4b5b3678f7d9fc9aefd0852cf7/includes/libs/Rdbms/Replication/MysqlReplicationReporter.php#L238-L299) - a implementação real da espera de replicação para MySQL.
- [WAIT_FOR_EXECUTED_GTID_SET()](https://dev.mysql.com/doc/refman/9.7/en/gtid-functions.html#function_wait-for-executed-gtid-set) - a função do MySQL que o MediaWiki usa hoje para a espera.
- [SOURCE_POS_WAIT()](https://dev.mysql.com/doc/refman/8.4/en/replication-functions-synchronization.html#function_source-pos-wait) - a alternativa mais antiga do MySQL para a mesma espera.

### Para se aprofundar
- Leitura das próprias escritas e outras garantias de sessão - o modelo de consistência que este recurso visa, e como ele se diferencia de garantias mais fortes.
- Tratamento de atraso de replicação em arquiteturas de escala de leitura - como outros bancos e serviços decidem entre leituras em réplicas e leituras no primário.
