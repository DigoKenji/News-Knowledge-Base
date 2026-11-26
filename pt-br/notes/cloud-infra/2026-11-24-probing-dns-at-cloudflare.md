---
type: Article Summary
title: Sondando DNS na Cloudflare
description: A Cloudflare estendeu seu framework de testes Flamingo para sondar continuamente a propagação de DNS, e usa essas medições de SLO para decidir sobre as releases.
resource: https://blog.cloudflare.com/probing-dns-at-cloudflare
tags:
- cloud-infra
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://blog.cloudflare.com/probing-dns-at-cloudflare
  title: Probing DNS at Cloudflare
  author: Cloudflare
status: stable
source_url: https://blog.cloudflare.com/probing-dns-at-cloudflare
source: Cloudflare
source_type: blog
published: '2026-11-23'
domain: cloud-infra
concepts:
- control-plane
- canary-deploy
- slo-error-budget
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d814e84b6f9d9508f2e14
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md
---

## 🎯 Tese central
Sondar o DNS continuamente, a partir de todos os data centers, contra a produção, com o mesmo framework modular de testes usado para HTTP, permite que as medições de SLO decidam se uma release é promovida ou revertida.

## 🧭 Por que o autor escreveu
A história começa com um incidente real: um bug de agendamento no serviço que publica registros DNS na borda da Cloudflare deixou algumas alterações de registro sem publicação por horas. O serviço continuou no ar e saudável, mas novos hostnames resolviam lentamente e certificados não podiam ser emitidos até que o bug fosse corrigido. Se a tese estiver certa, regressões de DNS são detectadas automaticamente pelo mesmo tipo de medição que controla outras releases, antes de se espalharem.

## A abertura: uma release que quebrou a produção
O artigo não tem título para a abertura, então o título aqui é um rótulo acrescentado à nota. Ele começa com um cenário familiar: você lança uma release, os testes unitários passam, os dashboards estão verdes, e então o telefone começa a tocar porque a produção está quebrada. A equipe de DNS da Cloudflare já viveu isso. O bug foi um problema de agendamento que deixou alterações de registro paradas, sem publicação, por horas. Nada parecia fora do ar no sentido usual, mas a falha era real para os clientes.

O autor então explica por que essa classe de falha é difícil de capturar. Falhas de DNS são lentas, se espalham pela rede e são parciais quanto ao local onde causam impacto. A meta que definiram é testar o DNS da forma como os clientes o experimentam: "continuamente, a partir de todos os data centers, e contra a produção."

### Modos de falha do DNS
O DNS mapeia hostnames para registros, e uma falha nele raramente derruba um serviço por completo. Em vez disso, ela aparece como respostas lentas ou desatualizadas, em alguns lugares e em outros não. O ponto do artigo é que o atraso é inerente, porque uma alteração precisa chegar aos servidores autoritativos e depois passar pelos caches antes que todos a vejam. Um serviço pode, portanto, parecer saudável por dentro enquanto novos nomes ou certificados falham para os clientes. Por isso o autor quer medição de fora para dentro, e não verificações internas ao serviço.

### Medindo do lado do cliente
As sondas rodam contra a produção, não contra uma cópia dela. A medição é o que um cliente veria a partir de cada localização, como saber se um registro está suficientemente atualizado para resolver. Esta é a base das seções posteriores sobre SLO, nas quais as mesmas sondas alimentam decisões de release.

## Um pouco de contexto
A Cloudflare tem foco em desempenho e segurança para os clientes, então seu código precisa ser testado constantemente, tanto em escala quanto na borda real que seus usuários utilizam. O Flamingo foi construído para esse propósito: medir o comportamento da produção e confirmar que o código em ambientes de nível inferior se comporta como esperado. O artigo observa que um post anterior tratou do uso do Flamingo em atualizações da pilha de CDN, mas que isso é apenas uma pequena parte de como ele é usado internamente. Uma de suas funcionalidades é uma arquitetura modular que permite às equipes ensinar outros protocolos de rede a ele, e o DNS é o protocolo sobre o qual este post trata.

