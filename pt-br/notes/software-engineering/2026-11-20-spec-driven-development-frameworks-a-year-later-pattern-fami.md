---
type: Article Summary
title: 'Frameworks de desenvolvimento orientado a especificações um ano depois: famílias de padrões, medições e uma matriz de escolha'
description: 'Nenhum framework de especificação venceu; o padrão certo depende do tipo de mudança, e o custo de manter a especificação atualizada após uma segunda mudança de requisito ainda não foi medido.'
resource: https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
tags:
- software-engineering
- adr
- agent-harness
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
  title: 'Spec-Driven Development Frameworks a Year Later: Pattern Families, Measurements and a Choice Matrix'
  author: Level Up Coding
status: stable
source_url: https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
source: Medium (Level Up Coding)
source_type: medium
published: '2026-10-30'
domain: software-engineering
concepts:
- adr
- agent-harness
- llm-as-a-judge
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81878993f04066a929ee
lang: pt-br
translation_of: /notes/software-engineering/2026-11-20-spec-driven-development-frameworks-a-year-later-pattern-fami.md
---

## 🎯 Tese central
Nenhum framework de desenvolvimento orientado a especificações venceu um ano depois de o campo ter quinze concorrentes. O padrão certo depende do tipo de mudança, e o custo de manter uma especificação verdadeira após uma segunda mudança de requisito é a parte que ninguém mediu.

## 🧭 Por que o autor escreveu
O autor parte de uma equipe que escolhe agora um framework de especificação, com uma única medição pública por tipo de mudança em mãos, na qual quatro frameworks ficaram dentro da faixa de ruído em tickets mesclados na primeira entrega. Se a tese estiver certa, a decisão deve ser tomada por tipo de mudança (correção de bug, funcionalidade em uma base de código existente, mudança entre componentes), e cada opção deve ser testada na segunda mudança de requisito, e não escolhida como produto.

> 🔎 Contexto adicionado: "Spec" aqui significa uma descrição estruturada do comportamento pretendido que um agente de programação com IA recebe como contrato para uma mudança.

## Quick Win — Ideia central
O início desta seção dá o conselho prático em uma linha: não escolha um framework, escolha um padrão para o tipo de mudança que você mais faz e depois verifique como ele se comporta na segunda mudança de requisito.

### Padrão por tipo de mudança
A única medição por tipo de mudança que o autor conhece mostra que uma correção de bug delimitada, com um teste que falha, não ganhou nada com a especificação: o controle sem especificação mesclou 9 de 10 tickets, o mesmo que as melhores configurações (Kiro com uma Bugfix Spec dedicada, BMAD com uma trilha compatível com a tarefa). Para uma funcionalidade em um sistema existente, a vantagem reaparece, com os braços com especificação mesclando de 2 a 3 tickets a mais em cada 10. A recomendação do autor é um template de correção de bug com três títulos mais um teste de regressão, apresentado explicitamente como uma recomendação a testar, e não como resultado medido. Para uma mudança que atravessa vários repositórios, os recursos dedicados do OpenSpec estão em beta e não foi encontrada nenhuma comparação.

### Manutenção da especificação após a implantação
Este é o ponto que o autor considera o mais importante e que nenhum estudo encontrado compara: o custo de manter a especificação atualizada. Nos repositórios públicos estudados, 81.5% dos arquivos de especificação têm um único commit. O argumento do autor é que um padrão que não obriga a especificação a ser reconciliada com o código no mesmo pull request deixa o agente, após a segunda mudança, com um livro de regras desatualizado como contexto.

> ❌ **Anti-padrão — Escolher um framework para toda mudança:** adotar um único fluxo de trabalho para correções de bug, funcionalidades e trabalho entre componentes indistintamente. **Por que as pessoas fazem isso:** o artigo não dá motivo; é o padrão natural quando uma equipe adota uma única ferramenta. **Consequência:** os próprios fornecedores dividiram seus caminhos (Kiro Bugfix Specs, a extensão de bug do Spec Kit, BMAD Quick Flow), o que o autor lê como uma admissão de que um fluxo único não serve para toda mudança. **Correção:** escolher um padrão por tipo de mudança e testá-lo na segunda mudança.

