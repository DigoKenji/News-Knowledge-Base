---
type: Article Summary
title: Testando a resiliência dos seus agentes no Amazon Bedrock AgentCore
description: Injetar falhas em ferramentas de um agente em execução mostra se ele falha de forma segura ou inventa respostas, e a observabilidade precisa capturar os casos silenciosos.
resource: https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore
tags:
- ai-ml
- well-architected
- llm-as-a-judge
- silent-failure
- chaos-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore
  title: Resilience testing your agents on Amazon Bedrock AgentCore
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore
source: AWS Architecture
source_type: blog
published: '2026-11-18'
domain: ai-ml
concepts:
- well-architected
- llm-as-a-judge
- silent-failure
- chaos-engineering
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d81c2a6abe0f5f75377a5
lang: pt-br
translation_of: /notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md
---

## 🎯 Tese central
Injetar falhas nas ferramentas de um agente em execução mostra se ele falha de forma segura ou responde com confiança a partir de dados ruins. Como falhas silenciosas e graciosas são invisíveis ao monitoramento de infraestrutura, a saída do próprio agente precisa ser validada.

## 🧭 Por que o autor escreveu
As equipes testam se os serviços sobrevivem a falhas de infraestrutura, mas raramente testam o que um agente de IA faz quando suas ferramentas dão timeout ou retornam dados errados. O artigo parte do fato de que chamadas de ferramenta, redes e serviços upstream falham em produção, e que um agente não testado opera com suposições. Se a tese se sustenta, um dashboard de disponibilidade com aparência saudável pode esconder um agente que apresenta respostas corrompidas como fatos.

## Introdução (sem título na fonte)
O artigo abre com uma pergunta: quando as ferramentas de um agente começam a falhar, ele tenta de novo, usa um plano B ou admite que não consegue terminar? Ele alerta que os piores resultados são uma resposta confiante construída sobre dados corrompidos, um erro bruto mostrado ao usuário ou uma falha silenciosa sem alerta para a operação. O experimento usa o AWS Fault Injection Service (AWS FIS) para testar como um agente Strands lida com falhas no nível das ferramentas antes que uma degradação real aconteça. O objetivo não é verificar se as ferramentas funcionam. É descobrir o que o agente faz quando uma ferramenta falha ou retorna dados incorretos, e se é possível detectar isso quando acontece. O artigo define três objetivos de aprendizado:
- Estruturar uma hipótese de resiliência mensurável para um agente de IA.
- Injetar falhas no nível das ferramentas (resultados de ferramenta cancelados ou corrompidos) em um agente Strands em execução no Amazon Bedrock AgentCore, usando AWS FIS e AWS Systems Manager (SSM).
- Construir a observabilidade para distinguir uma falha segura de uma falha silenciosa.

### Falhas no nível da ferramenta
Uma falha no nível da ferramenta afeta uma chamada de ferramenta dentro do agente, e não a rede nem a infraestrutura por baixo. O artigo distingue dois tipos: uma chamada cancelada que retorna erro, e uma chamada que tem sucesso mas traz dados corrompidos. Testar nesse nível mostra como o raciocínio do agente reage quando uma de suas entradas não é confiável. Isso é diferente de testar se as próprias ferramentas funcionam.

### Falha silenciosa
Uma falha silenciosa é aquela em que o sistema continua devolvendo respostas normais enquanto a resposta está errada ou incompleta. A principal preocupação do artigo é justamente essa classe de falha, porque nada no caminho da requisição parece quebrado. Ele trata a diferença entre uma falha segura e uma silenciosa como o que deve ser medido.

## Visão geral da solução
Neste experimento, o AWS FIS executa uma única ação de Automation do SSM que liga a injeção de falhas, mantém por uma duração definida e a desliga. As falhas são injetadas dentro do agente, logo antes ou logo depois de uma chamada de ferramenta, de modo que o comportamento do próprio agente é testado. O agente continua atendendo tráfego durante todo o processo, e nada é reimplantado para iniciar ou parar uma execução.

