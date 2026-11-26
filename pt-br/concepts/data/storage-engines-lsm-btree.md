---
type: Concept
title: 'Motores de armazenamento: LSM-trees e B-trees'
description: 'As duas formas principais de os bancos organizarem dados em disco - log-structured merge trees (gravar em sequência e depois mesclar arquivos ordenados em segundo plano) e B-trees (atualizar páginas de tamanho fixo no lugar) - com LSM-trees geralmente mais rápidas para escrita e B-trees para leitura.'
tags:
- data
- data-engineering
domain: data
subdomain: data-engineering
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 3 Storage and Retrieval: SSTables and LSM-Trees; B-Trees; Comparing B-Trees and LSM-Trees; Column-Oriented Storage'
- title: 'O''Neil, P. et al. (1996). The Log-Structured Merge-Tree (LSM-Tree). Acta Informatica 33'
  resource: 'https://doi.org/10.1007/s002360050048'
  kind: paper
- title: 'Bayer, R. and McCreight, E. (1972). Organization and Maintenance of Large Ordered Indices. Acta Informatica 1'
  resource: 'https://doi.org/10.1007/BF00288683'
  kind: paper
lang: pt-br
translation_of: /concepts/data/storage-engines-lsm-btree.md
---

# Motores de armazenamento: LSM-trees e B-trees

**Definição.** Um motor de armazenamento é a parte do banco que grava os dados em disco e os encontra de novo. Kleppmann descreve duas famílias. Uma log-structured merge tree (LSM-tree) acumula as escritas em uma tabela ordenada em memória, descarrega-a no disco como um arquivo ordenado imutável (SSTable) e mescla e compacta esses arquivos em segundo plano; é usada por LevelDB, RocksDB, Cassandra e HBase. Uma B-tree divide o banco em páginas de tamanho fixo organizadas em árvore e sobrescreve as páginas no lugar, com um write-ahead log para recuperação após falhas; é o índice padrão dos bancos relacionais. Como regra prática, LSM-trees são mais rápidas para escrita e B-trees para leitura, mas Kleppmann alerta que benchmarks dependem muito da carga de trabalho e é preciso testar a sua.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `data-engineering`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

A escolha explica por que bancos se comportam de forma diferente sob carga: LSM-trees transformam escritas aleatórias em sequenciais e comprimem bem, mas a compactação pode interferir nas requisições em primeiro plano e as leituras podem precisar consultar vários arquivos; B-trees dão leituras previsíveis e mantêm cada chave em um só lugar, o que simplifica o bloqueio transacional. O armazenamento colunar para análise (cap. 3) é um terceiro desenho, voltado a varrer poucas colunas em muitas linhas.

## Exemplo

Um serviço de métricas que ingere milhões de pontos por segundo escolhe um armazenamento baseado em LSM; o banco de pedidos, que atende principalmente consultas pontuais e transações, continua com os índices B-tree do PostgreSQL.

## Conceitos mais amplos

- [Banco de dados e SGBD](/pt-br/concepts/data/database-dbms.md)

## Conceitos relacionados

- [OLTP versus OLAP](/pt-br/concepts/data/oltp-olap.md)

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 3 Storage and Retrieval: SSTables and LSM-Trees; B-Trees; Comparing B-Trees and LSM-Trees; Column-Oriented Storage. Como as duas famílias funcionam, sua amplificação de escrita e os trade-offs, com o alerta de medir a própria carga.
2. O'Neil, P. et al. (1996). *The Log-Structured Merge-Tree (LSM-Tree)*. Acta Informatica 33. <https://doi.org/10.1007/s002360050048> — Artigo original da LSM-tree.
3. Bayer, R. and McCreight, E. (1972). *Organization and Maintenance of Large Ordered Indices*. Acta Informatica 1. <https://doi.org/10.1007/BF00288683> — Artigo original da B-tree.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