## O ano em que a primeira versão deixou de ser promessa
A seção descreve como o campo amadureceu em um ano e o que mudou na forma como os fornecedores apresentam suas ferramentas.

### Spec Kit 1.0.0
O Spec Kit lançou a versão 1.0.0 em 21 de agosto de 2026, um ano depois do seu primeiro commit. O mantenedor descreveu a 1.0.0 como um número de versão, não como um certificado de maturidade, e admitiu que entrar em uma base de código existente poderia ser mais suave do que é, e que a fronteira de confiança é orientação, não imposição. Na prática, isso significa que o agente é informado do que fazer e nada o impede de fazer outra coisa. Para um leitor que avalia a ferramenta, o lançamento é sinal de uma API se estabilizando, e não de um histórico comprovado em produção.

### Primitivas de plataforma
A versão 1.0 combina cinco primitivas: extensões, presets, integrações, workflows e steps. O ponto do autor é que o framework virou uma plataforma, então presets e extensões agora precisam ser versionados e atualizados como qualquer outra dependência de software. Isso transforma uma escolha de template em uma escolha de gestão de dependências.

### Posicionamento no Technology Radar
No Technology Radar vol. 34 de abril, a Thoughtworks posicionou o Spec Kit e o OpenSpec no anel "Assess", que significa vale explorar, mas ainda não vale adoção ampla. A entrada sobre o Spec Kit descreve dois grandes grupos: equipes que confiam em agentes cada vez mais capazes com estrutura mínima, e equipes que preferem fluxos definidos. O autor faz a ressalva de que o Radar relata experiência de equipes, não medições.

### Caminhos para bugs
A mudança mais importante, na visão do autor, é que os fornecedores agora dividem seus caminhos por tipo de mudança. O Kiro tem Bugfix Specs, o Spec Kit tem uma extensão de bug desde a versão 0.9.5 (assess, fix, test, sem spec, plan ou tasks), e o BMAD tem o Quick Flow. A leitura do autor é que isso é uma admissão de que um fluxo único não serve para toda mudança.

> 🔎 Contexto adicionado: Kiro, Spec Kit, OpenSpec e BMAD são citados como ferramentas no artigo; o artigo não descreve seus internos além do que é citado aqui.

*(Figura do artigo: o ano dos frameworks de especificação, do primeiro commit a uma 1.0 que não promete maturidade.)*

## Quatro famílias de padrões em vez de quinze produtos
A seção reduz os quinze produtos a quatro famílias de padrões, agrupadas pela forma como cada família representa uma mudança e pelo que trata como fonte da verdade. O autor apresenta isso como hipótese: a forma como uma família representa uma mudança determina em grande parte o custo de manter a especificação, embora os estudos citados não separem padrão, ferramenta e processo de equipe.

### Especificação como estatuto consolidado
O modelo mais simples que o autor oferece são os estatutos de uma empresa mantidos em um único texto consolidado. A constituição é a carta, o conjunto de regras que toda mudança herda. As famílias diferem na forma como cada mudança é incorporada e depois fundida nesse texto.

### Família constitution / specify / plan / tasks
Esta família, representada pelo Spec Kit e, em forma de IDE, pelos três documentos do Kiro, registra cada mudança como uma aplicação completa com três anexos: o quê, como e uma lista de tarefas. O comando converge compara o código atual com a especificação, o plano e as tarefas, e acrescenta as tarefas que faltam, sem alterar a especificação nem o código. O converge é, portanto, um detector de lacunas que lê e acrescenta tarefas, não um decisor sobre qual requisito prevalece.

> 🔎 Contexto adicionado: converge é o comando do Spec Kit citado no artigo; aqui ele é descrito apenas na medida em que o artigo o descreve.

### Família propose / apply / archive com delta de especificação
O OpenSpec representa cada mudança como um delta de especificação, uma emenda que lista apenas as disposições acrescentadas, modificadas e revogadas. O arquivamento funde a mudança na especificação atual e guarda sua pasta com uma data. A seção MODIFIED traz um bloco de substituição completo, e não uma nota sobre a diferença. O delta é o veículo da mudança; o texto consolidado é o resultado da fusão dos deltas.

