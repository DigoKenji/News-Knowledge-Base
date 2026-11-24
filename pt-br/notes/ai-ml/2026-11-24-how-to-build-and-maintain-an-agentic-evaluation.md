---
type: Article Summary
title: Como construir e manter uma avaliação agêntica
description: Avaliações agênticas só são confiáveis quando cada componente é construído, validado e compartilhado abertamente, pois cada um deles altera a pontuação.
resource: https://huggingface.co/blog/agentic-evals
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://huggingface.co/blog/agentic-evals
  title: How to build and maintain an agentic evaluation
  author: Avijit Ghosh, Ben Burtenshaw, Irene Solaiman
status: stable
source_url: https://huggingface.co/blog/agentic-evals
source: Hugging Face
source_type: blog
published: '2026-11-23'
domain: ai-ml
concepts:
- agent-harness
- ai-agent
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81dd9b05c464970d8244
lang: pt-br
translation_of: /notes/ai-ml/2026-11-24-how-to-build-and-maintain-an-agentic-evaluation.md
---

## 🎯 Tese central
Uma avaliação agêntica só é confiável quando sua tarefa, seu ambiente, seu agent harness, seu verificador e seus traces são cada um construídos, verificados e compartilhados abertamente, porque qualquer parte do software ao redor pode alterar a pontuação.

## 🧭 Por que o autor escreveu
98.2% das pontuações de benchmarks de modelos são divulgadas por apenas uma parte, e conjuntos de teste retidos tornam os resultados mais difíceis de verificar. Os autores argumentam que compartilhar a infraestrutura, os métodos de avaliação e as evidências, com revisão controlada quando a publicação aberta não é segura, torna as pontuações verificáveis e reutilizáveis.

## Visão geral
O guia cobre as três eras da avaliação (seção 1), um passo a passo de oito etapas (seção 2 e Apêndice A), como os erros dos benchmarks são corrigidos (seção 3) e o que abrir estrategicamente (seção 4).

### Avaliação agêntica
Uma avaliação entrega tarefas a um sistema, verifica suas respostas ou ações e produz uma pontuação. Uma avaliação agêntica pontua o que um agente alterou em seu ambiente, e não apenas o texto que devolveu. O hub do Hugging Face hospeda cerca de 3,500 leaderboards de avaliação e cerca de 17,000 datasets de avaliação, um crescimento de aproximadamente 16X no último ano.

## 1. Introdução: como o design de avaliações mudou
O design das avaliações passou por três formas à medida que os modelos se tornaram mais capazes: pergunta e resposta, agêntica, e equipes ou enxames (Figura 1). O artigo foca nas avaliações agênticas porque elas dominam os anúncios de lançamento de modelos e mostram claramente como as escolhas de design afetam as pontuações. As avaliações de equipes e enxames estão fora do escopo.

### Benchmark de pergunta e resposta
MMLU (2020) e GPQA (2023) verificam uma resposta contra um gabarito, medindo conhecimento e raciocínio pelas respostas que o modelo produz. A pontuação não exige considerar o ambiente em que o sistema foi executado.

### Benchmark agêntico
SWE-bench e WebArena pedem que um sistema resolva problemas de software ou complete tarefas em sites, então a configuração precisa considerar o ambiente e as alterações que o sistema faz. Em uma auditoria do Terminal-Bench, os pesquisadores mantiveram o modelo fixo e alteraram apenas o agent harness; a acurácia variou em até 29.9 pontos percentuais.

## A anatomia de uma avaliação agêntica
A Figura 2 mostra as partes. Cada uma pode ser publicada, inspecionada e reutilizada separadamente.

### Tarefa
A tarefa dá ao agente sua instrução. Suas unidades, definições de parâmetros e formato de saída precisam ser explícitos, ou a pontuação mede em parte adivinhação.

### Agent harness
O agent harness envolve o modelo e suas ferramentas, permitindo que ele trabalhe dentro de um ambiente fixado e transformando as respostas do modelo em ações, como comandos de shell.

### Rollout
Um rollout é uma tentativa de uma tarefa do início ao fim. O agente pode executar vários comandos, inspecionar suas saídas e tentar de novo antes de parar.

### Evaluation harness
O evaluation harness executa vários rollouts por tarefa, coleta os traces e agrega as pontuações.
### Verificador
Quando um rollout termina, o verificador avalia o estado final do ambiente. Um verificador sólido avalia a saída salva, não a afirmação do agente de que terminou.

