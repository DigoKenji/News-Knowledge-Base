---
type: Concept
title: Governança de dados e qualidade de dados
description: Governança de dados é o exercício de autoridade e controle sobre como os dados são geridos - responsabilidade, políticas, padrões e prestação de contas; qualidade de dados é o quanto os dados servem ao uso pretendido, medida por dimensões como exatidão, completude, consistência e atualidade.
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
- title: DAMA International (2017). DAMA-DMBOK - Data Management Body of Knowledge, 2nd edition, ch. 3 "Data Governance" and ch. 13 "Data Quality"
  resource: 'https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/'
  kind: book
- title: ISO/IEC (2008). ISO/IEC 25012 - Software engineering - SQuaRE - Data quality model
  resource: 'https://www.iso.org/standard/35736.html'
  kind: standard
- title: Wang, R. Y. and Strong, D. M. (1996). Beyond Accuracy - What Data Quality Means to Data Consumers. Journal of Management Information Systems 12(4)
  resource: 'https://doi.org/10.1080/07421222.1996.11518099'
  kind: paper
foundation_candidate: true
lang: pt-br
translation_of: /concepts/data/data-governance-quality.md
---

# Governança de dados e qualidade de dados

**Definição.** O DAMA-DMBOK define governança de dados como o exercício de autoridade, controle e decisão compartilhada (planejamento, monitoramento e fiscalização) sobre a gestão dos ativos de dados, e a coloca no centro do seu framework. A ISO/IEC 25012 define um modelo de qualidade de dados com características inerentes (exatidão, completude, consistência, credibilidade, atualidade) e dependentes do sistema (disponibilidade, portabilidade, recuperabilidade); a ISO 8000 trata da qualidade de dados para troca de dados mestres.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

A governança responde quem é dono de um conjunto de dados, quem pode usá-lo e sob quais regras (incluindo leis de privacidade como LGPD e GDPR); as práticas de qualidade verificam que os dados estão certos antes que decisões ou modelos dependam deles. Sem elas, análises e ML herdam erros silenciosos - "lixo entra, lixo sai".

## Exemplo

Uma empresa nomeia um dono de dados para "cliente", define que um registro válido precisa de e-mail verificado e CPF/CNPJ, roda checagens noturnas que sinalizam registros fora dessas regras e restringe campos pessoais a papéis aprovados conforme a LGPD.

## Conceitos mais específicos

- [Metadados e catálogo de dados](/pt-br/concepts/data/metadata-catalog.md)
- [Falha silenciosa](/pt-br/concepts/backend-systems/silent-failure.md)
- [Drift de dados](/pt-br/concepts/ai-ml/data-drift.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. DAMA International (2017). *DAMA-DMBOK - Data Management Body of Knowledge*, 2nd edition, ch. 3 "Data Governance" and ch. 13 "Data Quality". <https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/> — Definições de governança de dados e gestão da qualidade de dados.
2. ISO/IEC (2008). *ISO/IEC 25012 - Software engineering - SQuaRE - Data quality model*. <https://www.iso.org/standard/35736.html> — Características de qualidade de dados (pago; citado, não transcrito).
3. Wang, R. Y. and Strong, D. M. (1996). *Beyond Accuracy - What Data Quality Means to Data Consumers*. Journal of Management Information Systems 12(4). <https://doi.org/10.1080/07421222.1996.11518099> — Visão de adequação ao uso e dimensões de qualidade derivadas empiricamente.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
