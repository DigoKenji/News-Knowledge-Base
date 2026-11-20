---
type: Article Summary
title: 'Confiabilidade na nuvem: observabilidade como plano de controle para failover multirregião'
description: A observabilidade deve disparar ação automatizada e confirmação, porque o failover multirregião só é confiável quando testado sob carga real.
resource: https://www.infoq.com/articles/observability-control-plane-failover/
tags:
- cloud-infra
- chaos-engineering
- silent-failure
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/articles/observability-control-plane-failover/
  title: 'Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover'
  author: InfoQ
status: stable
source_url: https://www.infoq.com/articles/observability-control-plane-failover/
source: InfoQ
source_type: blog
published: '2026-11-20'
domain: cloud-infra
concepts:
- chaos-engineering
- silent-failure
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d8194a4b2e310479cc345
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md
---

## 🎯 Tese central
A observabilidade deve dirigir o comportamento do sistema, e não apenas informar pessoas: um sinal deve disparar uma ação automatizada, uma verificação posterior deve confirmar que a ação funcionou, e o failover multirregião só é confiável depois de ter sido executado sob carga real de produção e testado de ponta a ponta.

## 🧭 Por que o autor escreveu
A equipe do autor perdeu um registro de módulos Terraform por causa de um comportamento do S3 que eles nunca tinham modelado, e passou horas presumindo que a falha era deles. O artigo argumenta que a premissa de que "a infraestrutura de nuvem é confiável por padrão" é o que leva as equipes a construir pouca detecção, resposta e confirmação, e que uma queda regional forçou a equipe a projetar o failover com base no tráfego real, e não em diagramas.

## Principais conclusões (prefácio do autor)
O artigo abre com cinco afirmações. Cada uma é expandida abaixo e levada para o restante da nota.

- Um SLA de 99,99% é uma estatística, não uma promessa. Projete para a cauda dessa distribuição, porque, em escala suficiente, você eventualmente cairá nela.
- Um alerta que acorda uma pessoa só acelera a notificação. A resiliência vem de automação que age sobre o sinal e informa as pessoas depois.
- Uma região passiva que nunca carregou tráfego de produção é uma hipótese não testada. O modo ativo-ativo mantém as duas regiões continuamente validadas sob carga real.
- O TTL do DNS pode anular um failover perfeitamente executado. Escolha-o deliberadamente em função do seu RTO, do intervalo do health check e da tolerância a falsos positivos, em vez de aceitar o padrão do módulo.
- Uma detecção mais rápida só deixa a linha do tempo do post-mortem mais nítida. A confiabilidade exige recuperação automatizada mais um ciclo de confirmação que verifique se o failover de fato funcionou.

🔎 Contexto adicionado: RTO (recovery time objective) é o tempo máximo aceitável para restaurar o serviço após uma falha. Route 53 é o serviço de DNS da AWS, e seus health checks e registros ponderados são usados nos exemplos de roteamento abaixo.

## A falha que ninguém esperava de um serviço em que todos confiavam
O autor quase intitulou esta seção de "A falha que eu deveria ter previsto". A equipe do autor era dona de uma plataforma de execução do Terraform cujo registro de módulos era apoiado pelo S3, que o autor descreve como padrão, sem nada de exótico acrescentado. Numa manhã, o dashboard mostrou falhas no registro e artefatos que não baixavam. A equipe declarou um incidente e começou um processo de eliminação. Depois de cerca de duas horas, o autor ainda estava convencido de que a equipe tinha causado o problema por meio de uma política de bucket ou de um limite de permissões. O problema não estava nas mudanças, configurações ou no código deles.

### SLA como estatística
A principal lição do autor é sobre a premissa por trás do incidente. A equipe tratava a confiabilidade da nuvem como uma garantia. O autor aponta que um SLA de 99,99% significa "essencialmente sempre funciona" apenas na leitura casual. É uma afirmação estatística sobre uma distribuição, e é na cauda dessa distribuição que os incidentes acontecem. O artigo coloca o número de forma concreta: 99,99% permite 52 minutos e 36 segundos de indisponibilidade por ano. Na escala da equipe, o autor já tinha atingido essa janela mais de uma vez, mas não tratou esses episódios como evidência de que o projeto precisava mudar.