### Testando em escala contra a borda real
Quando um sistema é amplamente distribuído, as requisições seguem muitos caminhos e atravessam vários sistemas antes de chegar à origem. Testar nessa escala é mais difícil do que testar um único componente, e o argumento do artigo é que testes automatizados constantes contra sistemas reais, com os resultados medidos, são necessários para julgar a saúde de um serviço.

## O que é o Flamingo?
O Flamingo começou como uma ferramenta para testes HTTP complexos e de várias etapas, e seu escopo cresceu consideravelmente desde então. Foi construído desde o início para a infraestrutura da Cloudflare. Possui um plano de controle e um agendador que permitem às equipes direcionar ambientes específicos, data centers ou percentuais de *metals* (palavra do artigo; provavelmente máquinas bare-metal, dada a frase seguinte). Também se integra a relatórios personalizados, métricas e análises, e precisa ter desempenho previsível em servidores bare-metal em ambientes com recursos restritos.

### Plano de controle e agendador
O plano de controle decide quais trabalhos são executados, onde e quando, enquanto os workers os executam. No Flamingo, esta é a camada que permite a uma equipe direcionar um teste para um ambiente, um data center ou uma fração de máquinas. Separá-la da execução é o que torna possíveis execuções direcionadas, no estilo canary, sem que cada equipe construa seu próprio agendamento. O artigo descreve isso como uma parte central do design de infraestrutura do Flamingo.

### Testes unitários versus testes de ponta a ponta
Testes unitários verificam o código isoladamente e não mostram como as requisições se comportam ao longo de caminhos reais. O artigo afirma claramente que apenas testes unitários não bastam em escala. O ponto é medir os resultados de requisições reais contra sistemas reais e usar esses resultados para decidir se um serviço está saudável.

> ❌ **Anti-padrão — Testes unitários como única rede de segurança:** enviar uma release com base em testes unitários que passam e dashboards verdes. **Por que as pessoas fazem isso:** o artigo não discute; testes unitários são rápidos e familiares, então parecem suficientes. **Consequência:** o incidente de abertura mostra o custo, já que a quebra chegou à produção com todos os testes verdes. **Correção:** executar testes automatizados constantes contra o sistema real e medir os resultados.

## Ser agradável de usar
O autor quer que as ferramentas internas sejam fáceis de usar, mas poderosas o bastante para mais de 80% dos casos de uso. Um novo framework de testes de aceitação de borda também precisa empurrar as equipes para diretrizes comuns e scripts reutilizáveis, em um ambiente estável, escalável e que gerencie a complexidade. A visão do autor é que depurar um teste é o oposto de agradável, e as escolhas de design abaixo decorrem disso.

> ❌ **Anti-padrão — Testes escritos em linguagem de propósito geral:** deixar cada equipe escrever testes como programas comuns em uma linguagem tradicional. **Por que as pessoas fazem isso:** o artigo não discute; linguagens de propósito geral são flexíveis e familiares. **Consequência:** quando um teste falha, o engenheiro precisa depurar tanto o sistema testado quanto o próprio código do teste. **Correção:** usar um formato restrito para os testes, como o Flamingo faz com seu DSL.

### DSL de configuração-requisição-resposta
O Flamingo usa uma linguagem específica de domínio (DSL) própria, construída em torno de um formato simples de configuração (setup), requisição (request) e resposta (response). O exemplo do artigo garante que todo tráfego não criptografado seja redirecionado para sua versão criptografada:

```text
TEST test-redirect
SET $hostname www.cloudflare.com
GET "http://$(hostname)/"
ASSERT STATUS IS 301
ASSERT HEADER Location IS "https://$(hostname)/"
```