Figura 1: O AWS FIS executa uma única ação de Automation do SSM, que grava /chaos/{runtime_id}/ no Parameter Store. O controlador do agente lê os parâmetros do SSM a cada invocação e injeta as falhas configuradas.

O agente de exemplo é baseado no pokemon-team-advisor, amostra pública do repositório strands-agents/samples. Ele tem duas ferramentas, get_pokemon e get_move, que retornam JSON estruturado.

### Controle grosso e controle fino
O artigo usa dois controles para separar a implantação do código de falhas da sua ativação. O controle grosso responde "o chaos está carregado?" e é definido por uma linha no construtor do agente, plugins=chaos_plugins(). O controle fino responde "o chaos está ativo agora?" e é lido a partir de valores em /chaos/{runtime_id}/ no SSM Parameter Store. Esses valores são lidos a cada invocação, mas ficam em cache por 10 segundos usando um time to live (TTL). Cada invocação pula a injeção de falhas até que o SSM retorne active=true.

> 🔎 Contexto adicionado: o AWS Systems Manager Parameter Store é um armazenamento chave-valor da AWS para valores de configuração. Aqui ele é o interruptor que o AWS FIS liga e desliga durante uma execução.

### ChaosPlugin do Strands Evals
O SDK Strands Evals já oferece um ChaosPlugin para testes de caos offline, documentado no guia de chaos testing do Strands. A contribuição do artigo é a capacidade de executar as mesmas falhas contra um runtime AgentCore implantado, atendendo tráfego real, com o AWS FIS como controlador. Os artefatos do experimento estão na biblioteca de templates do FIS.

## Duas famílias de falhas
As falhas se dividem em duas famílias, e o artigo trata a segunda como a mais perigosa.

### Falhas de pré-hook
Falhas de pré-hook cancelam a chamada de ferramenta antes de ela executar e devolvem um erro ao agente. Os efeitos são timeout, network_error, execution_error e validation_error. Elas testam se o agente percebe que a chamada falhou e então tenta de novo, usa um plano B ou reconhece a falha.

### Falhas de pós-hook
Falhas de pós-hook deixam a ferramenta executar e depois corrompem sua resposta. Os efeitos são truncate_fields, remove_fields e corrupt_values. O ponto do artigo é que o agente pode receber uma chamada de ferramenta bem-sucedida carregando dados ruins e responder ao usuário com confiança, sem jamais dizer que os dados estavam degradados.

## Defina sua hipótese
Antes de rodar o experimento, escreva o que você espera que o agente faça quando suas ferramentas falharem e nomeie os sinais pelos quais vai avaliá-lo. O artigo oferece este modelo:

> Quando as chamadas de ferramenta do [agente] forem [canceladas | corrompidas], esperamos que ele [comportamento observável] e que falhe de forma segura, em vez de fabricar. Após {x} chamadas de ferramenta com falha em {y} minutos | {z} chamadas de ferramenta com falha em sequência, um alarme deve ser disparado e a equipe de DevOps notificada em até {a} minutos. Na recuperação, ele volta ao normal em [tempo], medido por [métricas/sinais].

Depois do experimento, compare o comportamento real com essa hipótese. O artigo diz que as lacunas são onde o agente precisa de iteração e refinamento.

### Hipótese de resiliência
A hipótese tem três partes: o comportamento esperado sob uma falha nomeada, os limiares de alarme e notificação, e o tempo de recuperação com os sinais usados para medi-lo. Escrevê-la primeiro transforma o experimento em um teste com condição de aprovação, em vez de uma exploração aberta. Os marcadores do modelo são o que você preenche para o seu próprio agente.

