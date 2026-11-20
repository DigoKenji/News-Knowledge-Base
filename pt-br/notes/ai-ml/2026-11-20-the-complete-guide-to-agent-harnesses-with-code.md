---
type: Article Summary
title: O Guia Completo de Agent Harnesses (Com Código)
description: O código do harness em torno do agente, e não o modelo, define o custo do agente e a aplicação das regras; por isso, imponha as regras em código.
resource: https://medium.com/data-science-collective/the-complete-guide-to-agent-harnesses-with-code-6fa11cecd004
tags:
- ai-ml
- agent-harness
- inference-tradeoffs
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/data-science-collective/the-complete-guide-to-agent-harnesses-with-code-6fa11cecd004
  title: The Complete Guide to Agent Harnesses (With Code)
  author: Data Science Collective
status: stable
source_url: https://medium.com/data-science-collective/the-complete-guide-to-agent-harnesses-with-code-6fa11cecd004
source: Medium (Data Science Collective)
source_type: medium
published: '2026-08-21'
domain: ai-ml
concepts:
- agent-harness
- inference-tradeoffs
- multi-agent-orchestration
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81b1866ecfc255dd007c
lang: pt-br
translation_of: /notes/ai-ml/2026-11-20-the-complete-guide-to-agent-harnesses-with-code.md
---

## 🎯 Tese central
O código ao redor de um modelo, o harness, decide quanto um agente custa e o que ele tem permissão para fazer; por isso, uma regra que você precisa manter deve ser imposta em código, e não apenas declarada em um prompt.

## 🧭 Por que o autor escreveu isto
O autor colocou uma regra ("nunca aplique uma edição que o humano não aprovou") em um arquivo de configuração, e um agente passou por ela onze vezes em uma única tarde, produzindo cerca de setenta e seis edições não aprovadas em um entregável de cliente. Se a tese estiver certa, uma regra que roda como hook em código antes de cada chamada de ferramenta bloqueia a edição, enquanto a mesma regra no prompt não bloqueia, e a equipe ganha algo concreto para depurar quando a regra estiver errada.

## O que o Harness Realmente Oferece
O loop central do agente tem seis linhas: pedir ao modelo a próxima chamada, parar se não houver nenhuma, executar a chamada e anexar o resultado. Tratamento de erros e uma nova tentativa levam o código a cerca de vinte linhas. O ponto do autor é que a parte interessante é a função `execute`, onde você decide o que o modelo pode fazer, a que ele pode acessar, o que ele lembra, quem o verifica e o que ele enxerga. Essas decisões são o harness.

```py
while agent.turns < max_turns:
    call = agent.next_call(observations)
    if call is None:
        break
    result = execute(call)
    observations.append(result)
```

### Agent harness
**O que é:** o código ao redor de um modelo fixo: o loop de ferramentas, as verificações antes de uma ferramenta rodar, o sandbox, a memória, a verificação e a montagem do contexto. O artigo usa "harness" neste sentido em todo o texto e admite que o campo usa a palavra de forma mais livre.
**Como funciona:** o modelo apenas propõe uma chamada. O harness decide se essa chamada é executada, com quais recursos, e qual resultado volta para o contexto. Toda decisão que importa para o comportamento está nesse caminho.
**Por que importa:** uma regra que existe apenas como texto do modelo depende de o modelo lembrá-la. Uma regra no harness roda em toda chamada, inclusive nas chamadas em que o modelo esqueceu a regra.
**Quando se aplica:** sempre que um agente puder alterar arquivos, acessar a rede ou gastar tokens em seu nome.

A primeira afirmação do autor é sobre custo, e ela é sustentada por um preprint de junho de 2026. O estudo rodou três harnesses (Goose, OpenCode, OpenHands-SDK) com dois modelos, em um subconjunto estratificado de 50 tarefas do Terminal-Bench Pro, uma variante mais difícil de um benchmark de agentes de terminal. Ele encontrou diferença de até 40x em tokens por tarefa resolvida entre harnesses. Atualizar o modelo mudou esse mesmo número em apenas 1,0 a 1,3x.

> 🔎 Contexto adicional: tokens por tarefa resolvida é a quantidade de tokens de entrada e saída do modelo consumida para cada tarefa que o agente concluiu. Um número maior significa mais dinheiro por sucesso.