**O que isso faz:**
- `TEST test-redirect` nomeia o teste.
- `SET $hostname` armazena o host uma vez, e a linha `GET` monta sua URL a partir dele com `$(hostname)`, de modo que o host é definido em um único lugar.
- `GET "http://..."` é a etapa de requisição, enviada por HTTP simples para que o comportamento de redirecionamento seja exercitado.
- As duas linhas `ASSERT` são a etapa de resposta: um status 301 e um cabeçalho `Location` que aponta para a versão HTTPS do mesmo host.
- O formato é declarativo, então uma falha aponta para o resultado, e não para a lógica do programa.

> 🔎 Contexto adicional: O status HTTP 301 significa um redirecionamento permanente. O cabeçalho `Location` informa ao cliente para onde ir.

O artigo diz que esse script de cinco linhas pode ser executado em qualquer ponto da borda da Cloudflare e escalar para milhares de requisições por minuto. Milhões de pontos de dados dessas execuções podem se transformar em um gráfico de como o serviço realmente se comporta no mundo real.

### Módulos
A etapa do meio do DSL normalmente é uma requisição. A jogada do artigo é tratar essa etapa como uma ação, de modo que o Flamingo deixe de ser uma ferramenta HTTP e passe a ser um sistema que pode executar qualquer ação cujo resultado você queira medir. Essas peças plugáveis são chamadas de módulos. O autor se empenhou em tornar sua escrita fácil para qualquer pessoa.

O Flamingo é escrito em Rust por causa de suas características consistentes de desempenho e de seu forte sistema de tipos. O artigo afirma que o sistema de tipos favorece a modularidade: enumerações fortemente tipadas para entrada e saída definem o contrato do executor de ações.

### Despacho de ações tipado
Cada protocolo tem uma variante de um enum de ações para suas entradas, e um enum de saída correspondente para seus resultados. Um tipo de handler é uma função assíncrona que recebe uma ação, um contexto compartilhado e uma configuração e retorna um resultado, e uma função de busca mapeia uma ação ao handler que a executa. O código Rust do artigo, como impresso:

```rust
pub enum Action {
  Query {
    method: String,
    url: Expression,
    parameters: Vec<http::QueryParameters>,
  },
  DnsQuery {
    query_name: Expression,
    query_type: Expression,
    parameters: Vec<dns::DnsQueryParameters>,
  },
}
pub enum ActionOutput {
  Http(http::FetchResponse),
  Dns(dns::QueryResponse),
}
pub type ActionHandlerFunction = Arc<
  dyn Fn(
      Action,
      Arc<Ctx>,
      InterpreterConfig,
    )
      -> BoxFuture<'static, Result<ActionOutput>>
    + Send
    + Sync
    + 'static,
>;
pub type ActionHandlerLUT = Arc<
  dyn Fn(&Action) -> ActionHandlerFunction
    + Send
    + Sync
    + 'static,
>;
```

**O que isso faz:**
- `Action` é um conjunto fechado de variantes: `Query` carrega um método HTTP, uma URL e parâmetros, e `DnsQuery` carrega um nome de consulta, um tipo de consulta e parâmetros de DNS.
- `ActionOutput` espelha isso, retornando uma resposta `Http` ou `Dns`, de modo que o tipo de resultado de cada handler é verificado em tempo de compilação.
- `ActionHandlerFunction` é uma função assíncrona compartilhável e encapsulada em box, o que permite que os handlers sejam armazenados e chamados independentemente uns dos outros.
- `ActionHandlerLUT` mapeia uma ação ao seu handler, de modo que o executor escolhe a implementação certa a partir do tipo da ação.
- Os limites `Arc`, `Send` e `Sync` permitem que os handlers sejam compartilhados com segurança entre threads. Este é um detalhe de Rust que o artigo não explica.

O resumo do autor é que a lógica para executar uma ação fica desacoplada em uma função de despacho dinâmico que pode ser chamada independentemente de outras ações. A programação dos testes e o gerenciamento de recursos ficam em um nível mais alto, de modo que quem escreve um módulo precisa se concentrar apenas na lógica.