> ❌ **Anti-padrão: SLA como garantia:** Ler a porcentagem de disponibilidade de um provedor como promessa de que a dependência sempre vai funcionar, e projetar sem nenhum plano para a cauda. **Por que as pessoas fazem isso:** O SLA é um número em uma página de contrato, e soa como um compromisso. **Consequência:** Detecção, failover e recuperação ficam subdimensionados, e cada evento da cauda parece uma surpresa. **Correção:** Trate o SLA como uma distribuição, calcule o orçamento de indisponibilidade que ele permite e projete para as falhas que caem dentro dele.

### URLs assinadas do S3 e a requisição HEAD
O mecanismo era restrito. As URLs assinadas do S3 funcionavam quando chamadas manualmente, e um artefato baixava sem problemas. O Terraform, porém, chama essas URLs em sequência. Ao resolver dependências de módulos, ele dispara várias requisições seguidas. Algumas delas falhavam silenciosamente, mas não todas, e sem um padrão consistente. Isso foi suficiente para quebrar pipelines. Todos os dashboards apontavam para o registro, então a falha parecia um bug do registro, e não do S3.

O autor reproduziu o problema em um laptop. As falhas eram intermitentes e apareciam apenas em produção, onde experimentar é desencorajado, então o autor montou uma reprodução segura. A reprodução era uma pequena configuração Terraform que baixa módulos do mesmo caminho de registro usado pelos pipelines, executada sob um teste de carga. O autor também obteve o código-fonte do Terraform e acrescentou logs dos cabeçalhos de requisição e resposta HTTP. A falha se reproduziu em dez minutos. Os logs apontaram para uma requisição HEAD, não para o registro. O Terraform delega o download de módulos à biblioteca go-getter, da HashiCorp, que envia uma requisição HEAD a cada URL assinada antes do GET, para verificar se o download pode ser retomado. O autor propôs que uma mudança recente na forma como a AWS tratava essa requisição HEAD produzia o 404 em cascata.

🔎 Contexto adicionado: Uma requisição HEAD pede os cabeçalhos de um recurso sem o corpo. Aqui ela foi usada como verificação de retomada antes do download propriamente dito.

A hipótese não era óbvia nem para o suporte da AWS. O autor a sustentou com passos de reprodução claros e evidências de diagnóstico. Depois que o suporte escalou internamente para a equipe de arquitetura, a AWS confirmou que uma mudança recente no tratamento das requisições era a causa raiz e reverteu essa mudança. A lição que o autor tira: quando uma dependência gerenciada falha de forma intermitente, reproduza exatamente o padrão de chamadas do seu cliente fora da produção e instrumente o cliente, não apenas o servidor.

### Linha do tempo de detecção e recuperação
A detecção levou algumas horas. A recuperação levou quase 24 horas. A maior parte desse tempo foi gasta em chamadas ao suporte para provar que o problema estava do lado da AWS, esperando que a AWS reconhecesse que outros clientes também eram afetados e esperando que a reversão fosse publicada. Quando a AWS reverteu a implantação do S3, as requisições simultâneas com URLs assinadas se estabilizaram, e o registro se recuperou sem nenhuma mudança do lado da equipe.

## Observabilidade deve orientar decisões, não apenas exibi-las
A experiência do autor é que toda organização comete o mesmo erro. Alguém constrói cedo um bom sistema de observabilidade. Os dashboards são bem pensados, os alertas são calibrados e os runbooks estão linkados. Depois o sistema cresce, os engenheiros mudam de time, e novos serviços são conectados sem que ninguém atualize o que já existia. Os alertas saem de calibração, e os dashboards mostram problemas que ninguém do time atual sabe interpretar. Quando algo quebra numa hora ruim, o engenheiro de plantão pode levar vinte minutos para entender o que os dashboards querem dizer. O autor chama isso de falha de projeto, não de falta de habilidade. Os dashboards foram construídos como uma camada de relatório. Nunca foram conectados a nada que agisse sobre o que mostram, então uma pessoa continua sendo a ponte entre sinal e resposta.

O erro não tem nada a ver com ferramentas, linguagem de consulta ou layout. Ele está em tratar a observabilidade como uma forma de informar pessoas, e não como uma forma de dirigir o comportamento do sistema.

### Camada de relatório versus plano de controle
O artigo contrasta dois desenhos. Na camada de relatório, uma pessoa recebe o alerta, interpreta sob pressão, decide o que fazer e age. No plano de controle, o sistema recebe o sinal, avalia-o segundo uma política definida, age e depois informa a pessoa sobre o que aconteceu. O ponto do autor é sobre escala: o modelo de relatório escala com o número de pessoas e com a escala de plantão, enquanto o modelo de plano de controle continua funcionando quando ambos estão esticados.

