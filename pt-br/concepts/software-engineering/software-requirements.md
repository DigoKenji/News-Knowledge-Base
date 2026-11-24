---
type: Concept
title: Requisitos e atributos de qualidade
description: Declarações do que um sistema deve fazer (requisitos funcionais) e de quão bem deve fazê-lo (requisitos não funcionais ou atributos de qualidade como desempenho, segurança, disponibilidade e manutenibilidade).
tags:
- software-engineering
- core
domain: software-engineering
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: ISO/IEC/IEEE (2018). ISO/IEC/IEEE 29148 - Requirements engineering
  resource: 'https://www.iso.org/standard/72089.html'
  kind: standard
- title: ISO/IEC (2023). ISO/IEC 25010 - Product quality model
  resource: 'https://www.iso.org/standard/78176.html'
  kind: standard
- title: IEEE Computer Society (2024). SWEBOK Guide V4.0, Software Requirements
  resource: 'https://www.computer.org/education/bodies-of-knowledge/software-engineering'
  kind: standard
foundation_candidate: true
lang: pt-br
translation_of: /concepts/software-engineering/software-requirements.md
---

# Requisitos e atributos de qualidade

**Definição.** A ISO/IEC/IEEE 29148 trata da engenharia de requisitos: levantar, analisar, especificar, validar e gerenciar requisitos. Requisitos funcionais descrevem comportamento ("o usuário pode exportar um relatório"); requisitos não funcionais restringem qualidades ("a exportação termina em menos de 3 segundos para 10.000 linhas"). A ISO/IEC 25010 fornece o modelo padrão de qualidade de produto que dá nome a essas qualidades.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Erros de requisitos são os mais caros de corrigir tarde, por isso determinam boa parte do sucesso de um projeto. Requisitos não funcionais moldam a arquitetura mais que os funcionais: dois sistemas com as mesmas funcionalidades mas necessidades diferentes de escala ou segurança terminam com desenhos muito diferentes.

## Exemplo

Funcional - "enviar um digest diário ao Telegram". Não funcional - "entregá-lo até 07:30 BRT em 99% dos dias e custar menos de US$ 20 por mês".

## Conceitos mais específicos

- [Atributos de qualidade e trade-offs](/pt-br/concepts/architecture/quality-attributes.md)
- [Análise de trade-offs ATAM](/pt-br/concepts/backend-systems/atam.md)

## Notas de revisão para Kenji

Candidato a fundamento: um órgão de padronização define este termo (veja as fontes). Ele passa para `foundations/` quando forem reunidas citações literais de dois órgãos emissores independentes (ADR-0010).

## Fontes

1. ISO/IEC/IEEE (2018). *ISO/IEC/IEEE 29148 - Requirements engineering*. <https://www.iso.org/standard/72089.html> — Processos de engenharia de requisitos e características de bons requisitos (pago; citado).
2. ISO/IEC (2023). *ISO/IEC 25010 - Product quality model*. <https://www.iso.org/standard/78176.html> — Lista padronizada de características de qualidade (pago; citado).
3. IEEE Computer Society (2024). *SWEBOK Guide V4.0*, Software Requirements. <https://www.computer.org/education/bodies-of-knowledge/software-engineering> — Requisitos funcionais vs não funcionais, levantamento e validação.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