O handler de módulo para HTTP é mostrado a seguir, como um trecho:

```rust
pub async fn query(
  action: Action,
  ctx: Arc<Ctx>,
  cfg: InterpreterConfig,
) -> Result<ActionOutput> {
  let Action::Query {
    method,
    url,
    parameters,
  } = action
  else {
    bail!("Invalid action for module-http::query: {action:?}");
  };
  ...
}
pub async fn execute_module<F>(
  ctx: Arc<Ctx>,
  cfg: &InterpreterConfig,
  handler_lut: F,
) -> Result<ActionOutput>
where
  F: (Fn(&Action) -> ActionHandlerFunction),
{
  let func = handler_lut(action);
  let action_out = (func)(action.clone(), ctx.clone(), cfg.clone()).await?;
  ...
}
```

**O que isso faz:**
- `query` desestrutura a variante `Query` com `let ... else`, e retorna um erro caso receba qualquer outra ação, de modo que um módulo lida apenas com o seu próprio tipo de ação.
- O `...` marca a implementação omitida no trecho.
- `execute_module` recebe a função de busca, encontra o handler da ação e o chama com a ação, o contexto e a configuração.
- No trecho como impresso, `action` é usada em `execute_module` sem ser vinculada como parâmetro, então o trecho é ilustrativo, e não completo.
- O design mantém o módulo focado na lógica de uma única ação, enquanto o executor cuida do agendamento e dos recursos.

## Usando o Flamingo para sondar DNS
O artigo abre esta seção com o ditado de que, quando surgem problemas, é sempre o DNS, e observa que há verdade na piada. O DNS é central para a Internet e, por depender de uma rede de fontes de verdade interconectadas, um único deslize pode causar uma tempestade de falhas. Ele é delicado porque é hierárquico: alterar um registro em uma zona da Cloudflare significa propagá-lo aos servidores autoritativos para que o registro atualizado fique amplamente disponível.

### Propagação de registros DNS
Os registros ficam armazenados em caches que estão mais próximos dos usuários, para reduzir a latência. A propagação é o tempo que um registro alterado leva para chegar aos servidores autoritativos e depois aos caches que atendem às consultas. Os atrasos importam porque operações sensíveis ao tempo, como a emissão de certificados e o failover de tráfego, dependem de o novo registro estar visível.

### TTL e cache
Cada registro tem um time to live (TTL) que informa ao cache local por quanto tempo ele pode manter o registro antes de buscá-lo novamente no servidor autoritativo. Um TTL longo faz as alterações demorarem mais a aparecer, e é por isso que o artigo trata o atraso de propagação como algo a ser medido, e não presumido.

### Servidores autoritativos e resolvedores públicos
A sonda mede duas coisas: a rapidez com que uma alteração chega aos nameservers autoritativos e a rapidez com que ela aparece através do resolvedor público 1.1.1.1. São verificações separadas porque os dois saltos têm expectativas de atualidade diferentes no design do artigo.

A equipe de DNS adicionou um módulo de DNS em vez de um novo serviço de sondagem. Como o framework já cuidava do agendamento, dos relatórios e da integração com o pipeline, a equipe obteve essas capacidades "de graça". A equipe também não precisou gerenciar um novo serviço de sondagem nem integrá-lo aos relatórios.

A sonda funciona assim. Ela atualiza continuamente um registro TXT com um timestamp e, a partir de cada data center, consulta três IPs de nameservers autoritativos e o 1.1.1.1. Ela verifica que a atualidade está dentro de 90 segundos para os servidores autoritativos e dentro de 600 segundos para o resolvedor.

### Teste de propagação de DNS
O exemplo do artigo sonda diretamente um nameserver autoritativo:

```text
TEST test-propagation-delay-direct-108-162-193-62
MOD dns
SET $hostname "some.probe.domain"
SET $authNs 108.162.193.62:53 # amir.ns.cloudflare.com
QUERY $hostname TXT
TARGET $authNs
ASSERT DNS.RCODE IS NOERROR
ASSERT DNS.FLAGS CONTAINS AA
ASSERT DNS.ANSWERS[0] GT SUBTRACT(TIMESTAMP(), "90")
```