### Trace
Um trace é o registro completo de um rollout: mensagens, chamadas de ferramentas e o que cada comando devolveu. Traces e pontuações juntos formam a evidência publicada.

## Por que as decisões de publicação de benchmarks importam
A Artificial Analysis afirma que 60% do seu Intelligence Index v5, anunciado em outubro de 2026, depende de conjuntos de teste privados ou de respostas retidas. O Google DeepMind fez um piloto de avaliações duplo-cegas.

### Conjunto de teste retido
Itens de teste mantidos privados para que os modelos não possam ser treinados ou ajustados com eles. A privacidade reduz a contaminação, mas não garante qualidade, e reter o material de apoio dificulta a revisão por especialistas.

### Avaliação duplo-cega
Um enclave seguro oculta os prompts de teste de um avaliador externo do provedor do modelo, e oculta os pesos do modelo do avaliador. Isso reduz a contaminação e mantém possível a avaliação externa.

## 2. Construindo uma avaliação: o que reutilizar, adaptar e validar
Os autores constroem uma avaliação agêntica para um laboratório de física, adaptando uma tarefa aberta para que a maior parte do esforço vá para a expertise de domínio e para a validação. As oito etapas são: definir a afirmação, encontrar uma tarefa, executá-la sem alterações, adaptá-la, verificar a avaliação, ler os traces, reportar a pontuação e comparar modelos.

## Método
### Afirmação
Uma afirmação é uma declaração simples do que o agente consegue fazer e que a avaliação sustenta. Esta é deliberadamente estreita: ajustar os picos Raman especificados, calcular a razão D-para-G solicitada (também chamada de razão de defeito, a intensidade de um pico em relação a outro, que pode indicar defeitos) e salvar resultados precisos de forma consistente. Revisar um relatório de laboratório completo exigiria mais habilidades e mais evidências.

**Tarefa inicial e linha de base.** Os autores reaproveitaram o formato de tarefa do Harbor e a tarefa raman-fitting do Terminal-Bench. O Harbor é um evaluation harness de código aberto; o Terminal-Bench é um benchmark aberto de tarefas concluídas em um terminal. Apenas o agent harness não exigiu trabalho novo.

**Adaptação.** O raman-lab tem oito espectros sintéticos com parâmetros conhecidos e níveis variados de ruído e de defeitos. Ele ajusta três picos em vez de dois e reporta uma razão de defeito. Um agente ajudou a gerar os espectros; os autores os revisaram manualmente e escreveram novas instruções, ajustes de referência e verificações de saída.

### Conjunto de desenvolvimento
Tarefas usadas durante a construção e a depuração de uma avaliação, em contraste com um conjunto de teste retido para as pontuações finais. Oito tarefas são poucas para generalizar; a cobertura deve seguir a variação das medições reais.

**Verificações.** Antes de executar mais modelos, os autores testaram a própria avaliação com três controles: uma solução de referência, um agente que não faz nada e um atalho.

### Oráculo
O oráculo executa a solução correta conhecida e deve passar em todas as tarefas. Se ele falhar, a falha está na tarefa, no ambiente ou no verificador, e as falhas dos modelos não podem ser interpretadas até que isso seja corrigido.

### Agente no-op
Um agente que não faz nada e deve falhar em todas as tarefas. Uma prova em branco não deveria passar.

### Atalho
O atalho pulou o ajuste: leu o ponto mais alto de cada pico nos dados, chutou as larguras e dividiu a altura do pico D pela altura do pico G. Ele devolveu a razão de defeito esperada nas oito tarefas, então um avaliador que checasse apenas a razão daria nota máxima. Os autores acrescentaram verificações em cada pico ajustado, e o verificador final o rejeita.

> ❌ **Antipadrão — avaliação apenas pela razão:** o verificador confere apenas o número final. **Por que as pessoas fazem isso:** a resposta esperada é fácil de declarar e comparar. **Consequência:** um método que pula a habilidade exigida recebe nota máxima (aqui, 8 de 8 tarefas). **Correção:** verifique o que a saída demonstra, como cada pico ajustado.

## Resultados
**Linha de base.** Na tarefa original, o gpt-oss-120b passou em 0 de 3 rollouts. Dois declararam a conclusão antes de observar a saída relevante; o terceiro não conseguiu ler o arquivo de dados, que usa vírgulas decimais que a solução de referência converte antes de carregar. A pontuação sozinha não conseguia distinguir isso de um ajuste incorreto. Os traces conseguiam.