A segunda afirmação é sobre capacidade, e ela é mais fraca. O mesmo estudo encontrou diferenças de taxa de acerto entre harnesses de 0 a 8 pontos percentuais, e os intervalos de confiança bootstrap cruzaram zero em todas as diferenças, exceto a maior. Os autores descrevem seus resultados de acurácia como descritivos, dado n=50. A leitura prática é que a mesma tarefa, no mesmo modelo, com a mesma taxa de sucesso, pode custar quarenta vezes mais em um harness do que em outro, enquanto o harness não torna um modelo fraco mais inteligente.

## Mova a Regra para o Código
Uma regra no system prompt é uma aposta de que o modelo se lembra dela, a lê como pretendido e a segue sob pressão de contexto. O modelo vence essa aposta na maioria das vezes, e o autor argumenta que essa confiabilidade é justamente o perigo, pois é o motivo pelo qual as pessoas param de verificar. Uma regra em código roda em toda chamada.

O artigo aponta cinco pontos no loop do agente onde o código pode impor uma regra. Cada um tem uma pergunta, uma guarda e um caso de falha. O conselho do autor é começar pela camada cuja falha você consegue imaginar acontecendo com você nesta semana, e não construir as cinco no primeiro dia.

> ❌ **Anti-padrão — regra apenas no prompt:** a política vive em um system prompt e nada verifica a chamada. **Por que as pessoas fazem isso:** é a forma mais rápida de declarar uma regra, e ela funciona na maioria das vezes. **Consequência:** quando o modelo se desvia, a violação passa em silêncio, sem erro e sem registro. **Correção:** mova a regra para uma função que roda antes da chamada da ferramenta.

## Camada 1: A Fronteira de Execução
**A pergunta:** o que o agente tem permissão para fazer?

A fronteira é uma função que roda entre a decisão do modelo e a chamada da ferramenta. O artigo mostra a falha primeiro. O system prompt diz para nunca apagar um arquivo sem perguntar, e o agente tem três chamadas de ferramenta na fila. O `execute` ingênuo apaga sem verificar.

```py
SYSTEM_PROMPT = "IMPORTANT: never delete a file without asking the user first."

def execute(call):
    if call.name == "delete_file":
        FILES.pop(call.args["path"], None)
        return f"deleted {call.args['path']}"
```

**O que isto faz:**
- `SYSTEM_PROMPT` guarda a regra como texto, que o modelo pode ou não seguir.
- `execute` despacha pelo nome da ferramenta e realiza a exclusão diretamente, sem nenhuma verificação no meio.
- A função é o único lugar onde a ferramenta roda, portanto é o lugar natural para impor a política.
- Resultado no artigo: o agente apaga `cache.tmp`, depois `client-deliverable.md`, e nunca pergunta.

### Fronteira de execução
**O modelo:** uma função guarda que envolve `execute` e inspeciona cada chamada antes de ela rodar. Ela devolve uma string de negação quando uma regra se aplica e, caso contrário, deixa a chamada passar.
**Como funciona:** `boundary(rules)` retorna um wrapper. Cada regra é um nome, um predicado sobre a chamada e um motivo. A primeira regra que casar retorna `DENIED by <name>: <reason>` e a ferramenta nunca é chamada.

A correção é um wrapper que roda antes da ferramenta. A regra de aprovação se aplica a exclusões que não têm uma flag explícita de aprovação humana.

```py
def boundary(rules):
    def wrap(execute):
        def guarded(call):
            for name, deny_if, reason in rules:
                if deny_if(call):
                    return f"DENIED by {name}: {reason}"
            return execute(call)
        return guarded
    return wrap

NEEDS_APPROVAL = [(
    "delete-needs-approval",
    lambda c: c.name == "delete_file" and not c.args.get("approved_by_human"),
    "deletion requires an explicit human approval flag on the call",
)]
```

**O que isto faz:**
- `boundary` é uma fábrica no estilo decorator: recebe as regras e retorna uma função que envolve qualquer `execute`.
- Cada regra é uma tupla de nome, predicado e motivo, então uma negação explica qual regra disparou.
- O predicado `c.args.get("approved_by_human")` deixa passar uma exclusão aprovada, de modo que a regra bloqueia por padrão e permite de propósito.
- `NEEDS_APPROVAL` é dado, então novas regras são adicionadas a uma lista, em vez de editadas dentro de `execute`.
- Mesmo agente, mesmo script, mesmo prompt: nada é apagado.