**O que isso faz:**
- `MOD dns` seleciona o módulo de DNS, que é o mecanismo que permite que a mesma DSL execute uma ação de DNS.
- `SET $hostname` nomeia o registro da sonda. O valor `some.probe.domain` é um placeholder no artigo.
- `SET $authNs` e `TARGET $authNs` fixam a consulta a um servidor autoritativo específico na porta 53. O comentário final nomeia esse servidor, o que é útil na leitura dos resultados.
- `QUERY $hostname TXT` pede o registro TXT, que carrega o timestamp que a sonda escreve.
- As duas primeiras linhas `ASSERT` verificam que a resposta é bem-sucedida e que é autoritativa. A terceira verifica a atualidade: o timestamp na resposta deve ser posterior ao horário atual menos 90 segundos.

> 🔎 Contexto adicional: `NOERROR` é o código de resposta DNS para sucesso. A flag `AA` significa que o servidor que respondeu afirma ter autoridade sobre a zona.

## Capturando bugs (SLO e contínuo)
O tráfego sintético constante através da produção constrói um retrato do que os usuários reais experimentam e alimenta os SLOs. Quando o tráfego sintético encontra problemas, o autor está confiante de que os clientes também os encontram, e assim a equipe pode fazer a triagem de acordo.

### SLI e SLO
Um objetivo de nível de serviço (SLO) é uma meta percentual para alguma métrica. Um indicador de nível de serviço (SLI) é a medição que acompanha essa meta, com foco na experiência ponta a ponta do usuário. O ponto do artigo é que as sondas são SLIs nesse sentido, porque medem o que um usuário veria, e não o que o serviço informa sobre si mesmo.

### Orçamento de erro e taxa de consumo
Um orçamento de erro (error budget) é a fração de indisponibilidade que um serviço tolera. A taxa de consumo (burn rate) é a velocidade com que esse orçamento está sendo usado em um dado momento. O exemplo do artigo contrasta dois casos: consumir 10% do orçamento em três dias é um problema de longa duração, que não é urgente, mas ainda afeta os clientes; consumir 10% em uma hora significa que um incidente está ocorrendo, e o impacto para os clientes será substancial, a menos que seja corrigido imediatamente. Como os mesmos números carregam os dois significados, um sistema automatizado pode julgar a gravidade a partir deles.

### Controle de releases
Durante uma implantação, a equipe observa os SLOs para julgar a saúde do serviço. Com base nisso, os sistemas de release recebem a instrução de promover a mudança ou revertê-la. O artigo apresenta isso como a razão de as medições importarem: elas transformam a observação em uma decisão que não exige que uma pessoa interprete cada dashboard.

## O caminho adiante
Mover o monitoramento de DNS para o Flamingo significa que o sistema pode detectar problemas de DNS antes que eles se espalhem pela borda. O artigo dá um exemplo: se o Flamingo detectar um pico na latência de propagação em um data center canary específico, ele pode interromper automaticamente o rollout de uma nova versão de software antes que ela afete o restante da Internet.

### Interrupção no canary
Um data center canary é um local pequeno e inicial onde uma mudança é implantada primeiro. Um pico de latência ali é tratado como um sinal para parar o rollout. A afirmação do artigo é que isso protege a Internet mais ampla contra uma release ruim, porque as sondas captam o problema no canary antes que a mudança chegue a outras localidades.

### Cobertura de protocolos
DNS é só o começo. O Flamingo já tem módulos para TCP, UDP, WebSockets e vários outros. A conclusão do artigo é que testes e monitoramento rápidos e automatizados dos serviços permitem que as equipes implantem mudanças grandes com segurança, sem que uma pessoa precise vigiar constantemente.

