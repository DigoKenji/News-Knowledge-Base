---
type: Concept
title: Banco de dados e SGBD
description: Um banco de dados é uma coleção organizada e persistente de dados; um sistema de gerenciamento de banco de dados (SGBD) é o software que a armazena e permite que muitos usuários a definam, consultem, atualizem e protejam com segurança e de forma concorrente.
tags:
- data
- core
domain: data
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC (2015). ISO/IEC 2382 - Information technology - Vocabulary (entry "database")
  resource: 'https://www.iso.org/standard/63598.html'
  kind: standard
- title: Hellerstein, J. M., Stonebraker, M. and Hamilton, J. (2007). Architecture of a Database System. Foundations and Trends in Databases 1(2)
  resource: 'https://doi.org/10.1561/1900000002'
  kind: paper
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 6 "Data Storage and Operations"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 3 Storage and Retrieval (storage engines, indexes); ch. 2 Data Models and Query Languages'
foundation_candidate: true
lang: pt-br
translation_of: /concepts/data/database-dbms.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Banco de dados e SGBD

**Definição.** A ISO/IEC 2382 define banco de dados como uma coleção de dados organizada segundo uma estrutura conceitual que descreve as características dos dados e seus relacionamentos. Um SGBD fornece essa estrutura mais uma linguagem de consulta, um otimizador de consultas, armazenamento e indexação, controle de concorrência, recuperação após quedas e controle de acesso. Hellerstein, Stonebraker e Hamilton descrevem a arquitetura canônica: gerenciador de processos, processador de consultas, gerenciador de armazenamento transacional e utilitários compartilhados.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O SGBD é o motivo pelo qual as aplicações não precisam reinventar armazenamento concorrente seguro. As famílias incluem relacional (PostgreSQL, MySQL), chave-valor, documentos, colunas largas, grafo, séries temporais e bancos vetoriais, cada uma otimizada para um modelo de dados e padrão de acesso. Escolher um depende principalmente das consultas e da consistência necessárias, não da moda.

## Exemplo

Uma clínica guarda pacientes e consultas no PostgreSQL. Duas recepcionistas marcando o mesmo horário no mesmo instante são serializadas pelo SGBD; após uma queda de energia ele reexecuta o log de escrita antecipada (WAL) para que nenhum agendamento confirmado se perca.

## Conceitos mais específicos

- [Transação e ACID](/pt-br/concepts/data/transactions-acid.md)
- [OLTP versus OLAP](/pt-br/concepts/data/oltp-olap.md)
- [Modelo de dados (relacional, documento, grafo)](/pt-br/concepts/data/data-model.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 3 Storage and Retrieval (storage engines, indexes); ch. 2 Data Models and Query Languages: Como um banco guarda e encontra dados internamente, e os modelos de dados que expõe.

## Fontes

1. ISO/IEC (2015). *ISO/IEC 2382 - Information technology - Vocabulary* (entry "database"). <https://www.iso.org/standard/63598.html> — Definição padronizada de banco de dados (pago; citado, não transcrito).
2. Hellerstein, J. M., Stonebraker, M. and Hamilton, J. (2007). *Architecture of a Database System*. Foundations and Trends in Databases 1(2). <https://doi.org/10.1561/1900000002> — Componentes de um SGBD e como se encaixam.
3. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 6 "Data Storage and Operations". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — Gestão de bancos de dados como área de conhecimento da gestão de dados.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 3 Storage and Retrieval (storage engines, indexes); ch. 2 Data Models and Query Languages. Como um banco guarda e encontra dados internamente, e os modelos de dados que expõe.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