### Conclusão precoce
O agente declara a tarefa concluída antes de observar qualquer saída de comando, aqui chamando `mark_task_complete` na primeira resposta. O artigo usa isso como diagnóstico: a maioria das falhas desta execução veio de parar cedo.

**Traces e pontuação.** Nas oito tarefas adaptadas, com três rollouts cada, a configuração passou em 11 de 24. Em 19 rollouts o modelo concluiu cedo; 11 desses não produziram arquivo de resultados, muitas vezes porque um script falhou por falta de NumPy ou SciPy. Das 13 falhas, apenas duas falharam na verificação do ajuste (Figura 7). O pass@3 foi 7/8 e o pass^3 foi 1/8. A taxa média de aprovação foi 0.46, com erro padrão agrupado de cerca de 0.11. Uma repetição da execução passou em 10 de 24.

> ❌ **Antipadrão — ler apenas a pontuação:** tratar uma contagem baixa de aprovações como defeito de ajuste. **Por que as pessoas fazem isso:** a pontuação é o único resumo que a maioria dos harnesses mostra. **Consequência:** a maioria das falhas aqui foi de paradas precoces e pacotes ausentes, não de ajustes errados. **Correção:** agrupe os rollouts com falha por causa, a partir dos traces: parsing, execução de ferramenta, parada precoce ou avaliação.

## Comparação de modelos
Os autores compararam seis modelos nas mesmas oito tarefas. Vários provedores servem um mesmo modelo de pesos abertos com quantização e limites de contexto diferentes, então fixaram um provedor por modelo. Com o DeepInfra fixado, o gpt-oss-120b passou em 13 de 24 rollouts, contra 11 de 24 no provedor padrão. Apenas o gpt-oss-120b declarou a conclusão na primeira resposta, em 22 de 24 rollouts. Qwen3.8-27B, DeepSeek-V4.1-Flash e MiniMax-M3.1-Flash passaram em todos os 24 rollouts, então essas tarefas não conseguem ranqueá-los.

### Quantização
Armazenar os pesos do modelo com precisão numérica menor para rodar mais rápido e mais barato, o que pode alterar as saídas um pouco. Dois provedores servindo o mesmo modelo podem, portanto, pontuar de forma diferente.

### Fixação de versão (pinning)
Fixar versões exatas de software, imagens ou provedores de modelo para que uma execução possa ser repetida. O provedor fixado deve ser registrado junto de cada pontuação.

> ❌ **Antipadrão — provedor de modelo não fixado:** deixar o roteador escolher o provedor. **Por que as pessoas fazem isso:** é o padrão e não exige reflexão. **Consequência:** o mesmo modelo fez 11 de 24 no provedor padrão e 13 de 24 no DeepInfra. **Correção:** fixe o provedor (por exemplo, `:deepinfra`) e registre-o junto da pontuação.

## 3. Lições amargas: quando um benchmark está errado
O atalho devolveu a resposta certa sem ajustar um espectro. Os autores o descobriram tentando quebrar o avaliador com uma solução que sabiam estar incompleta. Como um aluno que copia o resultado final sem a resolução, ele estava certo pelo motivo errado. A lição: um método válido e mais rápido deve passar, e um método que pula uma habilidade exigida não deve. Uma revisão de benchmarks agênticos concluiu que falhas na configuração das tarefas ou no design de recompensas podem alterar o desempenho medido em até 100% em termos relativos.

### Tolerância
A margem que um verificador permite ao comparar um resultado com uma referência. Se for muito apertada, rejeita ajustes alternativos válidos; se for muito frouxa, aceita trabalho que não atinge o ponto da tarefa. Especialistas de domínio ajudam a defini-la.

## A revisão pública é essencial para corrigir benchmarks
A revisão pública expõe erros que os construtores não perceberam. O Terminal-Bench 2.1 corrigiu 28 de suas 89 tarefas após relatos de usuários, incluindo uma cujos testes esperavam Spark SQL enquanto as instruções pediam PostgreSQL; o ranking dos agentes permaneceu estável. Em julho de 2026, a OpenAI estimou que cerca de 30% das tarefas do SWE-Bench Pro estavam quebradas e retirou sua recomendação de adotá-lo.