### Família IDE
O Kiro e os modos de planejamento em editores mantêm requisitos, design e tarefas dentro do editor. A Bugfix Spec do Kiro usa um padrão WHEN … THEN / SHALL / SHALL CONTINUE TO, de modo que a correção também descreve o comportamento que precisa sobreviver à mudança. A IDE é um eixo transversal: o Kiro pertence a esta família, mas representa uma mudança como a família da constituição.

### Família spec-as-source
Nesta família, a especificação é o código-fonte, e o código gerado é a saída do compilador. O Tessl é o exemplo, e ele virou um registro de skills neste ano, enquanto o framework continuou em beta.

### Níveis de especificação e não determinismo
Birgitta Böckeler, da Thoughtworks, distinguiu os níveis spec-first, spec-anchored e spec-as-source em uma análise de 2025 sobre três ferramentas. A análise dela também encontrou o ponto fraco do último nível: gerar repetidamente a partir da mesma especificação produziu códigos diferentes. O autor trata isso como um risco embutido na abordagem, e não como um defeito de uma versão específica.

> ❌ **Anti-padrão — Tratar a especificação como fonte e regenerar a cada mudança:** deixar a especificação ser a única coisa que um humano edita e regenerar o código a partir dela. **Por que as pessoas fazem isso:** o artigo não dá motivo; é a promessa da família spec-as-source. **Consequência:** a geração repetida a partir da mesma especificação produziu códigos diferentes na análise de Böckeler. **Correção:** o artigo não prescreve uma.

A classificação do autor coloca as células em dois eixos e observa que o BMAD é tratado como uma variante da aplicação completa, com trilhas ajustadas à escala. Os mecanismos para a segunda mudança se reduzem a duas estratégias: uma aplicação completa ou uma emenda. O autor sinaliza que esta é a classificação dele, e não o resultado de um estudo sobre convergência de mercado.

*(Figura do artigo: quatro famílias de padrões em dois eixos, como você representa uma mudança e qual é a fonte da verdade.)*

## O protocolo com o qual leio cada medição
O autor declara logo de início que o texto não contém execuções próprias, e que as medições vêm de preprints, relatórios e relatos de praticantes reunidos até o fim de outubro de 2026. A seção apresenta o protocolo que o autor usa para ler cada número.

### Seis condições para uma comparação justa
Uma comparação justa, nos termos do autor, precisa de três tipos de tarefa mais uma segunda mudança de requisito, do mesmo modelo, de versões fixadas, de um orçamento de tempo, de critérios de aceitação e de uma separação entre tempo decorrido e minutos humanos ativos. Sem isso, uma diferença entre ferramentas pode refletir a configuração, e não o padrão.

### O benchmark Uvik
A melhor medição disponível é o Spec-Driven Development Benchmark 2026, publicado por Paul Francis, da Uvik Software. Ele tem 50 tickets de produção de cinco repositórios, o mesmo modelo declarado, ordem aleatória dos braços por ticket, e até 45 minutos para a especificação mais até 45 para a construção. Isso significa que o controle tinha até 45 minutos no total, enquanto os braços com especificação tinham até 90. Ele quebra três condições: algumas versões de ferramentas são descritas apenas como "build" e "6.x", o Kiro rodou em um harness diferente, e não há uma segunda mudança, como os próprios autores declaram: "One ticket at a time".

### Harness
Um harness é o ambiente que dirige o agente, ou seja, seus prompts, ferramentas e loop. O ponto do autor é que, quando uma ferramenta roda em um harness diferente, a comparação fica confundida, porque os resultados da ferramenta não são apenas uma função do seu padrão de especificação.

### Faixa de ruído
A faixa de ruído é o intervalo dentro do qual uma diferença pode se resumir ao acaso. Os autores dão ±10 pontos percentuais, e com 10 tickets por célula um único ticket move o resultado por esse valor. O autor observa que isso é a resolução do escore, não um intervalo de confiança. As diferenças ficam dentro da faixa operacional dos autores, o que não prova que as ferramentas sejam equivalentes. O relatório vem de uma editora comercial e não foi reproduzido de forma independente.