## 🧠 Modelos mentais
### Sondas sintéticas como a visão do cliente
**O modelo:** Sondas que rodam continuamente contra a produção medem o que os usuários experimentam, então podem substituir a espera por relatos de usuários.
**Como aplicar:** Meça o resultado que um cliente veria, como atualidade, status ou tempo de resolução, a partir de cada localização, em vez de confiar nos relatórios de saúde do próprio componente.

### Taxa de consumo do orçamento de erro
**O modelo:** Um orçamento de erro é quanta falha um serviço pode tolerar, e a taxa de consumo é a velocidade com que ele está sendo gasto.
**Como aplicar:** Julgue a gravidade pela taxa, além do total. Exemplo do artigo: 10% do orçamento em três dias é um problema lento, enquanto 10% em uma hora é um incidente.

### Módulos em vez de testes sob medida
**O modelo:** Mantenha um único formato fixo de script de configuração-requisição-resposta e troque, para cada protocolo, um módulo de ação.
**Como aplicar:** Quando um novo protocolo precisar de testes, escreva um módulo que execute a ação e retorne uma saída tipada. O agendamento, os relatórios e a DSL vêm junto, como aconteceu com o DNS.

## 🔑 Principais conclusões
- Falhas de DNS podem deixar um serviço saudável incapaz de fazer novos hostnames resolverem ou de emitir certificados, por isso precisam de medição própria.
- Sondar de todos os data centers, contra a produção, captura falhas parciais que testes unitários e dashboards verdes podem não enxergar.
- Adicionar um módulo de ação de DNS ao Flamingo deu à equipe de DNS agendamento, relatórios e escala sem construir um novo serviço de sondagem.
- A sonda verifica que a atualidade está dentro de 90 segundos para os servidores autoritativos e dentro de 600 segundos para o resolvedor 1.1.1.1.
- Orçamentos de erro e taxas de consumo permitem que sistemas automatizados julguem a gravidade: 10% do orçamento em uma hora é um incidente, enquanto 10% em três dias não é urgente.
- Sondas sintéticas contínuas podem controlar releases, promovendo uma mudança ou revertendo-a com base em medições de SLO.
- Um pico de latência de propagação em um data center canary pode interromper um rollout antes que ele alcance o restante da Internet.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Na governança de arquitetura, o artigo mostra uma forma concreta de transformar confiabilidade de um dashboard em um portão de release: definir uma medida de fora para dentro do que os usuários veem, e deixar o pipeline de implantação agir com base nela. O mesmo padrão poderia se aplicar a um gateway de IA ou a uma plataforma de agentes, em que um serviço com aparência saudável ainda pode dar respostas desatualizadas ou erradas. A pergunta que ele levanta para você: quais dos seus sistemas podem falhar de formas que testes unitários e uma página de status saudável nunca revelariam, e qual sonda de fora para dentro expusesse essa falha?

## 📚 Plano de leitura
### Do artigo
- Nenhuma URL está presente no texto extraído, então nenhum link é escrito aqui. O artigo menciona um post anterior da Cloudflare sobre o uso do Flamingo em atualizações da pilha de CDN, e uma página de carreiras; esses links não foram extraídos.
### Para aprofundar
- Objetivos de nível de serviço, orçamentos de erro e alertas de taxa de consumo (prática de SRE; o livro Google SRE trata disso em profundidade) — o modelo de SLO do artigo, explicado em detalhes.
- Cache e comportamento de TTL em DNS (as especificações centrais de DNS, RFC 1034 e RFC 1035) — para entender como respostas autoritativas e TTLs interagem.
- Canary e entrega progressiva com rollback automatizado — o padrão de controle de releases do qual o artigo depende.

## 🔗 Conceitos
- [control-plane](/pt-br/concepts/cloud-infra/control-plane.md)
- [canary-deploy](/pt-br/concepts/software-engineering/canary-deploy.md)
- [SLO error budget](/pt-br/concepts/cloud-infra/slo-error-budget.md)