### Auditoria externa
Uma verificação independente das tarefas e dos testes de um benchmark, feita por revisores de fora de seus construtores. Seu custo depende do que os revisores conseguem ler. O CritPt mantém as soluções de referência privadas, então seus auditores físicos precisaram resolver os problemas e encontraram erros em 21 dos 56 desafios que revisaram. A auditoria da OpenAI sobre o SWE-bench Verified, trabalhando a partir de tarefas e testes legíveis, encontrou problemas materiais em 59.4% dos 138 problemas examinados.

## Verifique a afirmação, além do código
Uma pontuação consistente ainda pode medir a coisa errada. Uma revisão de 445 benchmarks de LLM feita por 29 revisores especialistas (Measuring what Matters) encontrou padrões recorrentes que minam a validade de construto. Ajustar espectros sintéticos não mostra que um agente consegue revisar relatórios de laboratório; isso pode exigir também reconhecer uma medição ruim ou pedir ajuda. Harder Than the Task estende isso ao trabalho organizacional, em que menos de um quarto dos benchmarks proeminentes afirma cobrir autoridade ou dependências. Se esses fatores importam para a afirmação, eles devem fazer parte da avaliação; se não, diga que permanecem não testados.

### Validade de construto
Se as tarefas e as métricas de um benchmark sustentam as afirmações feitas sobre a capacidade que ele pretende medir. Uma pontuação alta e consistente não a estabelece.

Cada erro merece um registro curto: a premissa que falhou, como foi encontrada, o que mudou, as versões afetadas e se as pontuações anteriores continuam comparáveis.

## Contaminação versus saturação
A auditoria do SWE-bench Verified também encontrou contaminação: todo modelo de fronteira testado conseguia reproduzir o patch de referência ou detalhes literais do problema em algumas tarefas, porque repositórios públicos e uso amplo expuseram as tarefas ao treinamento.

### Contaminação
Perguntas de teste ou soluções que aparecem nos dados de treinamento, de modo que uma pontuação pode refletir memória e não habilidade.

### Saturação
O ponto em que a maioria dos modelos atinge pontuações máximas em um benchmark, de modo que ele deixa de separá-los. Um estudo de 60 benchmarks de modelos de linguagem encontrou saturação semelhante em benchmarks públicos e privados; o que se correlacionou com menor saturação foram a curadoria por especialistas e um conjunto de teste maior. Esconder os dados, portanto, não impede sozinho a saturação.

> ❌ **Antipadrão — esconder o conjunto de teste como solução:** manter os itens privados e parar aí. **Por que as pessoas fazem isso:** a contaminação é visível e esconder é fácil. **Consequência:** benchmarks ocultos saturaram de forma muito parecida com os públicos. **Correção:** combine a proteção com curadoria por especialistas, um conjunto de teste maior e um plano de renovação.

## 4. Abrir o código de uma avaliação de forma estratégica
Os criadores podem liberar parte por parte: proteger tarefas de teste ativas enquanto compartilham infraestrutura, métodos de avaliação e exemplos de referência. O ARC-AGI-2 publica um conjunto de treinamento e um conjunto de avaliação de 120 tarefas, e retém conjuntos semiprivados e privados de 120 tarefas cada. O AILuminate mantém os prompts oficiais confidenciais, mas publica seus métodos e sua taxonomia. Nenhum revisor público pode verificar um item protegido, então o que permanece fechado precisa de sua própria via de revisão.

### String canário
Um marcador único nos arquivos de benchmark que permite aos treinadores de modelos encontrá-los e excluí-los dos dados de treinamento. O Terminal-Bench marca seus arquivos de tarefa dessa forma.

### Dataset com acesso controlado (gated)
Um dataset do Hugging Face que só pode ser baixado depois que o dono aprova uma solicitação de acesso. O artigo sugere um para os itens de teste ativos, com aprovação manual de cada solicitação.

A Tabela 1 apresenta, por componente, o que abrir, onde ele fica no Hub e o que pode permanecer protegido. A versão condensada:

