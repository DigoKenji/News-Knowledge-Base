---
type: Article Summary
title: Depois do waterfall, do agile e do DevOps — o playbook de SDLC AI-native da Anthropic
description: O SDLC AI-native substitui reuniões de passagem de bastão por artefatos versionados e governança no momento da escrita, já que programar mais rápido sozinho deixa o pipeline lento.
resource: https://medium.com/@vateo/after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-playbook-0131a3a9156e
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://medium.com/@vateo/after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-playbook-0131a3a9156e
  title: After waterfall, agile, and DevOps — Anthropic’s AI-Native SDLC playbook
  author: vateo
status: stable
source_url: https://medium.com/@vateo/after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-playbook-0131a3a9156e
source: Medium (vateo)
source_type: medium
published: '2026-09-22'
domain: software-engineering
concepts:
- sdlc
- devops
- agile-scrum
- ai-agent
- agent-harness
- context-engineering
- software-requirements
- software-testing
- version-control-code-review
- ci-cd
- observability
- incident-response
- spec-driven-development
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d81569a37c9fdaa50422a
lang: pt-br
translation_of: /notes/software-engineering/2026-11-25-after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-p.md
---

## 🎯 Tese central
Agora que a IA tornou a escrita de código rápida, o atraso restante está no processo ao redor dele. O autor argumenta que a solução é um SDLC AI-native: um ciclo em que a IA está embutida em todas as fases, as fases se passam o bastão por meio de arquivos versionados em vez de reuniões e tickets, e a governança é aplicada no momento em que o trabalho é escrito, e não numa revisão posterior.

## 🧭 Por que o autor escreveu o texto
O autor lidera uma transformação de IA em uma organização de engenharia desde o ano passado. A equipe acelerou a codificação com ferramentas como o Claude Code, mas a entrega não acelerou, porque planejamento, revisão, deploy e aprovações continuam rodando no ritmo humano. Se a tese estiver certa, o processo e o organograma precisam ser redesenhados em torno da IA, e não apenas receber ferramentas de IA dentro do pipeline existente.

## 1. O problema que o playbook resolve
O artigo começa com uma experiência que a maioria das equipes compartilha quando a codificação com IA se consolida: o código é escrito muito mais rápido, mas a entrega não fica mais rápida. Primeiro as equipes culpam a si mesmas por usar mal as ferramentas, depois culpam as ferramentas, e só então descobrem que o processo é que está travado.

O autor descreve o próprio caminho. O primeiro passo foi trocar a ferramenta de codificação para o Claude Code, a etapa mais fácil e com retorno mais rápido. O segundo passo foi apoiar-se no desenvolvimento orientado por especificação (SDD, spec-driven development) para lidar com questões mais difíceis: quantos humanos e agentes colaboram, como o código se mantém consistente e como isso escala entre versões. O trabalho de desenvolvimento agora roda em três modos, escolhidos conforme a complexidade do projeto: AI-assisted, AI-collaborative e AI-autonomous. Produto, design, QA e operações também adotaram IA. Os problemas aparecem nas junções. A passagem do produto para a UI ainda gera um mock no Figma que um agente não consegue consumir diretamente, e a passagem entre desenvolvimento e testes, e entre deploy e operações, continua sendo o pipeline antigo.

O diagnóstico inicial do autor tem três partes. Quando o código deixa de ser o gargalo:

- O gargalo passa para os dois lados da etapa de build (planejamento, revisão, deploy), que continuam rodando na velocidade humana.
- Os agentes escrevem a maior parte do diff, e a revisão humana linha por linha não consegue acompanhar.
- Exceções e aprovações ainda passam por comitês semanais e mensais, então o custo da governança sobe.

A frase-resumo é "quanto mais rápido você programa, mais tempo você espera".

### Otimização local versus autonomia do pipeline inteiro
Cada etapa fica mais rápida, mas o fluxo como um todo não. O ponto do autor é que a otimização local não resolve o que é, na essência, um problema de autonomia do pipeline inteiro. Resolvê-lo exige uma metodologia desenhada para a IA desde o início, que o autor chama de metodologia AI-native.

### A analogia da eletrificação
O autor lembra como as fábricas adotaram a eletricidade. Os donos primeiro acoplaram motores elétricos no lugar onde estivera a máquina a vapor, no mesmo prédio e nos mesmos eixos. O ganho real levou cerca de quarenta anos e só chegou quando as fábricas foram redesenhadas em torno da eletricidade e surgiu a linha de montagem. A analogia carrega o principal alerta do artigo: trocar a fonte de energia sem mudar o layout rende pouco.

