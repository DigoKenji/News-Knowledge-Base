---
type: Article Summary
title: 'O sistema invisível: o que acontece entre serviços'
description: Chamadas entre serviços escondem custos reais, então serviços saudáveis ainda podem gerar requisições que falham.
resource: https://blog.bytebytego.com/p/the-invisible-systems-what-happens
tags:
- architecture
- integration-and-software-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/the-invisible-systems-what-happens
  title: 'The Invisible Systems: What Happens Between Services'
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/the-invisible-systems-what-happens
source: ByteByteGo
source_type: blog
published: '2026-11-19'
domain: architecture
subdomain: integration-and-software-architecture
concepts: []
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/41ebaf74172d8192bd94d043a800edf1
lang: pt-br
translation_of: /notes/architecture/2026-11-20-the-invisible-systems-what-happens-between-services.md
---

## 🎯 Tese central
Cada seta entre dois serviços carrega trabalho, recursos e premissas ocultos, então uma requisição pode falhar mesmo quando os dois serviços estão funcionando corretamente.

## 🧭 Por que o autor escreveu
Diagramas de arquitetura mostram os serviços como caixas e as chamadas como setas, e a atenção vai para as caixas (código, bancos de dados, CPU, deploys). O autor argumenta que as setas merecem o mesmo escrutínio, porque é dali que vêm as falhas que parecem misteriosas.

## A natureza de uma chamada entre serviços
O texto disponível do artigo termina neste título. A paywall começa logo depois dele, então o corpo da seção não está disponível para ser expandido aqui.

O que a prévia afirma sobre esta seção é o enquadramento que atravessa toda a peça: uma chamada entre serviços não é gratuita. Ela consome trabalho, recursos e premissas que um diagrama de caixas e setas não mostra.

> 🔎 Contexto adicional: esta nota cobre apenas a prévia gratuita. As seções listadas abaixo são anunciadas na prévia, mas seu conteúdo não está incluído, então não são expandidas.

### Tópicos anunciados, mas não disponíveis na prévia
- Por que mover dados é custoso
- Por que serviços saudáveis podem ter problemas
- Como chamadas downstream gastam tempo
- Como retentativas (retries) podem causar problemas
- Como dependências lentas afetam as requisições
- Como mudanças de DNS afetam as requisições

## 🔑 Principais conclusões
- Uma requisição pode falhar enquanto os dois serviços estão funcionando corretamente, então um health check verde em cada serviço não prova que a chamada entre eles funciona.
- O autor cita três exemplos de modos de falha: um serviço esperando por uma conexão, uma chamada enviada a um endereço desatualizado e uma requisição que chega depois que quem chamou já desistiu.
- Cada seta carrega trabalho, recursos e premissas que um diagrama de arquitetura padrão deixa de fora.
- O argumento completo do artigo (custos de movimentação de dados, retentativas, dependências lentas, mudanças de DNS) está atrás da paywall e não é coberto por esta nota.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Tecnologia (conexões de rede, endereços e DNS são implícitos); a prévia não cobre os domínios de Negócio, Dados ou Aplicação |
| Escopo | Solução: uma única chamada entre dois serviços. O escopo só é declarado na prévia, então esta é uma classificação leve. |
| Stakeholders e preocupações (concerns) | Não declarados na prévia. Preocupação implícita: confiabilidade das chamadas entre serviços. |
| Frameworks e notações | Nenhum citado |

### Decisões e trade-offs
O artigo não descreve nenhuma decisão explícita no texto disponível.

### Fundamentos em que se apoia
Nenhuma entrada de fundamento corresponde ainda.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um arquiteto, o ponto útil é que a fronteira entre serviços é onde frequentemente estão os problemas de confiabilidade, então as revisões de confiabilidade devem cobrir as chamadas e seu tempo, e não apenas cada serviço isoladamente. Pergunta que ela levanta: quais das suas chamadas atuais entre serviços têm timeouts, regras de retentativa e resolução de endereços que alguém realmente revisou?

## 📚 Plano de leitura
### Do artigo
- [The Invisible Systems: What Happens Between Services](https://blog.bytebytego.com/p/the-invisible-systems-what-happens) — o artigo completo está atrás da paywall; leia-o por inteiro para as seções listadas acima.
### Para aprofundar
- Tempestades de retentativas (conceito de ontologia `retry-storm`) — relevante para a seção planejada do artigo sobre como retentativas causam problemas.