| Parte | Pode ser aberto | Pode permanecer protegido |
|---|---|---|
| Tarefa | Especificações, categorias, métodos de construção, split de desenvolvimento público | Itens e respostas de teste ativos (com acesso controlado); itens aposentados quando permitido |
| Ambiente | Definição do contêiner, arquivos de build, dependências fixadas, fixtures sintéticas | Dados privados, credenciais, ativos licenciados, detalhes perigosos |
| Agent harness e ferramentas | Código do loop, interfaces de ferramentas, adaptadores, permissões, orçamentos | Segredos, serviços privados, código sensível de ferramentas; forneça substitutos seguros |
| Rollout e trace | Traces de tarefas públicas, trechos revisados, código de análise | Traces que expõem tarefas, dados privados ou comportamentos restritos |
| Solução de referência | Método e código da solução para exemplos públicos | Soluções que revelam itens protegidos; revisores autorizados as verificam |
| Evaluation harness | Orquestração, configuração do modelo, logs, agregação, registros de versão | Credenciais e configuração que identifica itens; um registro revisável da execução restrita |
| Verificador | Rubricas, código do avaliador, tolerâncias, exemplos resolvidos | Respostas de referência e lógica que revelam itens protegidos |
| Afirmações e design de tarefas | Uso pretendido, justificativa da seleção, premissas, evidências de validação | Material de origem sensível (pode exigir redação) |
| Resultados | Pontuações, incerteza, custos, exclusões, limitações, configuração | Detalhes que revelam tarefas; publique resumos seguros |
| Correções | Histórico de correções nomeando as versões afetadas | Exemplo protegido por trás de uma correção |

No Hub, tarefa e ambiente vão em um repositório de dataset com um dataset card; os resultados são registrados como um Hub Benchmark, com as pontuações na pasta .eval_results/ de cada repositório de modelo, vinculadas ao dataset de traces. A versão do raman-lab dos autores segue esse padrão. Uma avaliação clínica poderia, da mesma forma, abrir seu harness e seu framework de avaliação enquanto mantém os registros de pacientes sob acesso controlado. Cada restrição deve vir com uma condição para ser revista.

## Checklist estratégico para abrir o código de uma avaliação
Antes de liberar ou aposentar uma avaliação, verifique se outro construtor consegue:

> 📋 **Checklist — Abertura estratégica de código**
> - [ ] Identificar o que ela mede, seu uso pretendido e as condições que deixa de fora.
> - [ ] Encontrar os artefatos reutilizáveis, suas licenças e versões, e uma forma de reportar problemas.
> - [ ] Ver como um agente que não faz nada pontua e, quando possível, como um especialista humano pontua.
> - [ ] Ler cada pontuação com sua incerteza e com orientação sobre o que as falhas conhecidas significam para ela.
> - [ ] Saber quais falhas foram encontradas, como foram tratadas e o quanto podem alterar os resultados.
> - [ ] Saber como e quando as tarefas serão renovadas para limitar a contaminação e o overfitting.

## 5. Conclusão: dando à avaliação uma vida útil depois
Benchmarks são construídos, adotados, otimizados e, por fim, deixam de separar os sistemas que foram feitos para comparar. O estudo de saturação classificou 29 dos seus 60 benchmarks como altamente saturados, e saturação não é uma falha em si. Quando tarefas, ambientes, verificadores e traces permanecem abertos, cada um continua útil depois que o leaderboard deixa de funcionar: uma tarefa resolvida vira uma verificação de regressão, e seu verificador pode servir de base para um sucessor mais difícil. As perguntas do MMLU viraram o MMLU-Redux, e o Terminal-Bench 2.0 virou o 2.1.

### Ciclo de vida do benchmark
As etapas pelas quais um benchmark passa, da construção à saturação. Liberações abertas continuam úteis em todas as etapas, enquanto uma pontuação publicada isoladamente serve para pouco além de ler o leaderboard.

> ❌ **Antipadrão — publicar apenas a pontuação:** divulgar o número do leaderboard sem suas tarefas, seu ambiente ou seus traces. **Por que as pessoas fazem isso:** a pontuação é a manchete, e empacotar o resto dá trabalho. **Consequência:** futuros construtores não conseguem reexecutar, adaptar ou verificar. **Correção:** publique tarefas, ambientes, verificadores e traces junto da pontuação.

## Apêndice A. Exemplo prático: implantação de um agente em um laboratório de física
O laboratório quer um agente que ajuste picos em um espectro Raman. O Terminal-Bench 2 já inclui o raman-fitting, que ajusta uma curva lorentziana a dois picos e salva oito parâmetros em /app/results.json. Esse é um ponto de partida executável para uma avaliação de laboratório mais estreita. A Tabela 2 lista o que cada parte oferece e o que verificar antes de reutilizar.