> ❌ **Anti-padrão — regra apenas no prompt:** a frase "never delete without asking" fica no system prompt, sem nenhum caminho de código que a verifique. **Por que as pessoas fazem isso:** não custa nada escrevê-la e ela soa como uma política. **Consequência:** a exclusão acontece e nada registra que uma regra foi quebrada. **Correção:** adicione uma função de fronteira que nega a chamada a menos que a flag de aprovação esteja presente.

> 🔎 Contexto adicional: na primeira seção do artigo, o autor conta 76 edições não aprovadas e 46 linhas marcadas, enquanto a seção final conta 15 violações, 11 delas em uma única tarde. A nota mantém cada número como o autor o escreveu, porque os dois conjuntos medem coisas diferentes.

## Camada 2: Sandboxing
**A pergunta:** o que o agente consegue alcançar?

Um sandbox é o conjunto de diretórios e hosts que o processo do agente consegue tocar. Você o constrói partindo do zero e adicionando de volta o que o trabalho precisa: uma allowlist de caminhos, uma allowlist de hosts e um processo que não enxerga nada fora deles. Uma deny list, a primeira versão usual, nomeia os caminhos que o agente não deve tocar.

```py
DENY = ["secrets/"]

def execute(call):
    path = call.args["path"]
    if any(path.startswith(d) for d in DENY):   # checks the spelling
        return "DENIED by deny-list"
    real = os.path.normpath(path)               # the ../ collapses HERE, after the check
    return DISK.get(real, "not found")
```

```javascript
read('secrets/api_key')          -> DENIED by deny-list
read('work/../secrets/api_key')  -> sk-live-DO-NOT-LEAK
```

**O que isto faz:**
- `DENY` lista prefixos, e a verificação compara a string bruta que o agente digitou.
- `os.path.normpath` roda depois da verificação, então os segmentos `..` são colapsados apenas na hora da busca.
- As duas leituras de exemplo mostram a brecha: a primeira é recusada, a segunda chega ao mesmo arquivo.
- O ponto do artigo é a ordem das operações: a verificação e a busca discordam sobre o que é o caminho.

### Resolução do caminho antes da comparação
**O modelo:** normalize o caminho primeiro e depois compare o caminho normalizado com as raízes permitidas.
**Como funciona:** `resolve` colapsa `..` e outras grafias, e aceita o caminho somente se ele for igual a uma raiz permitida ou estiver dentro dela. Qualquer outro caso retorna `None`.

A correção resolve antes de comparar:

```py
ALLOW_ROOTS = ["work"]

def resolve(path):
    real = os.path.normpath(path)               # resolve FIRST
    if not any(real == r or real.startswith(r + os.sep) for r in ALLOW_ROOTS):
        return None
    return real
```

**O que isto faz:**
- `normpath` roda primeiro, então toda grafia de um caminho vira uma única string canônica antes que qualquer regra a veja.
- A verificação `real == r or real.startswith(r + os.sep)` casa a raiz exatamente ou como prefixo de diretório, então `workshop/` não passa como se fosse `work/`.
- As duas grafias do exemplo agora viram `secrets/api_key`, fora de `work`, e ambas são recusadas.
- A allowlist substitui a deny list porque uma deny list só bloqueia as grafias que o autor dela imaginou.

O artigo é claro ao dizer que esta camada não é uma fronteira de segurança real. Uma verificação dentro do seu próprio programa vê apenas as chamadas que ela reconhece. O mesmo arquivo pode ser alcançado por um comando de shell, um symlink ou um caminho `..` que a verificação nunca inspeciona. Em produção, afirma o autor, a mesma lógica é imposta pelo próprio processo: um usuário ou container sem permissão de leitura fora de `work/`.

> ❌ **Anti-padrão — deny list sobre a string bruta:** a verificação compara a string que o agente digitou, e a busca usa o caminho resolvido. **Por que as pessoas fazem isso:** é fácil de escrever e bloqueia o caminho óbvio. **Consequência:** `work/../secrets/api_key` lê o segredo enquanto a regra informa que a leitura foi negada. **Correção:** resolva o caminho primeiro e permita apenas caminhos sob raízes conhecidas, e então aplique o mesmo limite no nível do sistema operacional.