O autor também observa que as ferramentas já existem. O Prometheus tem regras de alerta e roteamento por webhook há anos. O Terraform pode reconfigurar pesos de roteamento. Os componentes simplesmente não foram conectados com a automação como objetivo explícito. O mesmo padrão se aplica em todas as escalas, desde o autoscaling de pods até o failover regional: observar, decidir, agir e confirmar.

> ❌ **Anti-padrão: alerta como fim da cadeia:** Um alerta que aciona uma pessoa e mais nada, de modo que a resposta depende de o plantonista estar disponível e ser rápido. **Por que as pessoas fazem isso:** Acionar pessoas é o último passo familiar e bem compreendido do monitoramento. **Consequência:** O tempo de resposta escala com a disponibilidade humana, e a equipe carrega a carga cognitiva nas horas ruins. **Correção:** Associe uma ação automatizada ao alerta e faça com que a pessoa receba um resultado em vez de uma tarefa, sempre que a ação for segura de automatizar.

### Propriedade da observabilidade
O segundo ponto do autor é que a stack de observabilidade concentra conhecimento. Quando a pessoa que realmente conhece a stack sai, a calibração se desvia silenciosamente até que algo quebre no pior momento. A receita do artigo é dar à stack de observabilidade o mesmo modelo de propriedade dos serviços de produção: revisões regulares, testes reais de falha e uma responsabilidade coletiva que não expira quando o autor original sai. O autor já operou Prometheus e Grafana em centenas de serviços, grandes builds e pipelines de implantação com milhares de execuções por dia, e afirma que essas ferramentas têm um custo operacional que as equipes costumam subestimar de forma consistente.

> ❌ **Anti-padrão: observabilidade com dono único:** Dashboards e alertas que só a pessoa que os construiu entende e mantém. **Por que as pessoas fazem isso:** O construtor original é o único que conhece o contexto, então parece eficiente. **Consequência:** Os alertas se desviam, ninguém se atreve a mudá-los, e o sinal se torna pouco confiável sem que ninguém perceba. **Correção:** Revise alertas e dashboards em um cronograma, teste-os sob falhas reais e compartilhe a responsabilidade entre o time.

A Figura 1 do artigo mostra, lado a lado, a camada de relatório e o plano de controle. A diferença está em quem faz a ponte entre sinal e resposta: uma pessoa ou o sistema.

## Projetando failover multirregião que sobrevive a falhas reais
O autor observa que ninguém precisa de um white paper para entender por que o multirregião importa, basta uma queda. Depois de uma grande falha regional em um provedor de nuvem, a equipe teve a conversa sobre migrar o registro Terraform para uma arquitetura ativo-ativo. O registro é chamado em toda execução de pipeline, incluindo cada build, implantação e release. Quando ele cai, todos param, em todas as equipes. O raio de impacto dessa dependência era muito maior do que sugeria o diagrama de arquitetura.

### Ativo-ativo versus ativo-passivo
A equipe escolheu ativo-ativo. O autor enfatiza que o motivo dessa escolha aparece com menos frequência do que deveria: a equipe queria a região secundária sob carga real de produção antes de precisar dela para o failover.

O ativo-passivo fica bem em diagramas, mas seu modo de falha é sutil. A região passiva nunca tratou tráfego real. A configuração dela pode ter se desviado sem ninguém notar. Os caches estão frios, e as dependências a jusante nunca foram exercitadas sob algo próximo da carga real. Os testes em staging usam carga sintética que não corresponde ao perfil real de tráfego. Quando o failover acontece sob pressão, todos esses problemas aparecem de uma vez. O autor já esteve nessa ponte de incidente e a descreve como tentar se recuperar de um problema enquanto descobre um segundo conjunto que você não sabia que tinha.

O ativo-ativo custa mais. As regiões precisam ser mantidas sincronizadas, e a consistência de dados sob partição exige reflexão cuidadosa. Mas, quando uma região se degrada, a carga se desloca dentro de um sistema que sempre rodou em duas regiões. A equipe operava em 50/50 em condições normais. As duas regiões eram exercitadas constantemente, que é a validação contínua que o autor deseja. O autor acrescenta que 50/50 é o estado estável, não o estado de falha. Quando uma região realmente se degrada, o tráfego passa para 100/0.