```javascript
# Spec pattern evaluation protocol (one sprint)
tasks:   1. bug fix with a failing test  2. feature in an existing module
         3. change across two components  4. second requirement change for task 2
fixed:   model + version, tool versions, time budget for spec and build
record:  elapsed time | active human minutes | rounds of corrections
         spec lines | test results | who reconciled spec with code
```

**O que isso faz:**
- Lista as quatro tarefas, incluindo a segunda mudança de requisito na tarefa 2, que é o caso que o benchmark existente deixa de fora.
- Fixa as variáveis (modelo e versões, versões das ferramentas, orçamentos de tempo) para que a comparação isole o padrão.
- Registra o tempo decorrido e os minutos humanos ativos separadamente, para que velocidade não seja confundida com atenção.
- Registra linhas de especificação, resultados de testes e quem reconciliou a especificação com o código, o que transforma a manutenção da especificação em uma grandeza medida.

*(Figura do artigo: o protocolo de avaliação. A melhor medição disponível cumpre algumas condições e não estuda a segunda mudança.)*

## Resultados: o que foi medido e o que foi apenas prometido
A seção mantém cada número ligado à pergunta que ele responde, para que o autor não os empilhe em um ranking.

### Primeira entrega
No benchmark Uvik, quatro frameworks mesclaram de 40 a 42 dos 50 tickets, contra 36 do controle sem especificação. A mediana de tempo até a mescla foi de 29 minutos para o controle e 36 para a família delta, com 41 minutos para a família IDE, 44 para a aplicação completa e 55 para o BMAD. Os frameworks ficam dentro do ruído uns dos outros. O controle tem a menor mediana de tempo entre as próprias mudanças mescladas, mas cada braço mesclou um conjunto diferente de tickets, então isso não é uma comparação limpa de velocidade.

### Por tipo de mudança
Com 10 tickets por célula, o controle obteve 90% em uma correção de bug com teste que falha, contra 80 a 90% dos braços com especificação. Para uma funcionalidade em FastAPI ou Django, o controle obteve 60%, contra 80 a 90%, e para uma mudança que toca seis ou mais arquivos, o controle obteve 50%, contra 80%. O autor diz que esta é a única medição que ele conhece que sustenta a tese de que o melhor padrão para uma correção de bug não é o melhor para uma funcionalidade, de forma direcional e não como ranking. Uma mudança que toca seis ou mais arquivos não é o mesmo que uma mudança entre vários serviços ou repositórios.

### Correções e documentos de especificação
As rodadas de correção após o relatório de conclusão foram de 0,46 a 0,64 nos braços com especificação e 0,74 no controle. A mediana do tamanho da especificação foi de 93 linhas na família delta a 188 no BMAD. A divergência entre especificação e código em mudanças mescladas foi de 2,4% para os deltas e 4,9% para a IDE, com 12,5% para a aplicação completa e 12,2% para o BMAD. O autor observa que, com cerca de 40 mudanças mescladas por braço, isso são poucos eventos (1, 2, 5 e 5 mudanças), um sinal para investigar e não uma diferença confirmada, e é a maior diferença pontual entre famílias na primeira entrega.

### Qualidade por juiz LLM
Um preprint de abril de 2026, com 128 execuções em cinco repositórios, avaliou o sistema dos autores com um LLM-as-a-judge, um modelo que avalia a saída no lugar de um humano. O fluxo completo do Spec Kit obteve 3,51 em uma escala de 1 a 5, e a variante sem artefatos, 3,46. Os hooks de ancoragem no repositório deram um ganho mais claro, de +0,15 ponto, que os autores descrevem como captura de erros de contexto mais cedo, e não como elevação da média. Um pequeno teste humano cego não confirmou a direção: 19 votos para o fluxo completo, 8 para a variante com hooks e 33 empates.

> 🔎 Contexto adicionado: um juiz LLM pode ter vieses próprios; o artigo não discute isso, então o teste humano é a única verificação cruzada do autor.

### Tempo das execuções concluídas
As execuções concluídas levaram 14,4 minutos sem artefatos e 24,0 com o fluxo completo. O autor aponta que elas vêm de orçamentos de tempo diferentes (40 e 90 minutos), então não medem o custo de acrescentar uma especificação.