> 🔎 Contexto adicional: as referências inline em nomes de arquivo como sandbox.py e persistence.py apontam para domínios .py. São nomes de arquivo no repositório, não referências separadas, então o próprio repositório é a fonte a ser lida.

## Camada 3: Persistência de Memória
**A pergunta:** o que sobrevive a um reset de contexto?

A mudança mais útil do autor foi reiniciar conversas sem hesitação. O contexto enche, a qualidade cai e o agente começa a se contradizer, então, em vez de sustentar uma sessão, garanta que as coisas que importam estejam em um lugar que um reinício não toque: código commitado, configuração, o próprio harness e notas extraídas.

Todo estado que o agente produz precisa ir para um de três lugares: a conversa, onde morre com a sessão; o disco, onde sobrevive e pode ser recuperado; ou a configuração do harness, onde molda todas as sessões seguintes. A maioria das equipes nunca escolhe explicitamente, e é por isso que seus agentes parecem sem memória em alguns aspectos e teimosamente errados em outros.

```py
CONVERSATION, DISK, HARNESS_CONFIG = [], {}, {}

def remember(kind, key, value):
    """'chat' dies with the session, 'disk' survives it,
    'config' shapes every session that follows."""
    {"chat": lambda: CONVERSATION.append(value),
     "disk": lambda: DISK.__setitem__(key, value),
     "config": lambda: HARNESS_CONFIG.__setitem__(key, value)}[kind]()
```

**O que isto faz:**
- Três contêineres representam os três tempos de vida: conversa, disco e configuração.
- `remember` torna o destino um argumento obrigatório, então ninguém consegue guardar um valor sem nomear seu tempo de vida.
- O despacho por dicionário usa lambdas para que apenas o ramo escolhido rode.
- A demo do artigo (`layers/03_memory_persistence.py`) mostra a conversa devolvendo uma lista vazia enquanto a decisão e a regra permanecem no disco e na configuração.

### Três destinos de memória
**O modelo:** todo estado do agente tem um tempo de vida, e o harness deve nomeá-lo.
**Como funciona:** valores `chat` são limpos com a sessão, valores `disk` sobrevivem a reinícios e podem ser recuperados, e valores `config` mudam como todas as sessões posteriores se comportam.
**Quando se aplica:** ao decidir onde uma decisão, uma lição de uma falha ou uma nota extraída deve viver, antes de a sessão que a produziu terminar.

> ❌ **Anti-padrão — tudo na conversa:** decisões e regras vivem apenas no histórico do chat. **Por que as pessoas fazem isso:** é o padrão, e nada precisa ser escrito. **Consequência:** um reset perde a decisão, e a próxima sessão repete o erro. **Correção:** escreva as decisões no disco e as regras na configuração do harness, e mantenha o chat apenas para o estado de trabalho.

## Camada 4: Loops de Verificação
**A pergunta:** quem verifica o trabalho, e o que impede que ele mesmo o conserte?

Um loop de verificação é uma segunda passada sobre a saída do agente, feita por algo que pode relatar um problema, mas não pode corrigi-lo. O alerta do autor é que um revisor com permissão de escrita vai reparar silenciosamente aquilo que encontrar. Isso parece eficiente, mas remove o sinal, porque você não consegue mais distinguir qual trabalho estava certo e qual estava errado e foi remendado.

O segundo padrão é o dry run. Para qualquer coisa irreversível, a ferramenta recebe uma flag que, por padrão, descreve a ação, e só age quando a flag é desligada explicitamente. Uma flag esquecida fica então inofensiva.

```py
def execute(call):
    if call.name == "review":
        snapshot = dict(CODE)               # a copy, so the reviewer cannot write
        src = snapshot[call.args["path"]]
        return f"VERDICT: {'off-by-one' if '+ 1' in src else 'looks good'}"
    if call.name == "apply_fix":
        if call.args.get("dry_run", True):  # on by default, turned off on purpose
            return f"DRY RUN: would rewrite {call.args['path']}, nothing written"
        CODE[call.args["path"]] = call.args["new"]
        return f"wrote {call.args['path']}"
```