| Parte | No raman-fitting | Verificar antes de reutilizar |
|---|---|---|
| Tarefa | Dois ajustes de picos salvos em /app/results.json | Unidades, definições de parâmetros e formato de saída são explícitos? |
| Ambiente | Imagem de contêiner fixada com o espectro; limites de recursos em task.toml | Quais pacotes o agente precisa instalar? Recursos e permissões são adequados? |
| Agent harness e ferramentas | O Terminus-2 conecta o modelo a comandos de shell | O agente consegue inspecionar o que seus comandos produziram? |
| Rollout e trace | Uma tentativa, em agent/trajectory.json | O registro mostra o que aconteceu antes de o agente parar? |
| Solução de referência | solution/solve.sh executa o ajuste conhecido | Ela passa no ambiente fornecido? |
| Evaluation harness | O Harbor executa rollouts e coleta pontuações e traces | Versões e configurações de execução são registradas? |
| Verificador | tests/test.sh confere oito valores ajustados contra tolerâncias | Ele avalia o arquivo salvo e aceita ajustes alternativos válidos? |

### Espectroscopia Raman
A espectroscopia Raman faz um laser incidir sobre uma amostra e mede as mudanças na frequência da luz espalhada. Os picos resultantes revelam propriedades do material.

### Picos D, G e 2D do grafeno
No grafeno, uma folha de carbono com espessura de um átomo, os picos G e 2D próximos de 1580 e 2700 cm⁻¹ identificam o número de camadas. O pico D aparece quando a rede está desordenada, e sua intensidade em relação ao G pode indicar defeitos.

### Lorentziana
Uma curva matemática comumente usada para modelar a forma de um pico espectral.
## Etapa 1. Defina a afirmação
Uma afirmação é uma declaração simples do que o agente consegue fazer. Divida-a em habilidades verificáveis e peça a especialistas de domínio que escolham níveis de ruído, tolerâncias e softwares realistas.

## Etapa 2. Encontre uma tarefa inicial
Procure nos tipos de dataset Benchmark e Environment do Hugging Face Hub, e inspecione as instruções, o ambiente, a solução de referência e os testes de cada candidata. Mesmo uma tarefa que os modelos já resolvem pode fornecer infraestrutura útil.

## Etapa 3. Execute a tarefa sem alterações
Execute a solução de referência antes de qualquer modelo; se ela falhar, corrija primeiro a tarefa, o ambiente ou o verificador. Os autores rodaram o Harbor dentro de um HF Job, com cada rollout em seu próprio HF Sandbox, e com os Inference Providers fornecendo as respostas do modelo.

### HF Sandbox
Um ambiente isolado do Hugging Face onde um único rollout executa seus comandos e armazena seus arquivos.

### Inference Providers
Um serviço do Hugging Face que roteia requisições de modelos para provedores terceiros por meio de uma única API, chamada de roteador.

```bash
harbor run -t terminal-bench/raman-fitting \
  -a oracle -e hf-sandbox
OPENAI_API_KEY="$HF_TOKEN" harbor run \
  -t terminal-bench/raman-fitting -k 3 -e hf-sandbox \
  -a terminus-2 -m openai/openai/gpt-oss-120b \
  --ak api_base=https://router.huggingface.co/v1
```

**O que isto faz:**
- O primeiro comando executa o oracle, a solução conhecida, para confirmar que a tarefa é solucionável no sandbox.
- O segundo executa três rollouts (`-k 3`) do agente de terminal Terminus-2 com o gpt-oss-120b.
- `-t` nomeia a tarefa, `-a` o agente, `-e` o ambiente, `-m` o modelo e `-k` os rollouts por tarefa.
- Definir `OPENAI_API_KEY` com o token do Hugging Face autentica as chamadas ao roteador.

## Etapa 4. Adapte a tarefa
O instruction.md original pede x0, gamma, amplitude e offset sem defini-los nem às suas unidades. A solução de referência os trata como centro do pico, meia largura à meia altura, altura acima do fundo e nível de fundo, e converte o eixo x de nanômetros para deslocamento Raman. As instruções adaptadas declaram essas escolhas, e a razão de defeito deve dizer se intensidade significa altura ou área integrada.

> 🔎 Contexto adicional: a meia largura à meia altura é a distância entre o centro de um pico e o ponto onde sua altura cai para a metade do máximo. O deslocamento Raman é a diferença de frequência entre a luz do laser e a luz espalhada, medida em cm⁻¹.