### Piloto com revisores
Um piloto com cinco revisores, de setembro de 2026, acrescenta uma distinção entre detecção e responsabilização. Com uma especificação aprovada, a revocação (a parcela de discrepâncias reais encontradas) não diferiu de forma significativa: 0,525 contra 0,518, p = 0,69, o que não prova que não haja efeito. Atribuir uma descoberta a um requisito específico subiu de zero para 0,81, ao custo de uma revisão mais longa. O zero é estrutural, pois sem uma especificação aprovada não há nada a citar. A leitura do autor é que, neste piloto, a especificação comprou responsabilização, e não detecção.

### Testes de custo de praticantes
Duas execuções de um mesmo praticante no Medium (IT Chronicles, GPT-5.2) mostraram que o Spec Kit usou de 97 a 109% mais tokens do que o OpenSpec. Outro praticante (Mani, Medium) relatou que a mesma funcionalidade em uma base de código existente levou cerca de 5 horas com revisões usando o Spec Kit, e menos de duas com o OpenSpec. O autor rotula ambos como sinais de execuções isoladas, e não como normas.

### Apenas documentação
Vários recursos estão documentados, mas não medidos: detecção de drift no agente em nuvem do OpenSpec, as extensões converge e de reconciliação de artefatos no Spec Kit, testes de comportamento inalterado na Bugfix Spec, e as trilhas do BMAD.

### Estrutura leve sem framework
A estrutura leve tem apoio parcial. O controle sem artefatos de especificação foi competitivo na correção de bug e em mudanças pequenas, embora o autor não tenha testado o template recomendado antes. Em um preprint pré-registrado de setembro de 2026, um quadro de especificação de 267 palavras teve menos defeitos sinalizados por verificadores em 95 de 100 pares não empatados (de 250), na geração de código Python em uma única tentativa. O autor observa que não se sabe quanto disso vem do formato e quanto do raciocínio extra, já que, no piloto de Garg, raciocinar antes de programar produziu efeito semelhante em tarefas simples. Não foi encontrado nenhum estudo que compare um template com um framework na sequência completa, incluindo uma segunda mudança.

*(Figura do artigo: células verdes foram medidas por alguém, cinzas são prometidas pela documentação, brancas estão vazias, incluindo toda a linha de segunda mudança.)*

## A segunda mudança de requisito: quem quebra
A seção abre com uma citação de Helmuth von Moltke: "No plan survives first contact with the enemy." (Nenhum plano sobrevive ao primeiro contato com o inimigo.) O autor aplica a frase a uma especificação escrita antes da implementação.

### Desvio de implementação
Agarwal e coautores, da CMU e do Google, em um estudo de 2026, separaram duas formas de uma especificação quebrar. O desvio de implementação ocorre quando a especificação diz que uma sessão expira após 30 minutos e o agente implementa 60. Uma discrepância assim aparece nos testes, desde que os testes reflitam a versão correta do requisito.

### Decaimento da especificação
O decaimento da especificação ocorre quando o negócio muda deliberadamente o tempo para uma hora e a especificação ainda diz meia hora. Aqui, o código e os testes podem já corresponder à nova intenção, então uma suíte de testes verde passa apesar da especificação antiga. Os repositórios estudados mostram 81,5% dos arquivos de especificação com um único commit, embora o autor observe que muitos são descrições de mudança arquivadas, então isso não é a parcela de especificações desatualizadas. Uma avaliação estratificada por LLM de 1.000 documentos encontrou que 77,6% continham detalhes de implementação. Os autores concluem, a partir de entrevistas, que essas especificações envelhecem mais rápido, uma conclusão e não uma taxa medida.

> ❌ **Anti-padrão — Escrever detalhes de implementação na especificação:** descrever como o código funciona em vez do comportamento que ele deve ter. **Por que as pessoas fazem isso:** o artigo não diz; ele relata que 77,6% dos documentos amostrados fazem isso. **Consequência:** segundo as entrevistas dos autores, essas especificações envelhecem mais rápido. **Correção:** o artigo sugere manter a especificação restrita ao comportamento; não dá uma regra detalhada.

