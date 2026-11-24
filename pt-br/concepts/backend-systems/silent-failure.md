---
type: Concept
title: Falha silenciosa
description: Uma falha que não gera erro - o sistema devolve uma saída que parece normal mas está errada, incompleta ou desatualizada - então o monitoramento baseado em erros e disponibilidade não a vê e o dano se acumula até alguém notar os resultados.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Huang, P. et al. (2017). Gray Failure - The Achilles' Heel of Cloud-Scale Systems. HotOS 2017
  resource: 'https://www.microsoft.com/en-us/research/publication/gray-failure-achilles-heel-cloud-scale-systems/'
  kind: paper
- title: 'Dixit, H. D. et al. (2021). Silent Data Corruptions at Scale. arXiv:2102.11245'
  resource: 'https://arxiv.org/abs/2102.11245'
  kind: paper
- title: Hochschild, P. et al. (2021). Cores that don't count. HotOS 2021
  resource: 'https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s01-hochschild.pdf'
  kind: paper
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 6 Monitoring Distributed Systems. O'Reilly / Google
  resource: 'https://sre.google/sre-book/monitoring-distributed-systems/'
  kind: book
lang: pt-br
translation_of: /concepts/backend-systems/silent-failure.md
---

# Falha silenciosa

**Definição.** Uma falha silenciosa é uma falha cujos sintomas não disparam nenhum alarme: sem exceção, sem 5xx, sem crash. Os exemplos vão de corrupção silenciosa de dados no hardware até um pipeline que processa zero registros sem avisar, um cache servindo dados velhos ou um modelo de ML cuja precisão cai enquanto continua respondendo com confiança.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Ideias de pesquisa relacionadas: gray failure (Huang et al., Microsoft), em que um componente está degradado de formas que seus health checks não detectam mas os usuários sim; corrupção silenciosa de dados por CPUs defeituosas, documentada na Meta e no Google ("cores that don't count"), em que contas saem erradas de vez em quando sem nenhum sinal de erro. A detecção depende de verificar as saídas, não só se o sistema está vivo: sondas ponta a ponta com respostas conhecidas, checagens de qualidade de dados (contagem de linhas, atualidade, distribuições), checksums, reconciliação entre sistemas e SLOs de correção. O livro de SRE do Google recomenda monitoramento caixa-preta do que os usuários vivenciam além dos sinais internos.

## Por que importa e trade-offs

Falhas silenciosas duram mais e se espalham mais que as barulhentas, porque nada dispara uma resposta; dados corrompidos podem ser copiados para backups e sistemas posteriores. Checagens de correção custam e geram alarmes falsos, então os times precisam escolher as saídas que mais importam.

## Exemplo

Depois que uma fonte muda o formato do feed, a execução diária continua terminando com sucesso mas seleciona zero artigos dela por duas semanas; só uma checagem do tipo "nenhum item da fonte X em N dias" pega o problema.

## Fontes

1. Huang, P. et al. (2017). *Gray Failure - The Achilles' Heel of Cloud-Scale Systems*. HotOS 2017. <https://www.microsoft.com/en-us/research/publication/gray-failure-achilles-heel-cloud-scale-systems/> — Falhas que os health checks não veem mas os usuários sentem (observabilidade diferencial).
2. Dixit, H. D. et al. (2021). *Silent Data Corruptions at Scale*. arXiv:2102.11245. <https://arxiv.org/abs/2102.11245> — Corrupção silenciosa de dados no hardware medida na frota da Meta.
3. Hochschild, P. et al. (2021). *Cores that don't count*. HotOS 2021. <https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s01-hochschild.pdf> — Evidência independente do Google de núcleos instáveis produzindo resultados errados em silêncio.
4. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 6 Monitoring Distributed Systems. O'Reilly / Google. <https://sre.google/sre-book/monitoring-distributed-systems/> — Monitoramento caixa-preta vs caixa-branca; monitorar os sintomas que os usuários veem.

## Relações
- (nenhuma ainda)

## Notas
- [Resilience testing your agents on Amazon Bedrock AgentCore](/pt-br/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md)