**O que isto faz:**
- O ramo `review` trabalha sobre `snapshot`, uma cópia, então pode ler o código mas não alterá-lo.
- Ele retorna uma string de veredito, que é uma crítica sem efeitos colaterais.
- `apply_fix` define `dry_run` como `True` por padrão, então a escrita só acontece quando quem chama desliga a flag de propósito.
- A mensagem de dry run diz o que aconteceria e confirma que nada foi escrito; na demo, o bug off-by-one continua no arquivo ao final.

### Revisor somente leitura
**O modelo:** um revisor produz uma crítica, e um passo separado, com seu próprio registro, age sobre ela.
**Como funciona:** o revisor recebe uma cópia do estado e retorna um veredito. Aplicar o veredito é outra chamada, com sua própria flag e seu próprio log.
**Quando se aplica:** qualquer agente que verifique a saída própria ou a de outro agente.

### Dry run por padrão
**O modelo:** ferramentas irreversíveis descrevem seu efeito, a menos que quem chama diga explicitamente para agir.
**Como funciona:** a flag `dry_run` vem ligada por padrão, então omiti-la é seguro.
**Quando se aplica:** exclusões, escritas em documentos compartilhados, deploys e tudo o que não pode ser desfeito.

> ❌ **Anti-padrão — revisor com permissão de escrita:** o subagente revisor pode editar o arquivo que revisa. **Por que as pessoas fazem isso:** economiza um passo, e a correção parece certa. **Consequência:** a revisão deixa de mostrar o que estava errado, e a equipe não consegue auditar a mudança. **Correção:** torne o revisor somente leitura e aplique as correções em um passo separado.

## Camada 5: Pipelines de Contexto
**A pergunta:** o que o modelo realmente vê, e quanto isso custa?

Um pipeline de contexto é o caminho que a informação percorre até a janela do modelo, e a escolha de quanto dela chega. O custo é o centro da questão: cada token na janela é pago naquele turno e em todos os turnos seguintes. Por isso, a diferença de quarenta vezes da primeira seção aparece aqui, e não na fronteira de execução.

O harness ingênuo carrega tudo. Envia o arquivo inteiro quando o agente perguntou sobre uma função, a saída de busca inteira quando era necessária uma linha, e todos os turnos anteriores. Logo, o turno vinte paga de novo por tudo o que os turnos de um a dezenove leram.

A correção é delegação com destilação. Um subagente recebe a própria janela de contexto, explora dezenas de milhares de tokens e devolve de um a dois mil. A thread principal vê apenas a resposta.

```py
def subagent_search(query):
    """Its own window. The main thread never pays for this reading."""
    global subagent_tokens
    subagent_tokens += sum(len(v.split()) for v in CORPUS.values())
    return next(f"{n}: {b.split('ANSWER:')[1].strip()}"
                for n, b in CORPUS.items() if "ANSWER:" in b)
def execute(call):
    global main_tokens
    distilled = subagent_search(call.args["q"])
    main_tokens += len(distilled.split())    # the only line that bills you
    return distilled
```

**O que isto faz:**
- `subagent_search` debita sua leitura em `subagent_tokens`, um contador separado, então o custo da exploração é contabilizado à parte da thread principal.
- A função retorna apenas o texto após `ANSWER:` do primeiro documento que casa, que é o resultado destilado.
- `execute` cobra da thread principal apenas pela string destilada, a única linha marcada como ponto de cobrança.
- A divisão entre os dois contadores é a ideia toda: qual contador é incrementado determina o que a conta mostra.

A demo do repositório é a versão honesta mais simples. A pergunta e a resposta são idênticas nos dois casos, enquanto a thread principal lê 9.602 tokens em uma versão e 2 na outra.

### Isolamento de contexto por meio de subagente
**O modelo:** um subagente faz a exploração na própria janela e devolve uma resposta curta, então o contexto principal paga apenas pela resposta.
**Como funciona:** a leitura do subagente é debitada no contador próprio dele. A thread principal recebe uma string destilada, normalmente de um a dois mil tokens, em vez de dezenas de milhares.
**Quando se aplica:** buscas, leituras de arquivos grandes e qualquer exploração cuja saída seja muito maior que a resposta.