> ❌ **Anti-padrão: região passiva não testada:** Uma região de standby que nunca carregou tráfego de produção e que se presume funcionar quando necessário. **Por que as pessoas fazem isso:** Custa menos para manter, e fica certo no diagrama de arquitetura. **Consequência:** Desvio de configuração, caches frios e dependências não exercitadas aparecem durante o incidente, quando são mais difíceis de lidar. **Correção:** Coloque a secundária sob carga real de produção (ativo-ativo) ou, no mínimo, teste-a com tráfego realista em um cronograma.

### 100/0 e testes de caos
O artigo diz que a transição 100/0 é aquilo para que o projeto está sendo construído, e ela precisa ser testada deliberadamente. A equipe executou testes de caos justamente para esse caso: derrubar uma região inteira, verificar se a outra absorve toda a carga sem problemas e confirmar a recuperação quando a região volta. A conclusão do autor: se você não testou o estado 100/0, não testou seu sistema de recuperação de desastres. Você testou sua configuração de roteamento.

🔎 Contexto adicionado: Testes de caos significam injetar falhas deliberadamente em um sistema em execução para observar como ele se comporta. O artigo aplica a ideia à perda de uma região inteira.

O autor é franco sobre o fato de que o multirregião é caro de construir e manter. Para uma dependência crítica de build como esse registro, foi a decisão certa, mas o artigo defende que ela deve ser uma decisão deliberada, e não um padrão.

### Falha correlacionada entre regiões
O artigo aponta dois modos de falha que um teste de caos limpo não revela. O primeiro é uma falha que não é regional. O incidente do S3 atingiu as duas regiões ao mesmo tempo, porque ambas dependiam do mesmo comportamento do S3. O failover regional não resolve isso. O failover desloca o tráfego para a outra região, que está falhando da mesma forma, então o failover também dispara ali, e o tráfego volta. A automação fica movendo o tráfego entre duas regiões quebradas e acrescenta ruído a um incidente que não consegue resolver. O Route 53 se comporta da mesma forma na camada de DNS: quando todos os registros ponderados estão não saudáveis, ele devolve todos eles, e não nenhum. A resposta automatizada correta a uma falha em dependência compartilhada é manter o tráfego onde está e acionar uma pessoa.

🔎 Contexto adicionado: Uma falha correlacionada é aquela que derruba vários componentes redundantes de uma vez porque eles compartilham uma dependência, o que anula uma redundância construída apenas no nível dos componentes.

> ❌ **Anti-padrão: failover para dentro de uma dependência compartilhada:** Failover automatizado que continua movendo o tráfego entre regiões que dependem do mesmo serviço quebrado. **Por que as pessoas fazem isso:** O failover é a resposta automatizada esperada para qualquer alerta de saúde regional. **Consequência:** O tráfego oscila, o ruído aumenta, e o incidente nunca é resolvido pela automação. **Correção:** Detecte quando todas as regiões se degradam juntas, interrompa o failover, mantenha o tráfego no lugar e escale para uma pessoa.

### Health checks que verificam a coisa certa
O segundo modo de falha é um health check que informa a coisa errada. O endpoint /health da equipe confirmava que o processo da aplicação estava rodando, não que ele conseguia fazer o seu trabalho. Se o banco de dados de backend em uma região cai, a aplicação continua respondendo 200 OK em /health enquanto as requisições reais falham. Esse é exatamente o caso para o qual o failover regional existe, mas o Route 53 vê um endpoint saudável e continua enviando tráfego para a região quebrada. A Lambda de confirmação, descrita abaixo, verifica o mesmo endpoint, então também reportaria como bem-sucedido um failover para essa região.

A correção é um health check que exercite a funcionalidade que o serviço entrega, como uma leitura leve no banco de dados. O artigo alerta que checks profundos carregam risco próprio: se a dependência que eles testam é compartilhada pelas duas regiões, eles trazem de volta o problema de falha correlacionada descrito acima.

> ❌ **Anti-padrão: health check no nível do processo:** Um endpoint /health que retorna 200 sempre que o processo está rodando, independentemente de conseguir atender requisições. **Por que as pessoas fazem isso:** É o padrão em muitos frameworks e é barato de implementar. **Consequência:** O roteamento e o failover veem uma região saudável enquanto as requisições falham, e a lógica de confirmação informa sucesso falsamente. **Correção:** Verifique a capacidade real do serviço com uma chamada leve à dependência, e pese o risco de uma dependência compartilhada nos checks mais profundos.

