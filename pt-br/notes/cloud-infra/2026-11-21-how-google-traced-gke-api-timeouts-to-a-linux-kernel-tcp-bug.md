---
type: Article Summary
title: Como o Google Rastreou Timeouts da API do GKE até um Bug de TCP no Kernel Linux
description: Timeouts intermitentes na API do GKE vieram de três bugs que interagiam no PSC, no Envoy e no kernel Linux, corrigidos por um patch no kernel.
resource: https://www.infoq.com/news/2026/11/gke-kernel-network-bug
tags:
- cloud-infra
- control-plane
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/gke-kernel-network-bug
  title: How Google Traced GKE API Timeouts to a Linux Kernel TCP Bug
  author: Renato Losio
status: stable
source_url: https://www.infoq.com/news/2026/11/gke-kernel-network-bug
source: InfoQ
source_type: blog
published: '2026-11-21'
domain: cloud-infra
concepts:
- control-plane
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81ff8352c31d0f2df011
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-21-how-google-traced-gke-api-timeouts-to-a-linux-kernel-tcp-bug.md
---

## 🎯 Tese central
Timeouts intermitentes na API do GKE vieram de três bugs que interagiam no Private Service Connect, no Envoy e no kernel Linux, e somente um patch no kernel removeu a causa raiz.

## 🧭 Por que o autor escreveu
Em janeiro, os pods de um cliente sofreram timeouts intermitentes ao acessar o servidor de API do Kubernetes, bloqueando cerca de 800 desenvolvedores de implantar em produção. Losio resume o relato do Google sobre como os engenheiros rastrearam o problema. Se a tese se sustentar, um único sintoma, como um i/o timeout, pode se originar em qualquer camada, então a depuração precisa acompanhar o estado do protocolo através das camadas.

## O incidente: ACKs obsoletos em novas conexões
Em janeiro, um cliente que executava vários clusters GKE com Private Service Connect (PSC) para acesso privado ao control plane observou timeouts intermitentes quando os pods tentavam acessar o servidor de API do Kubernetes. Capturas de pacotes mostraram que o control plane respondia a novas solicitações de conexão TCP com pacotes ACK obsoletos, que pertenciam a conexões já fechadas. O artigo descreve três bugs separados em diferentes camadas de rede, o que explica por que nenhuma camada isolada explicava a falha.

### Private Service Connect
> 🔎 Contexto adicional: O PSC dá a uma VPC acesso privado a um serviço gerenciado por meio de um endpoint dentro da rede do cliente, em vez de pela internet pública. Aqui ele transportava o caminho dos pods até o control plane do GKE.

O artigo cita a reordenação de pacotes no PSC como uma das três causas. Como o PSC fica entre os pods e o servidor de API, o sintoma aparece dentro do cluster, embora a falha esteja no caminho de rede.

### Control plane
O control plane é o conjunto de componentes, aqui o servidor de API do Kubernetes, que decide quais trabalhos são executados e em que estado. Os pods dependem dele para chamadas de API, então seus timeouts aparecem como falhas de aplicação. O artigo usa o PSC como o caminho privado até esse endpoint.

### ACK obsoleto
Um ACK é a confirmação que um peer TCP envia para confirmar dados ou o estado de uma conexão. Um ACK obsoleto se refere a uma conexão que não existe mais, então não pode completar um novo handshake. O control plane os enviava em resposta a novas solicitações de conexão, o que o cliente percebia como um timeout em uma conexão que nunca se completava.

## Três causas que interagiam
A investigação identificou três causas que interagiam: reordenação de pacotes no PSC, o comportamento de half-close do Envoy, que impedia que conexões TCP fossem fechadas corretamente, e um bug do kernel Linux que descartava certos pacotes FIN reordenados. Cada camada oferecia uma explicação plausível, mas cada uma era incompleta por si só. Juntas, produziram um único sintoma: um i/o timeout em uma conexão que nunca se completou.

### Half-close do TCP
> 🔎 Contexto adicional: Uma conexão TCP se fecha quando cada lado envia um pacote FIN. Um half-close significa que um lado terminou de enviar, mas ainda pode receber. O Envoy é o proxy nesse caminho.