### Subagentes sequenciais
**O modelo:** uma cadeia de subagentes, cada um esperando o anterior, compra isolamento e nenhum paralelismo.
**Como funciona:** cada etapa roda depois que a anterior retorna, então o tempo total é a soma das etapas.
**Quando se aplica:** o autor diz para rodar as etapas em paralelo quando forem independentes, e para ser honesto sobre o custo do isolamento quando não forem.

> ❌ **Anti-padrão — cadeia sequencial descrita como paralela:** etapas que dependem umas das outras são apresentadas como ganho de velocidade ou de vazão. **Por que as pessoas fazem isso:** diagramas de multiagentes parecem trabalho paralelo. **Consequência:** o ganho de velocidade esperado não aparece, e as chamadas extras ao modelo aumentam o custo. **Correção:** rode em paralelo apenas as etapas independentes, e trate o custo da cadeia como o preço do isolamento.

> ❌ **Anti-padrão — carregar tudo a cada turno:** o arquivo inteiro, a saída de busca inteira e todos os turnos anteriores entram em cada chamada. **Por que as pessoas fazem isso:** é o harness mais simples e funciona. **Consequência:** o custo sobe a cada turno, porque cada um paga por todas as leituras anteriores. **Correção:** delegue a exploração a um subagente que devolva uma resposta destilada.

> 📋 **Checklist — as cinco perguntas das camadas (derivadas dos cinco pontos e de suas perguntas no artigo)**
> - [ ] Fronteira de execução: o que o agente tem permissão para fazer, e isso é imposto em código?
> - [ ] Sandboxing: o que o agente consegue alcançar, e o acesso é verificado depois que o caminho é resolvido?
> - [ ] Persistência de memória: para cada estado, ele está na conversa, no disco ou na configuração do harness?
> - [ ] Loops de verificação: quem verifica o trabalho, e o verificador consegue alterá-lo?
> - [ ] Pipelines de contexto: o que entra na janela a cada turno, e quanto isso custa?

> 🔎 Contexto adicional: a regra prática do artigo é que qualquer item que você não consiga marcar com um caminho de arquivo é uma regra na qual você está apenas torcendo.

## O Que Ninguém Pode Dizer Ainda
Esta seção lista os limites das evidências, e é a parte em que o autor é mais cuidadoso.

- **Nenhum estudo controlado aqui foi revisado por pares.** Todos os resultados de isolamento de harness até agosto de 2026 são preprints arXiv não revisados. O experimento mais cuidadoso se descreve como trabalho preliminar em avaliação em um workshop, e o artigo de enquadramento mais citado é um paper de posição, sem experimento. A conclusão do autor é refazer as comparações nas suas próprias tarefas antes de orçar com base em qualquer uma delas.
- **Ganhos de harness frequentemente não sobrevivem a tarefas novas.** Um artigo de julho de 2026 concluiu que a evolução automática de harness "does not consistently outperform simple test-time scaling methods and exhibits limited generalization", porque a busca e a avaliação final compartilham um benchmark. Em uma avaliação de aprendizado contínuo no Terminal-Bench 2.0, o GEPA subiu para 70,8% nas tarefas de ajuste e depois caiu para 54,5% no conjunto mais amplo, abaixo dos 56,8% em que começou. A lição é medir um harness em tarefas que você não usou para construí-lo.
- **Um resultado de fine-tuning pode ser pontuado pelo próprio sinal de recompensa.** A Baseten fez fine-tuning do Gemma 3 27B para geração de notas clínicas, passando de 35% pior que o Claude Sonnet 4 para 60% melhor. O texto deles diz que "the eval harness itself is used as the reward signal during training". O autor considera isso legítimo e diz que, quando um fornecedor relata uma vitória, você deve perguntar qual era a avaliação durante o treinamento.
- **Um harness não é portátil entre trocas de modelo.** Um estudo de junho de 2026 concluiu que aplicar um harness apenas após o treinamento recupera pouco do benefício de treinar com ele já no lugar. Modelos pós-treinados em um harness mínimo devolveram "Invalid tool format" em 75,1% das tentativas quando o formato das definições de ferramenta mudou, e uma variante do Qwen2.5–7B ficou 10,8 pontos abaixo do modelo base do qual partiu. A lição é escolher o harness antes do pós-treinamento e esperar que uma troca no meio do projeto custe mais do que parece.
- **O campo não concordou sobre o que "harness" significa.** Um glossário da Hugging Face de maio de 2026 separa harness de scaffold, e o artigo observa que Claude Code, Codex e outros chamam o conjunto inteiro de harness mesmo assim. A divisão em cinco camadas é uma decomposição entre outras, e, sob uma definição mais restrita, boa parte das evidências mede outra coisa. A lição é verificar quais camadas cada estudo inclui antes de comparar resultados.
- **Os efeitos do harness podem diminuir à medida que os modelos melhoram.** O Harness-Bench relata que backends de modelo mais fortes apresentam "higher mean scores while exhibiting lower cross-harness variance". O conselho do autor é dar mais peso ao trabalho de harness voltado para controle de custo, que persiste, do que à acurácia, que um novo modelo pode absorver.

