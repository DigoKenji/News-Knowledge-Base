---
type: Paper Summary
title: 'Kaer: replanejamento consciente de segurança para sistemas multiagente'
description: Kaer restringe o feedback dos agentes a modelos fixos de falha e reparos determinísticos, resistindo a ataques de replanejamento com mais segurança do que as linhas de base.
resource: https://arxiv.org/abs/2611.24839
tags:
- security
- indirect-prompt-injection
- multi-agent-orchestration
- model-context-protocol
- llm-as-a-judge
- threat-modeling
- information-flow-control
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.24839
  title: 'Kaer: Security-Aware Replanning for Multi-Agent Systems'
  author: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
status: stable
source_url: https://arxiv.org/abs/2611.24839
source: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
source_type: arxiv
published: '2026-11-18'
domain: security
concepts:
- indirect-prompt-injection
- multi-agent-orchestration
- model-context-protocol
- llm-as-a-judge
- threat-modeling
- information-flow-control
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d8180aac8ee7f22afe44f
lang: pt-br
translation_of: /notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md
---

## 🎯 Em uma frase
O Kaer mostra que o replanejamento em um sistema multiagente pode resistir a agentes maliciosos se o planejador vir apenas modelos fixos de falha e os reparos forem determinísticos: ele atinge um score de segurança de 92.3% no gpt-5.6-sol, contra 69.8% do ControlValve e 35.0% do Magentic-One.

## ❓ O problema
Um orquestrador é um componente central que escreve um plano, entrega cada etapa a um agente especialista (um LLM com ferramentas) e revisa o plano quando uma etapa falha. Essa revisão, chamada de replanejamento, lê o relatório de falha do agente, e esse relatório é um texto escrito pelo próprio agente. Um agente malicioso pode esconder instruções nesse texto (injeção indireta de prompt) para fazer o planejador acrescentar etapas, trocar agentes ou redirecionar o trabalho. Isso é sequestro de controle: o atacante não quebra o orquestrador, ele direciona o plano. As defesas existentes verificam cada chamada de ferramenta ou corrigem o plano inteiro antes da execução, então a própria etapa de replanejamento fica desprotegida. Fixar o plano com antecedência também é difícil, porque muitas tarefas reais revelam o que precisam apenas enquanto rodam.

## 💡 A ideia
O Kaer não permite que os agentes expliquem falhas em texto livre. Um agente informa uma condição de uma lista fixa, e o planejador recebe apenas essa condição como um modelo, com operadores de reparo sugeridos, nunca a saída nem as anotações do agente. Os autores comparam isso a um estreitamento de tipos (type narrowing): passar apenas booleanos, números ou enums através de uma fronteira de confiança limita o que um atacante pode dizer ao número de valores possíveis. Os reparos também precisam ser construídos a partir de um pequeno conjunto de operadores determinísticos, e um plano revisado só é aceito se passar em todas as políticas de tarefa e de agente. Juntos, esses mecanismos fazem com que o atacante só possa escolher entre rótulos de falha pré-definidos, e cada rótulo só possa disparar mudanças em conformidade com as políticas.

## ⚙️ Como funciona
### Estrutura do plano e a regra de validade
Uma subtarefa é definida como uma tupla de um agente atribuído, um conjunto de capacidades concedidas e uma entrada, e um plano só é válido se cada subtarefa atender às condições da política.

```text
S_j = (A_j, {C_i}_j, (P_j, O_{j-1}))                    (2)
{C_i}_j ⊆ CT ∪ X_j                                      (3)
```

Símbolos: S_j é a subtarefa j. A_j é o agente atribuído a ela. {C_i}_j é o conjunto de capacidades que esse agente pode usar nessa subtarefa. P_j é a diretiva em linguagem natural. O_{j-1} são as saídas armazenadas das subtarefas anteriores das quais esta depende. CT é o conjunto de capacidades que a política de tarefa do usuário permite. X_j é o conjunto de capacidades extras, fora de CT, que o usuário autorizou explicitamente para essa subtarefa e esse agente, vazio até haver uma concessão. A condição (3) diz que uma subtarefa só pode usar capacidades que a política da tarefa permite ou que o usuário concedeu.

### Gateway confiável para privilégio mínimo
Todas as chamadas de ferramentas e de LLM passam por um gateway confiável, que o orquestrador configura a partir do plano aceito. O gateway encaminha uma chamada somente quando quem chama é o agente ativo e o alvo pertence a uma das capacidades dessa subtarefa. Chamadas feitas depois que o agente terminou são rejeitadas. Assim, um agente não consegue escalar privilégios nem agir em nome da subtarefa de outro. O gateway não inspeciona os argumentos nem os valores de retorno das chamadas permitidas, mas cada chamada continua observável no rastro. Esse componente é o que impõe as capacidades concedidas em tempo de execução; o restante do Kaer trata do replanejamento.