O artigo diz que o comportamento de half-close do Envoy impedia que as conexões TCP fossem fechadas corretamente. Uma conexão que nunca completa seu fechamento deixa estado para trás em pelo menos um dos lados.

### Pacotes FIN reordenados e FIN_WAIT_2
Um pacote FIN sinaliza que um lado terminou de enviar. O bug do kernel descartava certos pacotes FIN reordenados, então o handshake de fechamento nunca se completava, e os sockets ficavam presos indefinidamente no estado FIN_WAIT_2.
> 🔎 Contexto adicional: FIN_WAIT_2 é o estado em que um socket que está fechando entra depois que o seu próprio FIN é confirmado, enquanto espera o FIN do outro lado.

## Por que a investigação foi difícil
Thakur argumenta que este caso foi diferente da maioria dos incidentes de rede. A maioria se resolve em uma única camada, como uma regra de firewall mal configurada ou um pool de conexões esgotado. Aqui, o mesmo i/o timeout apareceu na camada do PSC, na máquina de estados TCP do kernel e no gerenciamento de conexões do Envoy, então cada camada tinha uma explicação plausível e cada explicação era incompleta. Engenheiros do GKE, do PSC, de redes, de hostnet, do kernel e da comunidade upstream do Linux trabalharam juntos no problema. Capturas de pacotes em várias camadas de rede foram essenciais, e correlacionar sintomas de rede com mudanças de infraestrutura ligou os três bugs.

> ❌ **Antipadrão — Parar na primeira camada plausível:** Aceitar a primeira explicação que se encaixa no sintoma (uma mudança no PSC, um problema no proxy ou um problema no kernel) e seguir em frente. **Por que as pessoas fazem isso:** A maioria dos casos de rede se resolve de fato em uma camada, então o hábito costuma estar certo. **Consequência:** Aqui cada explicação era incompleta, então corrigir uma camada deixa as outras ativas. **Correção:** Verifique cada explicação contra capturas de pacotes e o estado do protocolo em cada camada antes de dar o caso por encerrado. O artigo sugere isso pelo relato de Thakur, sem rotulá-lo como tal.

## Três correções coordenadas
O Google corrigiu o problema em três etapas ao longo de várias semanas. Primeiro reverteu uma mudança no PSC para restaurar a conectividade. Depois corrigiu o tratamento do protocolo do proxy e adicionou testes de regressão. Por fim, um patch no kernel Linux, escrito por Eric Dumazet, ex-engenheiro de software do Google, corrigiu o problema subjacente no TCP. A correção final foi um patch no próprio kernel, não uma mudança de configuração nem uma feature flag.

### Protocolo PROXY
> 🔎 Contexto adicional: O protocolo PROXY é um pequeno cabeçalho que um proxy coloca no início de uma conexão TCP para carregar o endereço original do cliente. O artigo diz que o Google corrigiu a forma como isso era tratado, mas não dá os detalhes do erro.

### Testes de regressão
> 🔎 Contexto adicional: Um teste de regressão reproduz um bug já corrigido para que ele não volte sem ser notado. O artigo menciona esses testes sem listar o que eles cobrem.

## Reações da comunidade
Rohan Karne, diretor-gerente do Google Cloud, chama isso de "a realidade sem glamour da engenharia de plataformas de missão crítica": descartar os suspeitos óbvios e investigar capturas de pacotes até que a causa raiz no kernel apareça.

Guangba Yu, pesquisador de pós-doutorado da Chinese University of Hong Kong, aponta o método transferível para AIOps e chaos engineering: organizar observações simultâneas de caminhos, estado de protocolo e eventos de implantação, e separar a recuperação do sintoma da correção da máquina de estados. Ele também observa que o caso não fornece uma taxa de falha populacional e não explica todos os timeouts de API.

Justin Garrison, field CTO da Sidero, aponta que o GKE executava um kernel de três anos antes (6.1), onde o bug já estava corrigido no upstream, então o Google precisou fazer o backport da correção.