### TTL do DNS
O artigo chama o TTL do DNS de a configuração incorreta mais comum em recuperação de desastres, mesmo em sistemas bem projetados. O TTL controla por quanto tempo um resolvedor mantém em cache um registro DNS antes de consultá-lo de novo. Se o TTL é alto demais, por exemplo 300 ou 600 segundos, ou o que quer que tenha sido o padrão do módulo, a lógica de failover pode disparar corretamente e os pesos de roteamento podem ser atualizados corretamente, mas os clientes continuam acessando a região degradada por minutos, porque os resolvedores ainda guardam o registro antigo. O failover rodou, mas o tráfego nunca se moveu.

Se o TTL é baixo demais, o sistema reage exageradamente. Um pico de latência passageiro dispara uma mudança de DNS, o problema some trinta segundos depois, e as regiões ficam alternando, amplificando uma instabilidade que se resolveria sozinha. A conclusão do autor é que o TTL deve ser um número deliberado, escolhido em função do RTO, do intervalo do health check e da tolerância a falsos positivos. A maioria das equipes usa o padrão do módulo de IaC definido anos antes, e só descobre o valor enquanto depura um failover que não saiu como o esperado.

A Figura 2 do artigo mostra como uma mudança de peso que dispara corretamente ainda pode deixar o tráfego preso na região degradada até que os caches dos resolvedores expirem.

> ❌ **Anti-padrão: TTL padrão do módulo:** Aceitar o TTL do DNS que um módulo de infraestrutura traz de fábrica, sem verificá-lo contra os objetivos de recuperação. **Por que as pessoas fazem isso:** O padrão é definido uma vez, funciona na operação normal, e ninguém o revisita. **Consequência:** O failover é mais lento do que o projetado, ou tão rápido que oscila. **Correção:** Escolha o TTL em função do RTO, do intervalo do health check e da tolerância a falsos positivos, e documente a escolha.

## Recuperação de desastres como um sistema automatizado e observável
A visão do artigo sobre DR é que o que separa sistemas que funcionam dos que existem só no papel é a automação com confirmação. A detecção sozinha não basta. O sistema também precisa agir sobre o sinal e verificar se a ação funcionou. Muitas equipes com investimento sério em DR têm detecção, como alertas quando as taxas de erro sobem ou quando health checks falham. Poucas construíram a camada de resposta. Menos ainda construíram a camada de confirmação, que verifica se a resposta de fato funcionou.

A camada de confirmação importa mais do que as pessoas imaginam. Failover automatizado sem confirmação é um sistema que age e depois fica em silêncio. A secundária recebeu o tráfego? O DNS propagou dentro da janela do TTL? As dependências a jusante estão respondendo corretamente? Um sistema que continua observando depois de agir é um sistema em que você pode confiar. Um sistema que dispara e fica em silêncio é, nas palavras do artigo, uma hipótese não testada.

### Alertas compostos com duração
O padrão do artigo começa com um alerta do Prometheus que avalia uma condição composta: taxa de erro acima de um limiar, E latência p99 elevada, E sustentadas por uma janela definida. O autor explica o motivo. Alertas de métrica única são ruidosos, e um pico passageiro os dispara constantemente por problemas que se resolvem em um ou dois minutos. A condição composta com exigência de duração só dispara em degradação que persistiu.

🔎 Contexto adicionado: A latência p99 é o tempo de resposta que 99% das requisições superam, então descreve a cauda lenta, e não a média.

A maioria das implementações para no alerta. Neste desenho, o alerta é a porta de entrada para uma cadeia automatizada. Como o tráfego se move depende da topologia. No ativo-ativo, o Route 53 faz a troca: cada registro regional carrega um peso e um health check, e quando o health check de uma região falha, o Route 53 deixa de devolver aquele registro, então o tráfego se move sem nenhum código customizado. No ativo-passivo, o alerta dirige a troca: o Alertmanager roteia para um webhook, que invoca uma função Lambda que troca o registro de failover do Route 53 para a região secundária. Nos dois casos, o alerta também dispara uma Lambda de confirmação que consulta o endpoint de saúde da região alvo até que ele devolva respostas saudáveis. A pessoa só é notificada depois que a confirmação termina. O resumo do artigo é que detecção, ação e confirmação formam um único ciclo conectado, no qual a observabilidade dirige a ação e depois verifica o resultado.