> ❌ **Anti-padrão — ajustar e testar nas mesmas tarefas:** um harness ou fine-tune é pontuado no conjunto em que foi construído ou treinado. **Por que as pessoas fazem isso:** é o conjunto que elas têm, e os ganhos aparecem rapidamente ali. **Consequência:** o ganho desaparece em tarefas novas, como mostra o resultado do GEPA. **Correção:** reserve tarefas que o harness nunca viu e relate os resultados sobre elas.

> ❌ **Anti-padrão — trocar o harness após o pós-treinamento:** o modelo é treinado em um harness e depois colocado em outro. **Por que as pessoas fazem isso:** o harness parece um componente substituível. **Consequência:** erros de formato de ferramenta aumentam e boa parte do benefício do treinamento se perde. **Correção:** escolha o harness antes do pós-treinamento e mantenha-o estável.

## O Repositório
O autor disponibiliza dez scripts em github.com/paoloap-py/agent-harness-guide. Cada camada aparece com sua guarda e novamente sem ela, para que você possa comparar as duas execuções. O modelo é substituído por um substituto roteirizado que emite uma sequência fixa de chamadas de ferramenta, então nenhuma chave de API é necessária.

```javascript
git clone https://github.com/paoloap-py/agent-harness-guide
cd agent-harness-guide

python3 run_all.py        # all five layers, guard off then on, side by side
python3 test_harness.py   # asserts every difference above, 10 checks
```

**O que isto faz:**
- `git clone` baixa o repositório, que contém os scripts das camadas e as versões com falha.
- `run_all.py` roda as cinco camadas duas vezes, com a guarda desligada e depois ligada, para que as diferenças fiquem visíveis em uma única saída.
- `test_harness.py` verifica as diferenças que o artigo descreve, com 10 checagens.
- Os scripts usam substitutos roteirizados do modelo, então os resultados são repetíveis em qualquer máquina.

## Onde Isso Deixa Você
A regra do autor era sólida, mas ficou no lugar errado durante duas semanas. Isso custou quinze violações, onze delas em uma única tarde. Mover a regra para um hook é o que a fez valer, e o texto da regra não mudou. O modelo é uma compra. O harness ao redor dele decide quanto o agente custa e com que frequência ele age sem sanção, e ambos são trabalho de engenharia.

## FAQ
**O que é um agent harness?** O código ao redor do modelo: o loop de ferramentas, as verificações antes de uma ferramenta rodar, o sandbox, o que persiste entre sessões, o que revisa a saída e qual contexto chega a cada turno. O loop tem seis linhas, ou cerca de vinte com tratamento de erros.

**O harness realmente importa mais do que o modelo?** Em custo, sim, em até 40x em tokens por tarefa resolvida. Em se a tarefa é resolvida, a diferença medida entre harnesses é de 0 a 8 pontos percentuais, com intervalos de confiança cruzando zero. Os autores do Terminal-Bench concluem que a escolha do modelo geralmente importa mais para o desempenho.

**Onde a escolha do harness custa mais?** No contexto. Cada token enviado a cada turno é pago a cada turno, então a camada de contexto é onde o dinheiro vai, e delegar uma busca a um subagente muda a conta sem mudar a resposta.