### Modelos de falha como único canal de feedback
Quando uma subtarefa falha, o agente devolve um relatório de encerramento que nomeia uma ou mais condições de um conjunto fixo de 14. Cada condição se torna um modelo que nomeia o problema e sugere operadores de reparo, de modo que o planejador recebe um vocabulário finito, e não texto livre. O gateway também acrescenta condições, por exemplo quando rejeita uma chamada ou quando uma ferramenta falha. Subtarefas bem-sucedidas não são avaliadas: o orquestrador repassa as saídas delas adiante sem verificá-las. Os relatórios de progresso dos agentes voltam apenas para o mesmo agente, nunca para o planejador.

### Operadores determinísticos de reparo e aceitação
Um reparo é uma sequência de um ou mais de sete operadores, aplicados em ordem a uma cópia do plano: RE-EXECUTE, REVISE-DIRECTIVE, RESCOPE, REASSIGN, INSERT, PRUNE e REWIRE. Somente subtarefas não concluídas podem mudar, então as saídas já concluídas permanecem fixas. O orquestrador rejeita um reparo que tenha como alvo uma posição anterior, que não possa ser aplicado ou que deixe um plano inválido. Operadores que concedem uma capacidade fora de CT, como RESCOPE, REASSIGN ou INSERT, só produzem efeito depois que o usuário autoriza essa capacidade. Um reparo rejeitado volta ao planejador com os motivos, e a tarefa termina após um número fixo de rejeições consecutivas.

## 🧪 Evidências
| Cenário | Linha de base | Este artigo | Métrica |
|---|---|---|---|
| gpt-5.6-sol, tentativas de ataque | ControlValve 69.8%; Magentic-One 35.0% | Kaer 92.3% | Score de segurança (fração de tentativas de ataque bloqueadas) |
| gpt-5.6-terra, tentativas de ataque | ControlValve 80.4%; Magentic-One 47.8% | Kaer 88.0% | Score de segurança |
| deepseek-v4-pro, tentativas de ataque | ControlValve 78.7%; Magentic-One 50.1% | Kaer 79.9% | Score de segurança |
| gpt-5.6-sol, Kaer vs. ablação com texto livre | Kaer com texto livre 75.7% | Kaer 92.3% | Score de segurança |
| gpt-5.6-sol, tentativas benignas | Magentic-One 91.4%; ControlValve 54.3% | Kaer 64.3% | Conclusão de tarefas benignas |
| gpt-5.6-terra, tentativas benignas | Magentic-One 78.6%; ControlValve 34.3% | Kaer 65.7% | Conclusão de tarefas benignas |
| deepseek-v4-pro, tentativas benignas | Magentic-One 84.3%; ControlValve 54.3% | Kaer 41.4% | Conclusão de tarefas benignas |
| deepseek-v4-pro, com verificação de saída | Kaer 29 de 70 | Kaer com validação de saída 38 de 70 | Tentativas benignas bem-sucedidas |
| gpt-5.6-sol e gpt-5.6-terra, com verificação de saída | Kaer 86.6% e 87.0% | 96.8% em ambos | Taxa de defesa |

Os resultados sustentam a tese central: nos três modelos o Kaer supera as duas linhas de base em segurança, e nos dois modelos GPT ele também conclui mais tarefas benignas do que o ControlValve. As principais ressalvas estão na própria tabela. O Magentic-One tem maior conclusão de tarefas benignas nos modelos GPT, e o Kaer tem a menor conclusão de tarefas benignas entre os cinco sistemas no deepseek-v4-pro. O Cintra é simulado e construído pelos mesmos autores, com 14 tarefas e cinco execuções por tarefa, então os percentuais se apoiam em números modestos de tentativas. Os ataques que ainda têm sucesso contra o Kaer passam, em sua maioria, por relatórios de sucesso, que os modelos de falha não inspecionam.

## ⚠️ Limitações
- Fora do escopo, como declaram os autores: uso indevido dentro das próprias capacidades concedidas a um agente (argumentos malformados, saídas de ferramentas falsificadas localmente) e comprometimento de componentes confiáveis, como o orquestrador, o planejador, o registro, os servidores MCP e os servidores de LLM.
- O Kaer não verifica se uma saída de sucesso está correta, porque verificar a correção de tarefas abertas é difícil. Os autores acrescentam uma verificação por LLM como extensão, que usa o mesmo LLM do planejador.
- Os modelos de falha omitem intencionalmente o contexto de execução, então tarefas que precisam descobrir a localização de fontes de dados continuam em grande parte sem solução.
- O gateway não inspeciona os argumentos nem os valores de retorno das chamadas que permite.
- 🔎 Contexto adicionado: todas as tentativas de ataque colocam o agente malicioso em um único papel (filesurfer), então ataques de vários agentes em conluio não são medidos aqui.
- 🔎 Contexto adicionado: o artigo não relata intervalos de confiança, e cada configuração tem 5 execuções por tarefa ou par, então uma diferença de poucos pontos pode se apoiar em muito poucas tentativas.