A Figura 3 do artigo mostra esse ciclo: o Prometheus dispara, o Route 53 desloca o tráfego, a Lambda consulta até que a região alvo esteja saudável, e então a equipe é notificada.

### O ciclo detectar-agir-confirmar em código
O artigo apresenta uma Lambda de confirmação em Python:

```python
import urllib.request
import time

def lambda_handler(event, context):

    # CONFIRM: Route 53 or the failover webhook has already moved traffic
    # Lambda confirms the failover succeeded and notifies
    confirmed = False
    for attempt in range(10):
        time.sleep(30)
        try:
            response = urllib.request.urlopen(
                'https://registry-secondary.internal.example.com/health',
                timeout=5
            )
            if response.status == 200:
                confirmed = True
                break
        except Exception:
            continue

    return {
        'failover_executed': True,
        'confirmation_status': 'healthy' if confirmed else 'unconfirmed',
        'attempts': attempt + 1
    }
```

**O que isso faz:**
- Roda depois que o failover foi disparado, então seu papel é verificar o resultado, e não agir.
- Espera 30 segundos antes de cada uma das até 10 verificações, o que dá tempo para o DNS e para a região alvo se estabilizarem. A primeira verificação acontece após 30 segundos, e não imediatamente.
- Usa um timeout de 5 segundos em cada requisição, para que uma conexão travada não paralise o laço.
- Trata qualquer exceção como "ainda não saudável" e tenta de novo, o que absorve erros transitórios durante a troca.
- Devolve `unconfirmed` se o laço nunca obtiver um 200. O artigo diz que esse status deve virar um alerta, e não uma falha silenciosa. O campo `failover_executed` é sempre `True` neste esboço, então registra o disparo, não o resultado.

O laço não se fecha até que o health check devolva 200. Se nunca devolver, a função retorna "unconfirmed", e esse sinal se torna um alerta.

O artigo também mostra o lado do Route 53 em Terraform. As duas regiões têm o mesmo peso, e o Route 53 deixa de devolver o registro de uma região quando o health check dela falha:

```hcl
resource "aws_route53_health_check" "primary" {
  fqdn              = "registry-primary.internal.example.com"
  port              = 443
  type              = "HTTPS"
  resource_path     = "/health"
  failure_threshold = 3  
  request_interval  = 10  # or 30, fine tune based on the system
}

resource "aws_route53_health_check" "secondary" {
  fqdn              = "registry-secondary.internal.example.com"
  port              = 443
  type              = "HTTPS"
  resource_path     = "/health"
  failure_threshold = 3
  request_interval  = 10
}

resource "aws_route53_record" "registry_primary" {
  zone_id        = var.zone_id
  name           = "registry.internal.example.com"
  type           = "A"
  set_identifier = "primary"

  weighted_routing_policy {
    weight = 50
  }

  health_check_id = aws_route53_health_check.primary.id
  ttl             = 30  # or 60 can be fine tuned based on the system.
  records         = [var.primary_region_ip]
}

resource "aws_route53_record" "registry_secondary" {
  zone_id        = var.zone_id
  name           = "registry.internal.example.com"
  type           = "A"
  set_identifier = "secondary"

  weighted_routing_policy {
    weight = 50
  }

  health_check_id = aws_route53_health_check.secondary.id
  ttl     = 30   # or 60 can be fine tuned based on the system.
  records = [var.secondary_region_ip]
}
```

**O que isso faz:**
- Dois health checks HTTPS consultam o caminho /health de cada região a cada 10 segundos e marcam a região como não saudável após 3 falhas consecutivas.
- Cada região tem seu próprio registro A ponderado com peso 50, então o tráfego normal se divide em 50/50 e as duas regiões continuam sendo exercitadas.
- Cada registro está vinculado ao seu health check, então o registro de uma região não saudável deixa de ser devolvido.
- O TTL de 30 segundos é um valor curto, o que acelera o failover, mas aumenta o risco de oscilação discutido acima. Os comentários na linha dizem para ajustá-lo ao sistema.

Em seguida, o artigo apresenta a regra de alerta do Prometheus. Na fonte, ela aparece depois do Terraform, sob o rótulo "Prometheus rule:":

Regra do Prometheus:

```yaml
groups:
  - name: registry_availability
    rules:
      - alert: RegistryRegionalDegradation
        expr: |
          sum by (region) (rate(http_requests_total{job="registry", status=~"5.."}[2m]))
            /
          sum by (region) (rate(http_requests_total{job="registry"}[2m]))
            > 0.03
          and
          histogram_quantile(0.99,
            sum by (le, region) (rate(http_request_duration_seconds_bucket{job="registry"}[2m]))
          ) > 0.8
        for: 90s
        labels:
          severity: critical
        annotations:
          summary: "Registry degradation in region {{ $labels.region }}"
          description: "Error rate above 3% and p99 latency above 800ms sustained for 90 seconds in {{ $labels.region }}"
```

**O que isso faz:**
- Calcula a razão de erros 5xx por região em uma janela de 2 minutos e só dispara quando ela ultrapassa 3%.
- Exige que a latência p99 ultrapasse 0,8 segundo (800 ms) na mesma região, então o alerta precisa dos dois sintomas.
- A cláusula `by (region)` avalia a condição separadamente para cada região, então o alerta nomeia a região degradada e a automação sabe em qual agir.
- `for: 90s` significa que a condição precisa se manter por um minuto e meio. Sem isso, a regra dispararia em oscilações; com isso, dispara apenas em degradação sustentada.
- O rótulo de severidade `critical` e as anotações dão à pessoa um resumo legível depois que a automação agiu.

### Game days
A prática de teste preferida pelo artigo é o game day: um exercício agendado em que a equipe injeta deliberadamente falhas reais em produção e percorre detecção, resposta e recuperação em tempo real, com as pessoas que tratariam um incidente real presentes na sala. Não é uma revisão de runbook em staging.

Game days encontram lacunas humanas tão bem quanto as técnicas. Quem tem autoridade de decisão quando o comandante do incidente não pode ser contatado? Onde está, exatamente, o runbook desse modo de falha, e todos sabem que ele existe? Na primeira vez que a maioria das equipes faz um, essas respostas são desconfortáveis, e esse é o objetivo, porque descobri-las durante um game day custa uma tarde.

O artigo afirma que o teste mais consistentemente pulado é o da camada de confirmação. As equipes testam se o failover dispara, mas raramente testam o que acontece quando ele dispara e o sistema conclui que não funcionou. Esse caminho pode gerar falhas em cascata que a própria equipe causou.

> ❌ **Anti-padrão: testar apenas o gatilho:** Validar que um failover dispara sem validar que a camada de confirmação reconhece corretamente o sucesso e para de agir. **Por que as pessoas fazem isso:** O disparo é o comportamento visível, e é fácil de demonstrar. **Consequência:** Um veredito falso de "falhou" pode manter a automação agindo, criando uma falha em cascata. **Correção:** Nos game days, inclua um caso em que o sistema acredita que o failover não funcionou e verifique se ele para de agir.

## Conclusão
O autor conclui que o incidente do S3 não era algo que a equipe tivesse modelado. Ninguém tinha um runbook para "o S3 quebra silenciosamente requisições simultâneas com URLs assinadas sob carga, depois de uma implantação interna". A premissa por trás da maior parte do design de sistemas, a de que a infraestrutura subjacente é basicamente confiável, está certa na maioria das vezes, e por isso raramente é questionada.

O artigo termina com três passos para transformar um plano de failover em um sistema que funciona:

> 📋 **Checklist: transforme o plano de failover em um sistema que funciona**
> - [ ] Escolha um alerta que hoje aciona uma pessoa e conecte-o a uma resposta automatizada com uma etapa de confirmação.
> - [ ] Coloque a região secundária sob tráfego real de produção antes de precisar dela para o failover.
> - [ ] No seu próximo game day, teste a camada de confirmação, não apenas o gatilho. Verifique se o sistema reconhece um failover bem-sucedido e para de agir.

A frase final do autor: se você não validou os três, tem um plano, não um sistema. A infraestrutura vai falhar, e a próxima falha provavelmente será uma que você ainda não viu. Se uma pessoa precisa montar a resposta manualmente, ou se o sistema cuida disso, foi decidido muito antes de o incidente começar.

## 🧠 Modelos mentais
### Observar, decidir, agir, confirmar
**O modelo:** Todo mecanismo de confiabilidade do artigo é um laço de quatro passos: observar um sinal, decidir segundo uma política definida, agir automaticamente e confirmar o resultado antes de notificar uma pessoa.
**Como aplicar:** Para cada alerta, nomeie a decisão que ele dirige, a ação que se segue e a verificação que prova que a ação funcionou. Se algum passo estiver faltando, o alerta é um elemento de relatório, não de controle.