🔎 Contexto adicional: "autonomia do pipeline inteiro" aqui significa que um agente de IA pode conduzir o trabalho através de planejamento, build, teste, deploy e operações com pouca intervenção humana, e não apenas ajudar em uma etapa.

## 2. O que é um SDLC AI-native: o ciclo e as jogadas
O SDLC AI-native é apresentado como um processo reimaginado que mantém os objetivos de controle do ciclo de vida tradicional, mas os aplica de outra forma. O fluxo deixa de ser linear e vira um ciclo, com a IA embutida em cada ponto. O processo automatiza as passagens e dispara a próxima jogada, o que, segundo o autor, resolve as passagens manuais e desajeitadas entre fases no SDLC tradicional.

As seis fases permanecem: planejar, projetar, construir, testar, implantar e manter. O que muda é a forma como elas se conectam.

### O ciclo de arquivos
Em vez de reuniões, tickets e assinaturas, cada fase termina com o commit de um arquivo no git, e a fase seguinte começa lendo esse arquivo. A cadeia do artigo é:

intent.md → spec.md → plan.md → código e testes → PR com achados de revisão → registro de incidente → de volta a um intent.md novo

A ideia central é que humanos e IA leem o mesmo arquivo. Um product owner pode trabalhar com o intent.md, e um agente também. Os documentos deixam de ser memorandos escritos para pessoas e passam a ser a interface entre humanos e agentes. O histórico de commits também serve como trilha de auditoria: quem pediu o quê, o que a IA fez e quem aprovou.

### Jogadas
O corpo do playbook consiste em dezesseis jogadas, agrupadas em seis etapas não lineares. Cada jogada cobre cinco pontos: o que muda, como começar, os passos concretos, a governança e como medir o resultado.

### Quatro princípios
As jogadas se apoiam em quatro princípios:

- Artefatos são a evidência: tudo vai para o git.
- Defesa em profundidade: skills tornam as violações raras, e hooks as tornam quase impossíveis.
- Humanos ocupam os assentos de julgamento: intenção, risco e autorização sempre pertencem às pessoas.
- Coexistir com o que você já tem: Jira e Figma não precisam ser derrubados.

## 3. Planejar: os requisitos começam com o intent.md
Quem tem a ideia conversa com a IA diretamente, com as próprias palavras, até que a ideia fique clara. A IA transforma essa conversa em um arquivo intent.md, que registra o problema, o resultado desejado, os usuários afetados, as restrições e as questões em aberto.

O exemplo do artigo foi escrito por J. Ortiz, da área de operações de sinistros, e não por um product manager ou um engenheiro:

```markdown
# Intent: claims status self-service 
Author: J. Ortiz (claims operations). Status: draft. 

## Problem 
Customers phone the contact center to ask where their claim is. Handlers spend roughly a third of call time on status-only queries. 

## Proposed outcome 
Customers see claim status, next step and expected date in the portal. 

## Affected users and systems 
Claims handlers, portal team, claims-core API. 

## Constraints 
No new PII in the portal session. Existing authentication only. 

## Open questions 
Do third-party loss adjusters need access too?
```

> 🔎 Contexto adicional: o artigo rotula este bloco como javascript, mas ele é um documento de intenção em texto simples, então a cerca usa uma dica de markdown. O conteúdo não foi alterado.

**O que isso faz:**
- `# Intent: ...` e as linhas `Author` / `Status` identificam o documento e seu dono, então o arquivo funciona como registro de quem pediu o quê.
- `## Problem` declara a dor em termos mensuráveis: um terço do tempo de chamada dos atendentes vai para consultas que são só de status.
- `## Proposed outcome` descreve o que o cliente vai ver, e não como isso será construído, o que mantém a intenção separada do design.
- `## Affected users and systems` nomeia as pessoas e os sistemas que um agente precisa conhecer antes de planejar.
- `## Constraints` e `## Open questions` trazem o limite de segurança (nenhum PII novo, apenas a autenticação existente) e uma dúvida de escopo ainda não resolvida, para que ambos fiquem visíveis antes do início do design.

O papel do product owner muda de escrever requisitos para revisar a intenção. Esclarecimento e registro são feitos pelo agente. Em comparação com o processo atual, o autor diz que a análise de requisitos fica mais enxuta e começa antes, apoiando-se na dor e nos insights dos usuários e nos MRD e BRD. O artigo é franco ao afirmar que a forma exata de usar o intent.md ainda precisa ser ajustada para cada equipe e cada pessoa.

## 4. Projetar: a política é aplicada na escrita, não na revisão
O design de protótipos e o design de requisitos se fundem em uma única sessão. O intent.md vai para um agente de design, que produz um mock ou uma página de front-end junto com um spec.md.