> ❌ **Antipadrão — Executar um kernel antigo sem acompanhar as correções upstream:** O kernel do nó fica anos atrás do upstream, então bugs já corrigidos no upstream continuam ativos até alguém fazer o backport. **Por que as pessoas fazem isso:** O artigo não diz; uma plataforma gerenciada pode fixar um kernel mais antigo por estabilidade. **Consequência:** Um bug já corrigido no upstream ainda chega à produção, e a correção precisa ser levada por backport. **Correção:** Acompanhe as correções upstream em relação ao kernel que você executa e trate os backports como trabalho de rotina. O artigo levanta isso pelo comentário de Garrison, e não como um alerta próprio.

## Rollout da correção no kernel
A correção no kernel foi incorporada ao Linux 6.9.7 e levada por backport para os ramos LTS 6.6 e 6.1. Chegou aos clientes do GKE na versão 1.33.5 e posteriores.
> 🔎 Contexto adicional: Um ramo LTS (suporte de longo prazo) recebe correções selecionadas por anos após seu lançamento. Um backport copia uma correção de um kernel mais novo para um ramo mais antigo.

## 🧠 Modelos mentais
### Mesmo sintoma, muitas camadas
**O modelo:** O mesmo sintoma, aqui um i/o timeout, pode vir de falhas em várias camadas, e cada camada pode oferecer uma explicação plausível, mas incompleta.
**Como aplicar:** Compare uma conexão com falha entre as camadas, usando capturas de pacotes e o estado do protocolo, antes de aceitar qualquer explicação isolada.

### Recuperar o sintoma versus corrigir a máquina de estados
**O modelo:** Restaurar o tráfego não é o mesmo que corrigir o estado do protocolo que causou a falha.
**Como aplicar:** Depois de uma mitigação, como um rollback, verifique a máquina de estados subjacente antes de encerrar o incidente.

## 🔑 Principais conclusões
- Três causas que interagiam, no PSC, no Envoy e no kernel Linux, produziram um único sintoma: um i/o timeout em conexões que nunca se completavam.
- Capturas de pacotes em várias camadas de rede foram essenciais, e correlacioná-las com mudanças de infraestrutura ligou os três bugs.
- O Google primeiro reverteu uma mudança no PSC para restaurar a conectividade, depois corrigiu o tratamento do protocolo proxy e adicionou testes de regressão.
- A correção final foi um patch no kernel Linux, não uma mudança de configuração nem uma feature flag.
- Certos pacotes FIN reordenados foram descartados pelo kernel, deixando sockets presos em FIN_WAIT_2 indefinidamente.
- A correção no kernel incorporada no Linux 6.9.7 chegou ao GKE por meio de backports para 6.6 e 6.1 LTS, distribuídos a partir da versão 1.33.5.
- Segundo Yu, o caso não fornece uma taxa de falha populacional e não explica todos os timeouts de API.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Em uma arquitetura de plataforma, a correção depende de uma cadeia de redes gerenciadas, proxies e kernels de sistema operacional, cada um sob a responsabilidade de uma equipe diferente, e este incidente atravessou todos eles. A pergunta que ele levanta: como o processo de incidentes da sua plataforma transferiria um timeout entre a equipe de rede gerenciada, a equipe do proxy e a equipe do SO, e como você ficaria sabendo que uma correção de kernel chegou ao upstream?

## 📚 Plano de leitura
### Do artigo
- [Artigo da InfoQ](https://www.infoq.com/news/2026/11/gke-kernel-network-bug) — o resumo original, com as citações completas dos especialistas.
### Para aprofundar
- Post do blog do Google Cloud de Akash Thakur sobre a investigação no GKE — a fonte primária que este artigo resume, para o detalhe em nível de pacote (o artigo não dá link).
- Máquina de estados do TCP no Linux e o estado FIN_WAIT_2 — para ler o bug do kernel em seu contexto completo.
- Protocolo PROXY — o tratamento de proxy que o Google corrigiu na sua segunda correção.

## Conceitos
- [control-plane](/pt-br/concepts/cloud-infra/control-plane.md)