### Cenários e deltas paralelos
Um cenário é um exemplo de comportamento vinculado a um requisito, como um caso de teste escrito em prosa. A issue #1246 mostrou que duas mudanças no mesmo requisito, preparadas sobre a mesma base antiga, podiam ser arquivadas corretamente, enquanto a segunda removia silenciosamente os cenários da primeira. O mecanismo é que o bloco inteiro é substituído pelo nome do requisito. Uma proteção posterior aborta o arquivamento quando cenários atuais estão ausentes. A issue #1387 mostra a lacuna que sobra: preservar nomes de cenários não protege contra sobrescrever conteúdo mais novo com conteúdo mais antigo. O autor concilia isso com a divergência de 2,4% de antes: aquela medição cobriu tickets individuais, enquanto as issues descrevem deltas sobrepostos, preparados sobre uma base desatualizada e arquivados um após o outro.

> ❌ **Anti-padrão — Deltas paralelos sobre uma base desatualizada, arquivados em sequência:** duas mudanças em um mesmo requisito, ambas escritas contra uma versão antiga da especificação. **Por que as pessoas fazem isso:** o artigo não diz; o trabalho paralelo sobre um mesmo requisito é o caso implícito. **Consequência:** o segundo arquivamento pode remover ou sobrescrever a primeira mudança silenciosamente, mesmo quando os nomes dos cenários são preservados. **Correção:** o artigo aponta para uma verificação de drift antes do arquivamento e para mudanças explicitamente vinculadas e versionadas entre repositórios.

### Converge e a especificação viva
A família de aplicação completa tem o converge, que detecta lacunas entre o código e os artefatos, mas não decide se um novo requisito substitui o antigo. A escolha entre os modelos flow-back, flow-forward e living spec, descritos no guia do projeto, fica com a equipe. Na família IDE, a sincronização após uma mudança de design não tem medição de custo publicada. Na spec-as-source, a segunda mudança significa regenerar, com o risco de não determinismo.

### Uma equipe que abandonou o fluxo
Uma equipe SaaS no Japão (RevComm) abandonou o fluxo de deltas no frontend após alguns meses, porque o comportamento da UI mudava com frequência demais e as atualizações de especificação não se pagavam (blog de engenharia, julho de 2026). O autor diz que isso não é um veredito sobre todas as implantações.

*(Figura do artigo: deltas paralelos sobre uma base antiga. A proteção cobre os nomes dos cenários, mas o arquivamento ainda pode sobrescrever conteúdo mais novo de requisito.)*

### A conclusão do autor
Seja qual for a família, uma mudança de comportamento deve apontar para o contrato atual e para a mudança de especificação relacionada. Idealmente, isso acontece em um único repositório, no mesmo pull request; entre repositórios, como mudanças explicitamente vinculadas e versionadas. Antes de arquivar, vale rodar uma verificação de drift.

## Árvore de decisão: legado, greenfield, conformidade, equipe
O autor supôs de início que a árvore começaria com uma pergunta sobre a ferramenta. As evidências invertem a ordem: primeiro o tipo de mudança, depois conformidade e equipe, e a ferramenta por último. O resultado deve ser registrado como um ADR, um registro curto de decisão de arquitetura.

### Legado e correção de bug
Use um template de correção de bug, um teste de regressão e as regras da constituição. O fluxo completo de funcionalidade é exagero aqui: Böckeler descreveu um pequeno bug que virou 16 critérios de aceitação. O argumento contrário do Kiro é que a correção de bug é o lugar onde o agente quebra código que funcionava, por isso o template tem uma seção de comportamento inalterado.

> ❌ **Anti-padrão — Rodar o fluxo completo de funcionalidade em uma correção de bug:** aplicar o processo completo de especificação, plano e tarefas a um defeito pequeno. **Por que as pessoas fazem isso:** o artigo não diz; o fluxo completo é o padrão na sua família. **Consequência:** o exemplo de Böckeler transformou um pequeno bug em 16 critérios de aceitação. **Correção:** um template de bug com três títulos, um teste de regressão que falha e uma seção explícita de comportamento inalterado.

```javascript
**Bugfix:** <short description>
Current behavior:    WHEN <condition> THEN system <wrong behavior>
Expected:            System SHALL <correct behavior>
Unchanged:           System SHALL CONTINUE TO <behavior that must survive>
Regression test:     <path to the test that fails today>
```