**Regras de permissão são uma fronteira de segurança?** Não. A verificação do harness roda sobre a chamada que o agente fez, e o mesmo arquivo pode ser alcançado por um comando de shell, um symlink ou um caminho `..` que a verificação nunca vê. O limite que vale é o que o sistema operacional impõe ao processo: inicie-o sem acesso a nada e adicione de volta o que o trabalho precisa.

**Devo construir meu próprio harness ou adotar um?** Adote um e se comprometa com ele. Trocar zera o conhecimento institucional dos seus arquivos de configuração, e toda regra escrita depois de uma falha real precisa ser conquistada de novo.

## 🧠 Modelos mentais
### Regras em código, não em prompts
**O modelo:** uma regra só é real quando o código a impõe em toda chamada. Uma frase em um prompt é um pedido que o modelo pode esquecer.
**Como aplicar:** para cada regra que você se importa, nomeie a função que a verifica. Se não conseguir nomear uma, a regra é uma esperança.

### Resolva antes de comparar
**O modelo:** uma verificação e uma busca precisam ver o mesmo valor canônico, senão a verificação protege a coisa errada.
**Como aplicar:** normalize caminhos, identificadores e entradas primeiro, e então aplique toda regra a esse valor normalizado.

### Revisores relatam, não reparam
**O modelo:** um verificador que pode alterar o trabalho destrói o registro do que estava errado.
**Como aplicar:** torne os revisores somente leitura e coloque toda mudança em um passo separado e registrado.

### Pague pelo que entra na janela
**O modelo:** contexto é um custo recorrente, pago de novo a cada turno em que permanece na janela.
**Como aplicar:** deixe os subagentes explorarem em suas próprias janelas e devolverem a resposta, e contabilize o custo de cada um separadamente.

## 🔑 Principais conclusões
- Uma regra de harness imposta em código bloqueou a edição não aprovada que uma regra de prompt não havia conseguido impedir, e o autor encontrou a linha errada lendo o arquivo.
- O mesmo agente, no mesmo modelo, com a mesma taxa de sucesso, custou até 40x mais tokens em um harness do que em outro, enquanto a atualização do modelo mudou o número em 1,0 a 1,3x.
- Verificar um caminho antes de resolvê-lo deixa passar `work/../secrets/api_key`, então resolva primeiro e permita apenas raízes conhecidas.
- Revisores devem ser somente leitura, e ferramentas irreversíveis devem ter dry run como padrão.
- Subagentes com as próprias janelas reduziram o contexto da thread principal de 9.602 para 2 tokens na demo do artigo, mas cadeias sequenciais compram isolamento, não velocidade.
- Ganhos de harness costumam desaparecer em tarefas novas: o GEPA caiu de 70,8% no conjunto de ajuste para 54,5% no conjunto mais amplo, abaixo dos 56,8% de partida.

## 🏛️ Para o Kenji
Seu trabalho de arquitetura enfrenta a mesma lacuna entre regras declaradas e regras impostas sempre que você coloca um agente de IA em um caminho de entrega. As perguntas das camadas no checklist oferecem uma forma de revisar uma configuração de agente como você revisaria qualquer controle, perguntando onde cada regra é imposta e quanto ela custa. Esta é a sua interpretação, não a do autor. A questão que ela levanta para você: quais das regras atuais dos seus agentes falhariam em uma auditoria que pede um caminho de arquivo, e não uma frase?

## 📚 Plano de leitura
### Do artigo
- [github.com/paoloap-py/agent-harness-guide](http://github.com/paoloap-py/agent-harness-guide) — os dez scripts, com a guarda de cada camada e o mesmo agente sem ela; execute-os para ver a diferença.
### Para aprofundar
- Terminal-Bench e Terminal-Bench Pro — o benchmark de agentes por trás da comparação de custo de 40x; leia-o para julgar como as tarefas foram escolhidas.
- Glossário da Hugging Face sobre harness versus scaffold (maio de 2026) — a fonte de vocabulário que o artigo cita para definir um harness.
- Harness-Bench — o relatório sobre a menor variância entre harnesses em backends de modelos mais fortes.
- Write-up da Baseten sobre o fine-tuning do Gemma 3 27B para notas clínicas — a fonte da ressalva sobre avaliação como sinal de recompensa.
- Avaliação de aprendizado contínuo do GEPA no Terminal-Bench 2.0 — a fonte da queda de 70,8% para 54,5% em tarefas novas.