Regras de segurança, compliance e marca são escritas como skills, então a IA fica restringida no momento em que escreve a spec. Quando as regras entram em conflito, a IA sinaliza o conflito como item de risco e o encaminha ao dono da política antes que a engenharia o veja.

> ❌ **Anti-padrão — política verificada na revisão:** problemas de compliance, segurança e marca aparecem em uma reunião de revisão semanas depois de a spec ser escrita. **Por que as pessoas fazem isso:** é assim que os gates de revisão sempre funcionaram, e as regras vivem na cabeça das pessoas ou em documentos que ninguém lê na fase de design. **Consequência:** o retrabalho custa uma ordem de grandeza a mais do que custaria no design, como diz o artigo. **Correção:** codificar as regras como skills que o agente lê enquanto escreve a spec, e encaminhar os conflitos ao dono da política naquele momento.

O autor observa que a prática atual de sua equipe usa um prd-spec.md, mas ir direto dos requisitos para páginas desenhadas ainda exige forte envolvimento de designers.

## 5. Construir: revisar o plano, não o código
Engenheiros começam em plan mode. Eles dão ao Claude a intenção e a spec, e ele redige um plano de implementação que cobre quais arquivos mudam, em que ordem, o que pode quebrar e o que prova que a mudança está concluída.

🔎 Contexto adicional: plan mode é um modo do Claude Code em que o agente propõe um plano para aprovação antes de fazer edições.

O padrão é rigoroso: um engenheiro que não participou da discussão deveria conseguir implementar a mudança apenas a partir do plano. Quando o plano é aprovado, a implementação normalmente sai em uma única passada, porque as questões de direção já foram resolvidas nos documentos e a execução é o que a IA faz melhor.

A fase vem com três recursos:

- **CLAUDE.md**: descrito como o manual de integração da IA, cobrindo comandos, convenções, arquitetura e os erros que ela continua cometendo. Um erro só entra no arquivo na segunda vez em que acontece, e o arquivo fica abaixo de uma página.
- **Skills**: transformam o conhecimento organizacional em restrições flexíveis sobre o código.
- **Hooks**: restrições rígidas. Caminhos bloqueados continuam bloqueados, e credenciais nunca entram em um diff.

A regra-resumo: skills tornam a conformidade provável, e hooks a tornam certa. A prática atual do autor é um diretório de design e spec que registra essas regras por categoria e módulo, com detalhe real, e o autor diz que isso funciona bastante bem.

## 6. Testar: a IA verifica a si mesma primeiro
Antes de uma sessão entregar qualquer coisa, a IA roda o build, roda os testes e tira screenshots da UI, e corrige os problemas até tudo ficar verde. "Concluído" inclui a evidência.

A disciplina de correção de bugs é a parte mais inteligente. A IA primeiro reproduz o bug como um teste que falha e faz commit desse teste. Depois corrige o código, com um hook travando o arquivo de teste para que ele não possa ser alterado. Um teste preexistente que o agente não consegue reescrever é a única prova de que o bug realmente foi eliminado.

> ❌ **Anti-padrão — o agente edita o teste que deve passar:** o agente ajusta ou apaga o teste para que a suíte fique verde. **Por que as pessoas fazem isso:** um teste que falha bloqueia a sessão, e reescrevê-lo é o jeito mais rápido de seguir em frente. **Consequência:** a suíte deixa de provar qualquer coisa, e o bug pode voltar sem ser notado. **Correção:** fazer commit do teste que reproduz o problema primeiro, e travar o arquivo de teste com um hook antes de a correção ser escrita.

### Avaliando a configuração da própria IA
Uma camada além, a configuração da IA recebe testes de regressão como código. O autor sugere escolher de 20 a 50 tarefas históricas reais como suíte de avaliação. Qualquer mudança em CLAUDE.md, skills ou hooks executa o conjunto inteiro. Cada incidente em produção se torna um caso de avaliação permanente.

🔎 Contexto adicional: uma suíte de avaliação, aqui, é um conjunto fixo de tarefas com resultados bons conhecidos, executado contra a configuração do agente para detectar regressões após uma mudança.

## 7. Implantar: o agente chega ao portão de produção, mas não o atravessa
A revisão acontece nas duas direções. A IA revisa pull requests de outras pessoas sob uma mesma política, e trabalha os comentários nos próprios pull requests. Humanos respondem exatamente duas perguntas:

> 📋 **Checklist — revisão humana de mudanças feitas por IA**
> - [ ] Esta mudança é o que o plano previa?
> - [ ] O risco é aceitável?