**O que isso faz:**
- A linha WHEN … THEN captura o comportamento errado atual na mesma forma de um requisito, então o bug é declarado como uma condição testável.
- A linha SHALL declara o comportamento esperado como um requisito que a correção precisa satisfazer.
- A linha SHALL CONTINUE TO é a seção que o autor destaca: ela nomeia o comportamento que precisa sobreviver à correção, que é onde agentes costumam quebrar código que funcionava.
- A linha de teste de regressão aponta para um teste que falha hoje, o que liga a especificação a algo executável.

### Funcionalidade em uma base de código existente
Use um delta de especificação por padrão. Com uma aplicação completa, use um preset enxuto e, como recomendação do autor, uma regra da constituição: buscar primeiro a funcionalidade existente. Böckeler descreveu um agente que encontrou classes existentes e mesmo assim as gerou do zero, então a regra mira essa falha.

> ❌ **Anti-padrão — Gerar código novo quando já existe uma classe semelhante:** o agente escreve uma nova implementação sem verificar a base de código. **Por que as pessoas fazem isso:** o artigo não diz por que o agente faz isso. **Consequência:** funcionalidade duplicada, relatada por Böckeler. **Correção:** uma regra da constituição para buscar a funcionalidade existente primeiro.

### Greenfield
Uma aplicação completa com uma constituição faz sentido quando a arquitetura é fluida e a equipe quer gates. Uma equipe do Reddit citada no estudo da CMU e do Google relatou de 2 a 3 vezes mais tokens e cerca de o dobro do tempo. O autor chama isso de uma anedota, não de uma norma, mas é um valor a incluir no orçamento.

### Conformidade
Um arquivo de deltas datados e a atribuição de descobertas a requisitos oferecem uma trilha de auditoria. A imposição continua nos testes e na revisão.

### Tamanho da equipe
Para um desenvolvedor solo ou uma equipe pequena, o autor recomenda o modo de planejamento mais um único arquivo, como recomendação, e não como resultado medido. Para fluxos paralelos e vários componentes, o autor sugere contexto compartilhado para o agente (Stores e Worksets no OpenSpec, em beta) mais uma verificação separada de base desatualizada e conflitos de delta, que esses recursos não oferecem sozinhos. Não foram encontradas comparações de efetividade.

As ferramentas importam em três pontos: na semântica de fusão de deltas, no converge (que apenas lê e acrescenta tarefas) e nos gates embutidos na IDE. O autor descreve essas diferenças como operacionais, não como Markdown intercambiável.

*(Figura do artigo: a árvore de decisão, com o tipo de mudança primeiro, depois conformidade e equipe, e a ferramenta por último.)*

## Resumo: a matriz de escolha
O resumo diz que, um ano atrás, o mapa de quinze frameworks respondia o que existe, e hoje a pergunta mais útil é qual padrão serve à mudança feita nesta semana.

Legenda usada na matriz: medido significa que alguém mediu, docs significa apenas documentação e recomendação significa a conclusão do autor a ser testada.

1. As ferramentas das antigas quinze se dividem em quatro famílias de padrões (recomendação). As diferenças operacionais entre as ferramentas permanecem.
2. Na primeira entrega, as diferenças em tickets mesclados ficam dentro da faixa de ruído (medido: Uvik, 10 tickets por célula). O tipo de mudança diferencia mais o resultado.
3. Correção de bug com teste: um template de três títulos (recomendação; o controle é igual aos braços, medido). Funcionalidade em base de código existente: delta de especificação (recomendação; a vantagem dos braços sobre o controle, medida). Vários repositórios: funcionalidades em beta (docs), sem medições.
4. O custo de manter uma especificação após a segunda mudança não foi comparado nos estudos encontrados. Vincular uma mudança de comportamento a uma mudança de especificação (recomendação) funciona em todas as famílias.
5. Leia os números dos fornecedores pelo protocolo: modelo, versões, orçamento, critérios, ruído.

O autor encerra pedindo aos leitores que rodem o protocolo em três tickets e descrevam o que aconteceu na segunda mudança de requisito.