### Camada de relatório versus plano de controle
**O modelo:** Uma camada de relatório diz a uma pessoa o que aconteceu, e um plano de controle age sobre o que aconteceu e depois informa a pessoa. A primeira escala com o número de pessoas, e a segunda continua funcionando quando o número de pessoas está esticado.
**Como aplicar:** Para cada dashboard ou alerta, pergunte quem age sobre ele e quanto tempo isso leva às 3 da manhã. Se a resposta for "uma pessoa, depois de vinte minutos lendo", considere se existe uma ação automatizada segura.

### O ativo-ativo mantém o caminho de failover aquecido
**O modelo:** Um caminho de failover que carrega tráfego real o tempo todo é validado continuamente. Um standby que não carrega nenhum é uma hipótese.
**Como aplicar:** Teste o estado 100/0 deliberadamente com o corte de uma região, e trate a divisão normal como o estado estável que torna os estados de falha críveis.

### Dependências compartilhadas anulam a redundância
**O modelo:** A redundância construída no nível regional não protege contra uma falha em um serviço que todas as regiões compartilham, e o failover automatizado pode piorar esse caso.
**Como aplicar:** Liste as dependências que todas as regiões compartilham, incluindo os serviços de armazenamento e DNS do provedor de nuvem. Para cada uma, decida o que a automação deve fazer quando todas as regiões se degradam juntas, o que normalmente é manter e acionar uma pessoa.

### Sinais compostos e sustentados
**O modelo:** Um alerta sobre uma métrica, sem duração, dispara com ruído. Uma condição composta com janela sustentada dispara em degradação que persiste.
**Como aplicar:** Combine um sinal de taxa de erro com um sinal de latência, exija uma duração e rotule o alerta por região, para que a automação conheça seu alvo.

## 🔑 Principais conclusões
- Um SLA de 99,99% permite 52 minutos e 36 segundos de indisponibilidade por ano. Projete o failover e a recuperação para essa cauda, em vez de tratar o número como garantia.
- Falhas em dependências gerenciadas podem ser intermitentes e visíveis apenas para o cliente, então reproduza o padrão exato de chamadas do cliente fora da produção e instrumente o cliente tanto quanto o servidor.
- Mantenha a região secundária sob carga real de produção (o artigo usa ativo-ativo em 50/50), para que o failover seja validado continuamente, e depois teste o estado 100/0 com um corte deliberado de região.
- O failover não resolve uma falha em dependência compartilhada. Quando todas as regiões se degradam juntas, a automação deve manter o tráfego e acionar uma pessoa.
- Escolha o TTL do DNS e o desenho do health check em função do RTO, do intervalo de verificação e da tolerância a falsos positivos. Um endpoint /health que só prova que o processo está ativo continuará roteando tráfego para uma região quebrada.

## 🏛️ Para o Kenji
Isto se aplica diretamente ao trabalho de arquitetura de qualquer dependência de plano de controle compartilhado, como um registro, um repositório de artefatos, um provedor de identidade ou o DNS. A pergunta do artigo para uma revisão de arquitetura é quais decisões de failover o sistema toma automaticamente, quais ele mantém e como confirma cada resultado. *(Esta é a sua interpretação, não a do autor.)* Uma pergunta útil a levantar: para um serviço do qual a sua plataforma depende, o que a camada de confirmação faz quando conclui, de forma errada, que um failover falhou?

## 📚 Plano de leitura
### A partir do artigo
- O corpo do artigo não cita links externos, então não há nenhum link a seguir a partir dele.

### Para se aprofundar
- Health checks e políticas de roteamento ponderado do AWS Route 53: como health checks com falha removem registros das respostas, e como o TTL e o cache dos resolvedores afetam o tempo de failover.
- Downloads de módulos do Terraform e a biblioteca go-getter: como o Terraform busca módulos, incluindo a requisição HEAD usada para verificar a retomada.
- Regras de alerta do Prometheus e roteamento do Alertmanager: durações `for`, expressões compostas e receptores de webhook.
- Engenharia do caos e game days: como as equipes planejam e executam exercícios controlados de falha em produção.
- Falhas correlacionadas em desenhos multirregião: dependências compartilhadas que anulam a redundância regional.