A governança não roda em reuniões de revisão. Ela roda em hooks como gates. A autonomia é escalonada: a IA publica livremente em ambientes de desenvolvimento, o staging fica no meio, e a produção exige a aprovação de um humano nomeado.

> ❌ **Anti-padrão — o agente passa pelo gate de produção por conta própria:** o mesmo agente que escreveu a mudança também a envia para produção. **Por que as pessoas fazem isso:** elimina a espera por um humano e parece automação. **Consequência:** a etapa de aprovação desaparece da trilha de auditoria, e nenhuma pessoa nomeada é dona do risco de produção. **Correção:** manter a aprovação de produção com um humano nomeado, e transformar o gate em hook, não em reunião.

O autor também dá um lembrete contraintuitivo: o rollback deveria ser o caminho mais ensaiado de todo o pipeline, porque o agente da fase de manutenção pode acioná-lo a qualquer hora da madrugada.

## 8. Manter: o ciclo se fecha
O monitoramento não usa IA nenhuma. Scripts determinísticos observam métricas contra linhas de base estatísticas. Só quando uma métrica ultrapassa um limite é que a IA é acordada, e sua autoridade depende de quanto a métrica se afastou:

- Um desvio padrão: registrar.
- Dois desvios padrão: diagnóstico somente leitura.
- Três desvios padrão: agir. Mesmo assim, só são permitidas duas ações: abrir um PR para revisão ou acionar um runbook pré-aprovado.

> ❌ **Anti-padrão — remediação aberta pelo agente:** um agente acordado por um alerta pode tomar qualquer ação corretiva que julgue necessária. **Por que as pessoas fazem isso:** é o jeito mais rápido de estancar um incidente à noite. **Consequência:** o agente pode alterar produção de maneiras que ninguém revisou, e a mudança não deixa para trás nenhum caminho aprovado. **Correção:** atrelar a autoridade do agente ao tamanho do desvio, e limitar a ação a um PR revisado ou a um runbook pré-aprovado.

O diagnóstico é escrito como um novo intent.md e volta para a fase de planejamento. Problemas em produção viram insumo para a próxima rodada de desenvolvimento, algo que, segundo o autor, o SDLC tradicional nunca teve.

O playbook inclui um exemplo real de um canal de incidentes no Slack. Às 22h04, alguém mencionou o Claude com @. Três minutos depois, ele entregou um diagnóstico com base no diff da release. Um humano respondeu "Go". Quatro minutos depois, o rollback estava completo e o postmortem estava registrado. A solicitação, o diagnóstico, a autorização e a correção ficam todos no canal, que funciona como log de auditoria.

## 9. Por que eu levo isso a sério
O autor dá três razões. Primeiro, o playbook atua na camada de processo. Usando o padrão de ferramentas, depois processo, depois indústria, o autor argumenta que essa é a etapa com que uma mudança de paradigma costuma começar. Segundo, ele trata a governança diretamente: compliance, auditoria e segurança são a última milha da adoção em empresas, algo que a conversa sobre codificação com IA em grande parte deixou de lado. O autor argumenta que o que empresas reguladas conseguem adotar tende a se tornar o padrão de fato. Terceiro, os autores executam o processo eles mesmos: 80% do código mesclado na Anthropic é escrito pelo Claude, e a suíte de testes é quase inteiramente escrita pelo Claude. O artigo repete esse número de mais de 80% como divulgação da própria empresa.

A ressalva do autor é que o padrão de fato ainda é um rascunho de quem está na frente, não um veredito. O agile levou quase uma década para passar do manifesto ao padrão global, e o autor espera que o processo AI-native precise de um período próprio de adoção, embora provavelmente não tão longo. O conselho final é avançar além deste playbook, porque na era da IA o objetivo é ser protagonista, não espectador.

## 🧠 Modelos mentais
### Ferramentas, depois processo, depois indústria
**O modelo:** os paradigmas de software passaram por waterfall, agile e DevOps seguindo a mesma ordem: as ferramentas mudam primeiro, o processo se atualiza depois, e então a indústria se reorganiza em torno delas. O agile produziu o Jira e o Scrum Master, e o DevOps produziu o Kubernetes e o SRE.
**Como aplicar:** quando uma nova ferramenta se torna padrão nas suas equipes, espere que o processo e os papéis sejam a próxima coisa a mudar, e planeje isso em vez de parar na implantação da ferramenta.

### O motor da fábrica
**O modelo:** adotar uma tecnologia poderosa sem mudar o layout em volta captura muito pouco do seu valor. O ganho chega quando o layout é redesenhado em torno da nova fonte de energia.
**Como aplicar:** para cada etapa que você acelera com IA, verifique se as passagens antes e depois dela ainda assumem o gargalo antigo.