## 🧠 Modelos mentais
### Especificação como estatuto consolidado
**O modelo:** uma especificação é o estatuto de uma empresa mantido como um único texto consolidado, e a constituição é a carta que toda mudança herda.
**Como aplicar:** antes de acrescentar uma mudança, pergunte quais regras da carta ela precisa herdar e qual parte do texto consolidado ela altera.

### Delta como emenda
**O modelo:** um delta de especificação é uma emenda que lista apenas as disposições acrescentadas, modificadas e revogadas, e o arquivamento a funde no texto consolidado.
**Como aplicar:** quando duas emendas tocam um mesmo requisito, verifique se ambas foram escritas contra a base atual antes de arquivar qualquer uma.

### O padrão segue o tipo de mudança
**O modelo:** o melhor padrão é aquele que se ajusta ao tipo de mudança, e seu comportamento precisa ser verificado na segunda mudança de requisito.
**Como aplicar:** escolha o padrão para o seu tipo de mudança mais frequente e, antes de adotá-lo amplamente, rode contra ele uma mudança de requisito.

### Medição versus promessa
**O modelo:** um número responde a uma pergunta sob uma configuração; uma funcionalidade documentada, mas não medida, é uma promessa, não um resultado.
**Como aplicar:** antes de confiar em um número de fornecedor, verifique o modelo, as versões, o orçamento de tempo, os critérios de aceitação e a faixa de ruído.

## 🔑 Principais conclusões
- Nenhum framework venceu; os resultados medidos do artigo ficam dentro de uma faixa de ruído de cerca de ±10 pontos percentuais na primeira entrega, então o tipo de mudança importa mais do que a ferramenta.
- Para uma correção de bug delimitada com teste que falha, o controle sem especificação mesclou 9 de 10 tickets, tanto quanto as melhores configurações.
- Para uma funcionalidade em uma base de código existente, os braços com especificação mesclaram de 2 a 3 tickets a mais em cada 10 do que o controle, a única vantagem que o artigo chama de medida.
- O custo de manter uma especificação atualizada após uma segunda mudança de requisito não foi medido em nenhum estudo encontrado pelo autor, e é a questão aberta mais importante.
- Arquivos de especificação raramente são editados de novo: 81,5% têm um único commit nos repositórios públicos estudados.
- Deltas sobrepostos preparados sobre uma base desatualizada podem se sobrescrever no arquivamento, mesmo quando os nomes dos cenários são preservados.

## 🏛️ Para Kenji
Esta é a sua interpretação, não a do autor. A ordem de decisão do artigo (tipo de mudança primeiro, conformidade e equipe em segundo, ferramenta por último) se encaixa bem no trabalho de arquitetura, em que a escolha de um padrão é uma decisão a registrar como ADR. A questão que ele levanta para um arquiteto é como uma equipe detectaria o decaimento de especificações em um repositório antes de uma segunda mudança de requisito, já que as medições do artigo não respondem a isso.

## 📚 Plano de leitura
### Do artigo
- [Original article on Medium](https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64) — o texto completo com figuras e fontes citadas; o texto extraído não traz URLs nos links, então as fontes que ele cita são listadas abaixo apenas pelo nome.

### Para aprofundar
- Spec-Driven Development Benchmark 2026 (Paul Francis, Uvik Software) — a principal medição por tipo de mudança que o artigo analisa, com sua faixa de ruído e seu desenho de 50 tickets.
- Thoughtworks Technology Radar vol. 34 (abril) — a posição Assess do Spec Kit e do OpenSpec e o enquadramento de "dois grandes grupos".
- Análise de 2025 de Birgitta Böckeler sobre três ferramentas (Thoughtworks) — os níveis spec-first, spec-anchored e spec-as-source e o achado de não determinismo.
- Agarwal e coautores (CMU e Google), estudo de 2026 — a distinção entre desvio de implementação e decaimento da especificação e os números de 81,5% e 77,6%.
- Piloto de revisores de Garg (InfoQ, setembro de 2026) — revocação versus atribuição ao revisar contra uma especificação aprovada.
- Issues #1246 e #1387 do OpenSpec, conforme citadas no artigo — o problema de deltas sobrepostos e de base desatualizada e a lacuna que ainda resta.