### Dados sintéticos
Dados gerados a partir de parâmetros conhecidos, de modo que a resposta correta é conhecida de antemão. Os autores geraram os espectros do raman-lab dessa forma e depois revisaram se os exemplos representavam o problema pretendido.

O formato compartilhado pode permanecer como está enquanto os dados e as verificações mudam. Este comando cria um esqueleto de tarefa:

```bash
harbor task init my-org/task-01 -p datasets/my-evals
```

**O que isto faz:**
- Cria um novo esqueleto de tarefa em `datasets/my-evals`.
- Mantém o formato compartilhado de tarefa fixo, de modo que apenas os dados e as verificações mudam entre as tarefas.

> 📋 **Checklist — Regras de design do verificador**
> - [ ] Avalie o que o agente produziu: inspecione a saída salva, não a afirmação de que terminou. O τ-bench faz isso comparando o estado final do banco de dados com um estado-objetivo anotado.
> - [ ] Declare todo fato verificado nas instruções, incluindo unidades, convenções de parâmetros, tolerâncias e campos obrigatórios.
> - [ ] Gere dados sintéticos a partir de parâmetros conhecidos para que a resposta de referência esteja disponível, e depois revise se os exemplos representam o problema pretendido.

## Etapa 5. Verifique a avaliação
Antes de qualquer execução de modelo, teste as tarefas adaptadas em `datasets/my-evals`:

```bash
harbor run -p datasets/my-evals -a oracle -e hf-sandbox
harbor run -p datasets/my-evals -a nop -e hf-sandbox
```

**O que isto faz:** o primeiro comando executa o oracle, que deve passar em todas as tarefas; o segundo executa o agente no-op (`nop`), que deve falhar em todas as tarefas.

> 📋 **Checklist — Validar a avaliação antes das execuções de modelos**
> - [ ] O oracle deve passar em todas as tarefas, ou seja, a solução conhecida precisa funcionar no ambiente fornecido.
> - [ ] O agente no-op não faz nada, então deve falhar em todas as tarefas. Uma prova em branco não deveria passar. Uma revisão de benchmarks agênticos encontrou um que contava respostas vazias como acertos.
> - [ ] Um atalho que pula trabalho exigido deve falhar. Uma forma de testar isso é com uma solução deliberadamente preguiçosa, como uma que devolve uma resposta plausível sem fazer o trabalho, para ver se o verificador a aceita. Teste também respostas erradas plausíveis e métodos corretos alternativos, para pegar tanto falsas aprovações quanto falsas reprovações.

## Etapa 6. Execute e leia os traces
O Harbor registra mensagens, chamadas de ferramentas e observações de cada rollout em agent/trajectory.json. Agrupe as falhas por causa (parsing, execução de ferramenta, parada precoce ou avaliação). A falta de NumPy ou SciPy foi uma falha inicial comum; pré-instalá-los mudaria a condição, então mantenha modelo, harness, tarefas e número de rollouts fixos ao comparar. O Terminal-Bench 2.1 consertou nove tarefas cujas dependências externas haviam mudado.

### ATIF
O Agent Trajectory Interchange Format, um formato compartilhado para as mensagens, chamadas de ferramentas e observações de um agente. O Harbor o grava para cada rollout.

## Etapa 7. Reporte a pontuação
### pass@k
A fração de tarefas em que pelo menos um dos k rollouts tem sucesso. O pass@3 foi 7/8: sete tarefas tiveram sucesso pelo menos uma vez. Isso serve para trabalhos em que uma pessoa revisa a saída e novas tentativas são aceitáveis.

### pass^k
A fração de tarefas em que todos os k rollouts têm sucesso. O pass^3 foi 1/8. O processamento não supervisionado exige consistência, então esta medida importa mais.

### Erro padrão agrupado
Rollouts do mesmo espectro compartilham a sua dificuldade, então não são independentes. Agrupar por tarefa dá a margem honesta: uma média de 0.46 com erro padrão de cerca de 0.11.

> ❌ **Antipadrão — reportar pass@k para trabalho não supervisionado:** usar como manchete o número de pelo menos uma vez. **Por que as pessoas fazem isso:** é o número mais alto e mais lisonjeiro. **Consequência:** uma tarefa que tem sucesso uma vez em cada três conta como resolvida. **Correção:** reporte o pass^k, ou a média com seu erro padrão agrupado, quando nenhuma pessoa revisa a saída.