## Pré-requisitos
O passo a passo usa o experimento agentcore-strands-agent-faults da biblioteca de templates do FIS no GitHub. Esse repositório fornece o módulo controlador de caos, o documento de Automation do SSM, o template de experimento do FIS e políticas IAM de menor privilégio para cada papel. Você precisa do seguinte:
> 📋 **Checklist — Pré-requisitos**
> - [ ] Um agente Strands integrado com chaos_plugins() (incorpora strands_agentcore_chaos.py e suas dependências: strands, strands-agents-evals, pydantic e boto3).
> - [ ] O runtime ID do runtime AgentCore alvo (resolvido automaticamente a partir de AGENTCORE_RUNTIME_URL).
> - [ ] O documento de Automation do SSM implantado na sua conta e chamado ChaosExperiment.
> - [ ] Um papel IAM para o AWS FIS (confiável para fis.amazonaws.com) com a política de menor privilégio do repositório.
> - [ ] Um papel IAM para a Automation do SSM (confiável para ssm.amazonaws.com) com ssm:PutParameter e ssm:DeleteParameters em /chaos/*.
> - [ ] Um papel de execução do runtime de build de caos com ssm:GetParametersByPath em /chaos/*.
> - [ ] O template de experimento do FIS implantado na sua conta.

### Papéis de menor privilégio
A configuração usa três papéis IAM, um para o AWS FIS, um para a Automation do SSM e um para o runtime. Cada um tem apenas as permissões que sua etapa precisa. O papel do runtime pode ler os parâmetros de caos, mas não gravá-los, e o papel do SSM é o único que os grava.

## Integrando o caos ao seu agente
Os plugins de caos se conectam ao construtor do agente com plugins=chaos_plugins(). O artigo traz um arquivo completo e implantável:

```python
import logging
from strands import Agent
from strands_agentcore_chaos import chaos_plugins
from bedrock_agentcore.runtime import BedrockAgentCoreApp
from my_tools import get_pokemon, get_move

logging.getLogger("strands_agentcore_chaos").setLevel(logging.INFO)

agent = Agent(
    model="us.anthropic.claude-sonnet-4-5-20250929-v1:0",
    system_prompt=(
        "You are a Pokémon team-building advisor. Use the get_pokemon and get_move tools. "
        "If a tool call fails, immediately stop, notify the user which data you could not "
        "retrieve, and do not answer from guesses."
    ),
    tools=[get_pokemon, get_move],
    plugins=chaos_plugins(),
)

app = BedrockAgentCoreApp()

@app.entrypoint
def invoke(payload):
    return {"result": str(agent(payload.get("prompt", "")))}

if __name__ == "__main__":
    app.run()
```

**O que isso faz:**
- `logging.getLogger("strands_agentcore_chaos").setLevel(logging.INFO)` define o nível de log do módulo de caos como INFO.
- `plugins=chaos_plugins()` é o controle grosso. É a única linha que torna o runtime capaz de injetar falhas.
- A frase sobre tratamento de falhas no `system_prompt` é o que as execuções do artigo mostraram produzir comportamento seguro sob falha.
- Os nomes em `tools=[get_pokemon, get_move]` são os nomes que as chaves de `fault_injections` devem corresponder.
- `@app.entrypoint` expõe `invoke` como o ponto de entrada do runtime. Ele passa o prompt ao agente e retorna a resposta como string.

### Prompt de sistema para tratamento de falhas
A instrução "If a tool call fails, immediately stop, notify the user which data you could not retrieve, and do not answer from guesses" (em português: "Se uma chamada de ferramenta falhar, pare imediatamente, informe ao usuário quais dados não conseguiu obter e não responda com base em suposições") é o que separou as boas execuções das ruins. O artigo é claro que isso é uma mitigação probabilística, não um interruptor. Por isso ele recomenda verificar o prompt com experimentos de caos, em vez de confiar nele.

> ❌ **Anti-padrão — confiar apenas no prompt:** Presumir que uma linha no prompt de sistema garante comportamento seguro sob falha. **Por que as pessoas fazem isso:** não coberto no artigo. **Consequência:** o comportamento muda com execuções não determinísticas, e a lacuna só é encontrada em produção. **Correção:** verificar a instrução com execuções de caos repetidas, como faz o artigo.

## Configurando o experimento
Atualize o documentParameters do template de experimento padrão do FIS para atender aos seus requisitos. Dois deles importam mais. FaultRate decide se uma invocação individual sofre falha. FaultInjections define quais falhas se aplicam a quais ferramentas:

```json
{
    "get_move": [{"effect_type": "timeout"}],
    "get_pokemon": [{"effect_type": "truncate_fields", "max_length": 200}]
}
```

**O que isso faz:**
- Cada chave é o nome de uma ferramenta, e as chaves devem corresponder exatamente aos nomes de ferramentas registrados no agente.
- `get_move` recebe um efeito `timeout`, que é uma falha de pré-hook: a chamada é cancelada antes de executar.
- `get_pokemon` recebe `truncate_fields` com `max_length` 200, que é uma falha de pós-hook: a ferramenta executa e sua resposta é cortada.
- Usar dois efeitos de famílias diferentes em uma mesma configuração testa os dois modos de falha em uma só execução.
- O artigo alerta que uma chave que não corresponde não produz erro. O experimento informa sucesso enquanto não faz nada.

Efeitos de pré-hook cancelam a chamada de ferramenta: timeout, network_error, execution_error e validation_error. Efeitos de pós-hook corrompem a resposta: truncate_fields (com max_length), remove_fields (com remove_ratio) e corrupt_values (com corrupt_ratio).

### Ordem de escrita e de parada
O documento de Automation do SSM grava primeiro fault_rate, fault_injections e execution_id, e define active=true por último. O artigo diz que isso significa que nenhuma invocação enxerga uma configuração pela metade. Na parada, o documento inverte essa ordem e aguarda o cache de TTL esvaziar. Os handlers onCancel e onFailure executam o mesmo caminho limpo de desativação se um alarme de condição de parada disparar.

> ❌ **Anti-padrão — nomes de ferramentas que não correspondem:** Escrever chaves de fault_injections que não correspondem aos nomes de ferramentas registrados. **Por que as pessoas fazem isso:** não coberto no artigo. **Consequência:** o experimento termina com "sucesso" e não testa nada. **Correção:** copiar os nomes do registro de ferramentas do agente e conferi-los nos parâmetros antes de iniciar.

## Executando o experimento
Inicie o experimento do FIS, confirme que o chaos está ativo no Parameter Store e envie uma rajada de tráfego:

```bash
aws fis start-experiment --experiment-template-id <YOUR_TEMPLATE_ID> --region <YOUR_REGION>

# confirm chaos is active
aws ssm get-parameters-by-path --path /chaos/<YOUR_RUNTIME_ID>/ --region <YOUR_REGION>

# send a burst of traffic
for i in $(seq 1 20); do
    agentcore invoke '{"prompt": "Which of these three has the strongest move?"}'
done
```

**O que isso faz:**
- `start-experiment` inicia o template do FIS, que executa o documento do SSM que liga o chaos.
- `get-parameters-by-path` em `/chaos/<YOUR_RUNTIME_ID>/` confirma que os valores estão no Parameter Store antes de o tráfego começar.
- O loop envia uma rajada de 20 invocações, o padrão de tráfego que o artigo usa para exercitar as falhas.
- O prompt faz uma pergunta que precisa de `get_move` para ser respondida, de modo que a ferramenta com falha realmente está no caminho.

## O que a falha revela
O artigo executou a mesma falha em duas versões do agente que diferem apenas no prompt de sistema. A falha foi `get_move` com `timeout` e `fault_rate=1.0`, com cinco execuções por configuração. A Configuração A não tinha prompt de tratamento de falhas. A Configuração B tinha.

> Figura 2: O agente sem prompt de tratamento de falhas (Configuração A) fez 21 chamadas à ferramenta quebrada e então respondeu com base no conhecimento de treinamento. O agente com prompt (Configuração B) desistiu após 12 chamadas e nomeou os dados de que estava faltando.

O prompt fez duas coisas de forma consistente. Reduziu o número de tentativas: a Configuração A usou em média cerca de 1,35 vez as chamadas de ferramenta da linha de base sem falhas, e chegou a 2,5 vez na pior execução. Mais importante, mudou o que o agente fazia quando as tentativas se esgotavam. Quando a pergunta foi feita como um usuário real a faria, a Configuração A respondeu com base no conhecimento prévio de treinamento em todas as execuções e apresentou esses dados como fatos. A Configuração B nomeou os dados faltantes em todas as execuções.

O artigo cita uma resposta literal da Configuração A, dada depois que todas as chamadas a `get_move` haviam falhado:

> "I'm experiencing technical difficulties with the move lookup tool. However, based on standard Pokémon move data: Among Mamoswine, Nidoking, and Garchomp, the one that learns the single most powerful move is Nidoking, with Megahorn at 120 power…"

A leitura do autor é que isso é uma cláusula de ressalva seguida de uma resposta confiante e detalhada, construída sobre dados de treinamento desatualizados. Com estoque proprietário, preços ou saldos de contas, em que o modelo não tem nenhum dado relevante, o mesmo comportamento poderia produzir números fabricados com a mesma autoridade. Um usuário, ou outro agente, poderia agir com base neles sem que ninguém percebesse.

### Três classes de resultado
O mesmo impedimento produziu três classes de resultado dependendo da configuração. **Falha segura** significa que o agente percebe a falha, recusa ou acrescenta uma ressalva, e não mostra ao usuário nenhum erro bruto. **Contorno resiliente** significa que o agente tenta de novo ou desvia da ferramenta quebrada, com um múltiplo das chamadas normais de ferramenta, o que custa latência, tokens e dinheiro. **Confiantemente errado** significa que o agente recorre a dados treinados e os apresenta como se as ferramentas tivessem respondido. O artigo diz que essa última classe corrói a confiança de forma invisível, e que o prompt de tratamento de falhas a eliminou.

### Corrupção é mais difícil
Quando o autor passou para falhas de pós-hook, catorze das quinze execuções sem prompt produziram uma resposta confiante sem mencionar que os dados estavam degradados. O agente só detectou a corrupção onde tinha expectativas fortes sobre como os valores deveriam ser. Por exemplo, ele reconheceu que 99999 não é um poder plausível para um golpe de Pokémon. Para valores proprietários não existem essas expectativas. Adicionar o prompt de tratamento de falhas mudou o resultado: todas as cinco execuções com prompt recusaram e nomearam os dados inválidos.

### Avaliadores LLM-as-a-judge
Para uma pontuação além da revisão manual das transcrições, o SDK Strands Evals traz três avaliadores LLM-judge: FailureCommunicationEvaluator, RecoveryStrategyEvaluator e PartialCompletionEvaluator. Cada um avalia essas classes de resultado em uma escala de 0 a 1. Isso torna a comparação repetível, embora o artigo não discuta quão confiáveis são os próprios juízes.

> ❌ **Anti-padrão — ler uma única transcrição como resultado:** Julgar o agente com base em uma única execução e tratá-la como prova. **Por que as pessoas fazem isso:** não coberto no artigo. **Consequência:** respostas não determinísticas podem fazer uma execução de sorte parecer um design seguro. **Correção:** executar cada cenário várias vezes e pontuar as execuções.

## Observando a falha
Uma falha no nível da ferramenta que o agente trata de forma graciosa é invisível ao monitoramento de infraestrutura. Durante o experimento do autor, cada métrica de erro do Amazon CloudWatch ficou exatamente em 0.0. Isso incluiu invocações em que todas as chamadas de ferramenta foram canceladas ou corrompidas. Toda invocação retornou HTTP 200, porque do ponto de vista do runtime nada havia falhado. Se a sua observabilidade acompanha apenas a disponibilidade, você não verá essa classe de problema.

> ❌ **Anti-padrão — observabilidade apenas de disponibilidade:** Monitorar se o runtime responde e retorna 200. **Por que as pessoas fazem isso:** não coberto no artigo. **Consequência:** um agente confiantemente errado parece saudável em todos os dashboards. **Correção:** validar as saídas das ferramentas do agente e emitir métricas a partir dessa validação, como na próxima seção.

### Falha silenciosa na prática
A distinção do artigo é entre uma falha segura, que alguém pode ver, e uma falha silenciosa, que parece sucesso. A métrica que importa é, portanto, aquela que registra dados ruins de ferramentas dentro do agente, e não se a infraestrutura continuou de pé.

## Mitigações
A principal mitigação do artigo é validar a saída das ferramentas dentro do agente. A abordagem adiciona um plugin de observabilidade de ferramentas no hook AfterToolCallEvent do Strands. O plugin valida a saída contra um contrato Pydantic e emite uma linha de log no Embedded Metric Format (EMF). O modelo de exemplo é:

```python
from pydantic import BaseModel, Field

class PokemonOutput(BaseModel):
    name: str = Field(min_length=4)  # catches truncate_fields
    pokedex_id: int
    types: list[str]
```

> 🔎 Contexto adicionado: o Pydantic é uma biblioteca Python que verifica dados contra classes cujos atributos têm anotações de tipo. Ele lança um ValidationError quando os dados não correspondem.

**O que isso faz:**
- `BaseModel` transforma cada atributo anotado em uma regra de validação, e passar a saída da ferramenta por ele lança um ValidationError quando os dados quebram o contrato.
- Declarar `name`, `pokedex_id` e `types` sem valores padrão os torna obrigatórios, então `remove_fields` falha com um erro "field required".
- `pokedex_id: int` e `types: list[str]` verificam o formato de cada valor, o que captura uma falha `corrupt_values` que muda o tipo de um valor.
- `name: str = Field(min_length=4)` é uma restrição sobre o próprio valor. Truncar "mamoswine" para "mam" resulta em uma string válida, então só a regra de tamanho mínimo a captura.
- Para capturar um valor com o tipo certo mas sem sentido, como um id da Pokédex negativo ou absurdamente grande, o artigo sugere uma verificação explícita de faixa, como `Field(ge=1)`.

### Verificações de tipo e verificações de restrição
O artigo traça uma linha clara entre os dois tipos de verificação. Uma anotação de tipo verifica o formato de um valor. Uma restrição verifica o próprio valor. Verificações de formato capturam mudanças de tipo e campos ausentes, e restrições capturam valores plausíveis mas errados. Normalmente você precisa de ambas.

### Embedded Metric Format
Com o EMF, o CloudWatch cria métricas diretamente a partir da linha de log, e essas métricas podem ter alarmes. É assim que as falhas de validação viram algo pelo qual uma equipe de operações pode ser acionada.

### Amazon Bedrock Guardrails
O artigo recomenda complementar essa abordagem com o Amazon Bedrock Guardrails, como filtragem de conteúdo e verificações de grounding contextual, ao executar agentes no Amazon Bedrock.

### Saídas estruturadas não verificam resultados de ferramentas
Saídas estruturadas e uso estrito de ferramentas em modelos Claude recentes garantem que o que o modelo emite corresponde a um schema, tanto a resposta final quanto os argumentos que ele passa às ferramentas. O artigo observa que elas não dizem nada sobre se os dados retornados pela ferramenta estavam corretos. Uma resposta válida pelo schema pode, ainda assim, ser construída sobre uma chamada de ferramenta expirada ou corrompida. As restrições de tamanho e faixa que capturaram truncamento e corrupção também não são aplicadas no lado do servidor.

> ❌ **Anti-padrão — presumir que schema válido significa correto:** Tratar uma resposta verificada por schema como verificada. **Por que as pessoas fazem isso:** não coberto no artigo. **Consequência:** uma resposta construída sobre dados corrompidos de ferramenta passa em todas as verificações. **Correção:** adicionar validação às próprias saídas das ferramentas, como faz o contrato Pydantic.

## Boas práticas
O artigo encerra sua orientação com esta lista:

> 📋 **Checklist — Boas práticas**
> - [ ] Use um runtime de caos dedicado.
> - [ ] Projete o comportamento que você quer e então teste-o.
> - [ ] Execute cada cenário várias vezes.
> - [ ] Valide a saída do agente, não apenas a disponibilidade.
> - [ ] Aponte as condições de parada para o impacto no cliente, não para a falha que você está injetando.

O artigo explica cada item brevemente. Não injete falhas em produção, e verifique se o runtime de produção omite chaos_plugins() e não tem acesso de leitura a /chaos/*. Escreva primeiro a salvaguarda de tratamento de falhas e então use o caos para provar que ela se sustenta. Respostas do agente são não determinísticas, então uma única execução limpa é uma anedota, não evidência. Um agente que continua "no ar" enquanto serve respostas fabricadas falhou no teste. Condições de parada devem disparar alarmes sobre a taxa de sucesso das tarefas, e não sobre a métrica de erro de ferramentas que o experimento foi feito para mover.

## Limpeza
O experimento apaga seus próprios parâmetros quando termina, mas os recursos ao redor persistem. O artigo traz os comandos para removê-los:

```bash
# Delete FIS template and SSM document
aws fis delete-experiment-template --id <YOUR_TEMPLATE_ID> --region <YOUR_REGION>
aws ssm delete-document --name ChaosExperiment --region <YOUR_REGION>

# Delete the chaos runtime, container image, and log group
aws bedrock-agentcore-control delete-agent-runtime \
    --agent-runtime-id <YOUR_RUNTIME_ID> --region <YOUR_REGION>
aws ecr delete-repository --repository-name <YOUR_REPO> --force --region <YOUR_REGION>
aws logs delete-log-group --log-group-name <YOUR_RUNTIME_LOG_GROUP> --region <YOUR_REGION>
```

**O que isso faz:**
- Os dois primeiros comandos apagam o template de experimento do FIS e o documento SSM ChaosExperiment, para que o experimento não possa ser iniciado novamente.
- `delete-agent-runtime` remove o próprio runtime de caos.
- `delete-repository --force` remove o repositório ECR junto com sua imagem de contêiner.
- `delete-log-group` remove o log group do runtime.
- O artigo acrescenta que os três papéis IAM e qualquer alarme de condição de parada precisam ser desvinculados e apagados separadamente.

## Conclusão
O ponto do artigo é que o valor do experimento não está em confirmar que o agente continua rodando quando suas ferramentas falham. O valor está em descobrir o que o agente faz com dados ruins e se você perceberia isso. No teste do autor, o mesmo timeout produziu uma recusa limpa de um agente e uma resposta confiante e errada de outro, e os dois diferiam por uma linha de prompt. O artigo recomenda começar com uma taxa de falha baixa e um único timeout em um runtime de caos fora de produção, corrigir o que quebra e então escalar para resultados corrompidos, executando esses experimentos regularmente conforme o agente evolui.

## Para mais informações
O artigo lista estas referências. Apenas a URL do próprio artigo foi capturada no texto extraído, então as demais aparecem como títulos:
- Experimento agentcore-strands-agent-faults na biblioteca de templates do FIS — a fonte do controlador, do documento SSM, do template do FIS e das políticas IAM.
- Strands Agents SDK — o framework de agentes usado no exemplo.
- Guia de chaos testing do Strands — o teste de caos offline que o artigo estende para runtimes ao vivo.
- Documentação do Amazon Bedrock AgentCore — o runtime em que o agente é implantado.
- Amazon CloudWatch Embedded Metric Format — como as linhas de log EMF viram métricas.
- AWS Well-Architected Reliability Pillar: Test resiliency — a orientação de confiabilidade que o experimento segue.

## Sobre os autores
O texto extraído do artigo contém apenas este título. Nenhuma biografia de autor foi capturada.

## 🧠 Modelos mentais
### Três classes de resultado de uma falha de ferramenta
**O modelo:** Quando uma ferramenta falha, a resposta do agente cai em uma de três classes: falha segura, contorno resiliente ou confiantemente errado.
**Como aplicar:** Para cada ferramenta do seu agente, decida qual classe você quer e então teste se uma falha produz essa classe. Trate "confiantemente errado" como a primeira a eliminar.

### Verificações de formato versus verificações de restrição
**O modelo:** Uma anotação de tipo verifica o formato de um valor, e uma restrição verifica o próprio valor. Cada uma captura falhas que a outra deixa passar.
**Como aplicar:** Para cada campo da saída de uma ferramenta, adicione um tipo para o formato e uma restrição como min_length ou ge para o valor. Teste cada regra contra uma amostra truncada ou corrompida.

### Disponibilidade não é correção
**O modelo:** Um sistema pode continuar no ar, retornar HTTP 200 e ainda assim servir respostas fabricadas. Métricas de disponibilidade não conseguem mostrar isso.
**Como aplicar:** Combine alarmes de disponibilidade com métricas de validação que registrem dados ruins de ferramentas dentro do agente, e use medidas de impacto no cliente como condições de parada.

## 🔑 Principais conclusões
- Um timeout de ferramenta produziu uma recusa limpa em uma configuração e uma resposta confiante e errada, baseada em dados de treinamento, em outra. A única diferença foi uma linha no prompt de sistema.
- A corrupção pós-hook foi mais difícil de capturar. Catorze de quinze execuções sem prompt deram respostas confiantes sem sinalizar dados degradados.
- Um prompt de tratamento de falhas mudou esse resultado: todas as cinco execuções com prompt recusaram e nomearam os dados inválidos.
- Falhas graciosas são invisíveis ao monitoramento de infraestrutura. As métricas de erro do CloudWatch ficaram em 0.0 enquanto cada chamada de ferramenta falhava e cada invocação retornava HTTP 200.
- Contratos Pydantic capturam truncamento, campos ausentes e mudanças de tipo, mas apenas restrições de faixa ou tamanho capturam valores plausíveis e errados.
- Saídas estruturadas garantem o formato da saída do modelo, não a correção dos dados de ferramenta em que ela se baseou.
- Execute cada cenário várias vezes, porque uma única execução limpa não é evidência, e configure alarmes sobre o sucesso da tarefa, e não sobre a falha injetada.

## 🏛️ Para o Kenji
> 🔎 Esta é a minha interpretação, não a do autor. Para um arquiteto, a parte útil é a separação entre implantar o código de falhas e ativá-lo, o que permite testar um runtime semelhante ao de produção sem reimplantação. Isso levanta uma questão para comitês de revisão: quais saídas das ferramentas do seu agente têm um contrato no estilo Pydantic, e quais ferramentas o seu monitoramento mostraria como saudáveis mesmo que devolvessem dados plausíveis mas errados?

## 📚 Plano de leitura
### Do artigo
- Resilience testing your agents on Amazon Bedrock AgentCore (o artigo original) — a fonte completa, com as figuras e a redação exata do modelo de hipótese. [Resilience testing your agents on Amazon Bedrock AgentCore](https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore) — leia para conferir os números e a lista de referências, que esta nota não reproduz integralmente.
- Experimento agentcore-strands-agent-faults na biblioteca de templates do FIS — o código de trabalho, o documento SSM e as políticas IAM. (Link não presente no texto extraído.)
- Guia de chaos testing do Strands — a versão offline das mesmas falhas, para comparação. (Link não presente no texto extraído.)
- Modelos suportados por região da AWS — para verificar a disponibilidade dos modelos antes de implantar. (Link não presente no texto extraído.)
- Amazon Bedrock Guardrails — os controles de conteúdo e grounding que o artigo recomenda junto com a validação própria. (Link não presente no texto extraído.)
### Para ir além
- Chaos engineering — a prática geral da qual este experimento é um exemplo, para desenhar futuros experimentos de falha.
- Condições de parada para experimentos de injeção de falhas — como escolher alarmes que protejam os clientes sem encerrar o teste cedo demais.

## 🔗 Conceitos
- [well-architected](/pt-br/concepts/cloud-infra/well-architected.md)
- [llm-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md)
- [silent-failure](/pt-br/concepts/backend-systems/silent-failure.md)
- [chaos-engineering](/pt-br/concepts/backend-systems/chaos-engineering.md)