### Skills tornam a conformidade provável, hooks a tornam certa
**O modelo:** restrições flexíveis, como skills e convenções no CLAUDE.md, tornam o comportamento correto provável. Restrições rígidas, como hooks que bloqueiam caminhos ou vazamentos de credenciais, tornam a violação quase impossível.
**Como aplicar:** coloque em skills as regras que toleram exceções, e em hooks as regras que nunca podem ser quebradas.

### Autoridade graduada
**O modelo:** a autoridade da IA deve crescer com a gravidade da situação: registrar desvios pequenos, diagnosticar os maiores e agir apenas dentro de movimentos pré-aprovados.
**Como aplicar:** defina limites explícitos e as ações permitidas para cada nível antes de um incidente, não durante um.

### Humanos ocupam os assentos de julgamento
**O modelo:** intenção, risco e autorização permanecem com as pessoas, enquanto o agente faz o rascunho, a verificação e a execução.
**Como aplicar:** antes de automatizar uma etapa, nomeie o humano que é dono da decisão que ela alimenta.

## 🔑 Principais conclusões
- A geração de código não é mais o gargalo; planejamento, revisão, deploy e aprovações são, e todos ainda rodam na velocidade humana.
- Cada fase do SDLC passa o bastão fazendo commit de um arquivo no git (intent.md, spec.md, plan.md, código, registro de incidente), e humanos e agentes leem o mesmo arquivo.
- Regras de política e compliance devem ser aplicadas quando a spec é escrita, usando skills, com conflitos encaminhados ao dono da política, e não descobertos numa reunião de revisão semanas depois.
- Testes escritos antes de uma correção, travados por um hook, são a prova de que um bug foi eliminado; a IA não deve conseguir editá-los.
- A aprovação de produção fica com um humano nomeado. A autonomia é escalonada por ambiente, e o rollback deve ser o caminho mais ensaiado.
- O monitoramento é determinístico. A IA só é acordada além de um limite estatístico, com autoridade que cresce com o desvio, e os incidentes alimentam a próxima intenção.

## 🏛️ Para o Kenji
O seu trabalho em governança de arquitetura é onde isso se aplica mais diretamente: a tese do playbook é que a governança precisa sair dos gates de revisão e ir para o momento em que os artefatos são escritos, e que os arquivos de passagem se tornam a trilha de auditoria da arquitetura. Esta é a sua interpretação, não a do autor. A pergunta que isso levanta para a sua prática de arquitetura é quais das suas decisões atuais de governança poderiam ser codificadas como skills ou hooks, e quais devem continuar com um humano nomeado porque são decisões de julgamento sobre risco.

## 📚 Plano de leitura
### A partir do artigo
- Nenhum link externo utilizável. Os links inline do artigo apontam para nomes de arquivo (intent.md, spec.md, plan.md, CLAUDE.md, prd-spec.md), e não para recursos, então não há nada a seguir.

### Para aprofundar
- Anthropic, The AI-Native SDLC playbook (datado de 2026-08-21 nas referências do artigo) — a fonte primária que este artigo resume, e a origem das dezesseis jogadas.
- História do agile e do Scrum — o artigo usa o caminho de quase dez anos do agile, do manifesto ao padrão, como referência de adoção.
- DevOps, Kubernetes e SRE — o ponto de comparação do artigo sobre como mudanças de paradigma anteriores geraram novas ferramentas e papéis.
- Desenvolvimento orientado por especificação — a prática por trás do segundo passo do artigo, em que as specs guiam a forma como os agentes constroem software.

## Conceitos
- [sdlc](/pt-br/concepts/software-engineering/sdlc.md)
- [devops](/pt-br/concepts/software-engineering/devops.md)
- [agile-scrum](/pt-br/concepts/software-engineering/agile-scrum.md)
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [agent-harness](/pt-br/concepts/ai-ml/agent-harness.md)
- [context-engineering](/pt-br/concepts/ai-ml/context-engineering.md)
- [software-requirements](/pt-br/concepts/software-engineering/software-requirements.md)
- [software-testing](/pt-br/concepts/software-engineering/software-testing.md)
- [version-control-code-review](/pt-br/concepts/software-engineering/version-control-code-review.md)
- [ci-cd](/pt-br/concepts/software-engineering/ci-cd.md)
- [observability](/pt-br/concepts/cloud-infra/observability.md)
- [incident-response](/pt-br/concepts/security/incident-response.md)
- [spec-driven-development](/pt-br/concepts/software-engineering/spec-driven-development.md)