Reporte tokens, tempo decorrido e chamadas de ferramentas junto da pontuação, para que os leitores possam comparar configurações que chegam a resultados semelhantes com custos diferentes.

## Etapa 8. Compare modelos
Uma única execução que falha não permite saber se a falha está no modelo, no harness ou na avaliação. Quando vários modelos falham na mesma tarefa, inspecione as instruções, o ambiente e o verificador em busca de uma causa comum. Quando um modelo falha de forma peculiar, compare seus traces com as tentativas bem-sucedidas. Cada novo modelo é mais um valor para `-m`, e um sufixo como `:deepinfra` fixa o provedor.

```bash
OPENAI_API_KEY="$HF_TOKEN" harbor run -p datasets/my-evals -k 3 \
  -e hf-sandbox -a terminus-2 -m openai/Qwen/Qwen3.8-27B:deepinfra \
  --ak api_base=https://router.huggingface.co/v1
```

**O que isto faz:** executa três rollouts por tarefa (`-k 3`) do Terminus-2 sobre `datasets/my-evals`, com o Qwen3.8-27B fixado no provedor DeepInfra pelo sufixo `:deepinfra`.

A inferência para os seis modelos custou menos de US$ 5 aos preços de tabela. O rollout mediano do raman-lab usou cerca de 6,000 tokens com o gpt-oss-120b e cerca de 90,000 com o MiniMax-M3.1-Flash.

## Como reaproveitamos componentes abertos existentes
O reaproveitamento cobriu o formato de tarefa, o executor do Harbor, o Terminus-2 e as convenções `solution/solve.sh` e `tests/test.sh`. O trabalho novo foi a afirmação mais estreita, o gerador de espectros, os ajustes de referência e as verificações de ajuste e de atalho.

## Referências
As cerca de 30 referências do artigo não são reproduzidas; o plano de leitura mantém as mais ligadas ao argumento.

## Glossário
Termos não definidos acima: **limite de contexto** (context limit: o maior volume de texto, em tokens, que um modelo lê em uma única requisição); **imagem de contêiner** (container image: um pacote de software fixo, de modo que cada rollout começa do mesmo ambiente); **registro de execução** (run record: a tarefa, o ambiente, a configuração do modelo e as versões de código exatas por trás de uma pontuação).

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor: para trabalho de arquitetura, uma avaliação é um sistema com suas próprias dependências, versões e histórico de correções, então precisa da mesma governança dos agentes que ela julga. O registro de execução, o harness fixado e o log de correções são artefatos que uma revisão de arquitetura ou um processo de governança de IA poderia exigir. Pergunta: quais dos nossos testes de aceitação de agentes um agente no-op ou um atalho passaria?

## 📚 Plano de leitura
### Do artigo
- [Documentação do Harbor](https://docs.harborframework.com/) — o evaluation harness usado em todo o artigo, incluindo seu exportador de traces.
- [Notas de lançamento do Terminal-Bench 2.1](https://www.tbench.ai/news/terminal-bench-2-1) — como os relatos de usuários corrigiram 28 tarefas e uma divergência entre Spark SQL e PostgreSQL.
- [Why we no longer evaluate SWE-bench Verified](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) — a auditoria por trás do número de 59.4% e das descobertas de contaminação.
- [Measuring what Matters](https://huggingface.co/papers/2511.04703) — a revisão de 445 benchmarks por trás do argumento sobre validade de construto.
- [When AI Benchmarks Plateau](https://huggingface.co/papers/2602.16763) — o estudo de saturação de 60 benchmarks.
- [Harder Than the Task](https://arxiv.org/abs/2611.03494) — por que a dificuldade organizacional está ausente da maioria dos benchmarks.
- [Piloting the world's first double-blind AI evaluations](https://deepmind.google/blog/piloting-the-worlds-first-double-blind-ai-evaluations/) — o piloto com enclave seguro.
- [τ-bench](https://huggingface.co/papers/2406.12045) — a comparação com o estado-objetivo, que o artigo usa como modelo para avaliar saídas.
### Para aprofundar
- Erros padrão agrupados para pontuações baseadas em rollouts — como evitar superestimar a confiança quando há poucas tarefas.

## 🔗 Conceitos
- [agent-harness](/pt-br/concepts/agent-harness.md)
- [ai-agent](/pt-br/concepts/ai-agent.md)