## 🏗️ O que isso significa para construir sistemas
Isto é interpretação, não uma afirmação dos autores. Em uma arquitetura real, o padrão se encaixa no contrato entre agente e orquestrador: relatórios de falha viram um enum tipado, o orquestrador mantém uma lista de permissões por subtarefa no gateway, e um validador de plano fica entre o planejador e a execução. Os custos são uma taxonomia de falhas que você precisa manter, menor conclusão em tarefas que exigem descoberta em tempo de execução, e uma chamada extra de LLM por sucesso caso você acrescente uma verificação de saída. Para confiar nele, seria preciso testar com a sua própria combinação de tarefas e ferramentas, porque o benchmark aqui é simulado e escrito pela equipe do artigo.

## 🔑 Principais conclusões
- Relatórios de falha em texto livre são o canal do ataque: com eles, o planejador pode ser levado a executar um script plantado, e o score de segurança do Kaer cai de 92.3% para 75.7% no gpt-5.6-sol quando se usa a ablação com texto livre.
- O planejador do Kaer vê apenas uma de 14 condições fixas de falha, e cada condição vem com operadores sugeridos que o planejador pode ignorar.
- Os reparos ficam limitados a sete operadores determinísticos, subtarefas concluídas não podem mudar, e todo plano revisado deve passar pelas políticas da tarefa e do agente.
- O Kaer não é um ganho de utilidade em todos os casos: no deepseek-v4-pro sua conclusão de tarefas benignas é de 41.4%, a menor entre os cinco sistemas, contra 84.3% do Magentic-One.
- A maioria dos ataques bem-sucedidos contra o Kaer usa agentes que relatam sucesso com saída errada, e a verificação de saída por LLM dos autores elevou a taxa de defesa para 96.8% nos dois modelos GPT.
- O privilégio mínimo é imposto pelo gateway em tempo de execução, e não pela redação do prompt: cada subtarefa alcança apenas os endpoints de suas capacidades concedidas.

## 📚 Plano de leitura
### Do artigo
- [ControlValve (Jha et al.)](https://openreview.net/forum?id=PNU9Rj5RDQ) — a defesa de estado da arte com a qual o Kaer é comparado, e a abordagem de políticas de fluxo de controle para ler primeiro.
- [Magentic-One](https://arxiv.org/abs/2411.04468) — a linha de base de orquestrador sem defesa, cujo replanejamento o Kaer modifica.
- [Jacob et al., type-directed privilege separation](https://arxiv.org/abs/2509.25926) — a origem da ideia de "booleanos, números ou enums" por trás dos modelos de falha.
- [Progent](https://arxiv.org/abs/2504.11703) — controle de privilégios para agentes, uma alternativa de defesa em tempo de execução para comparar.
- [Architecting secure AI agents](https://arxiv.org/abs/2603.30016) — um levantamento de defesas em nível de sistema contra injeção indireta de prompt, para o contexto de design mais amplo.
- [Agent-Sentry](https://arxiv.org/abs/2603.22868) — limitar agentes de LLM por proveniência de execução, uma abordagem recente de imposição em tempo de execução.

### Para aprofundar
- Failure taxonomy of multi-agent LLM systems (Cemri et al., "Why do multi-agent LLM systems fail?") — a taxonomia da qual os modelos de falha do Kaer derivam.
- Information-flow control for AI agents (Costa et al., "Securing AI agents with information-flow control") — as ideias de endosso e rebaixamento nas quais o Kaer se apoia.
- Control-flow hijacking in multi-agent systems — a classe de ataque que o artigo nomeia, e a razão pela qual a integridade do plano importa.
- Planning-time attacks on workflows (FlowSteer, Li et al.) — por que um plano que o planejador pode reescrever é ele próprio uma superfície de ataque.

## 🔗 Conceitos
- [indirect-prompt-injection](/pt-br/concepts/indirect-prompt-injection.md)
- [multi-agent-orchestration](/pt-br/concepts/multi-agent-orchestration.md)
- [model-context-protocol](/pt-br/concepts/model-context-protocol.md)
- [llm-as-a-judge](/pt-br/concepts/llm-as-a-judge.md)
- [threat-modeling](/pt-br/concepts/threat-modeling.md)
- [information-flow-control](/pt-br/concepts/information-flow-control.md)
