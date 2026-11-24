---
type: Article Summary
title: 'Mais controle, menos dependências: por que a DORA está forçando uma revisão da conectividade'
description: Vários provedores de nuvem não agregam resiliência se compartilham os mesmos caminhos de rede; rastreie os serviços críticos até a conectividade para encontrar pontos de falha compartilhados.
resource: https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html
tags:
- architecture
- technology-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html
  title: 'More control, fewer dependencies: Why DORA is forcing a connectivity rethink'
  author: Thomas King
status: stable
source_url: https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html
source: CIO.com
source_type: blog
published: '2026-11-24'
domain: architecture
subdomain: technology-architecture
concepts:
- correlated-failure
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81869ea5eb4fa3fa2281
foundation_candidates:
- business-continuity
- resilience
- single-point-of-failure
lang: pt-br
translation_of: /notes/architecture/2026-11-24-more-control-fewer-dependencies-why-dora-is-forcing-a-connec.md
---

## 🎯 Tese central
Várias nuvens e provedores não garantem resiliência se dependem das mesmas conexões de rede, por isso os CIOs precisam saber o que acontece quando essas conexões falham.

## 🧭 Por que o autor escreveu o artigo
O autor, Thomas King, abre com um banco cujos investimentos estavam distribuídos entre vários fundos, todos expostos à mesma empresa, e cuja equipe de risco teria perguntas difíceis a responder. Ele argumenta que os CIOs deveriam aplicar o mesmo ceticismo à arquitetura de rede. Se ele estiver certo, a conectividade precisa fazer parte das decisões de investimento e de continuidade antes que um serviço de nuvem seja escolhido, e não depois que ele já estiver em operação.

## Abertura: resiliência depende de independência
O artigo abre com a afirmação de que distribuir aplicações entre provedores não ajuda se os caminhos até elas convergem. O autor diz que "quando distribuímos aplicações entre diferentes provedores, precisamos saber onde a sua aparente 'independência' realmente termina."

### Independência de um fallback
Um fallback só protege você se ele não estiver exposto às mesmas variáveis do principal. O autor afirma isso de forma direta: "um plano B só funciona se não estiver exposto ao mesmo conjunto de variáveis do Plano A." Uma segunda nuvem pode abrir mais opções, mas seu valor como fallback depende de as pessoas ainda conseguirem alcançá-la quando algo falha. Se as aplicações nas duas nuvens continuam rodando, mas o caminho até elas passa pela mesma infraestrutura, uma única disrupção ainda pode deixar os clientes sem acesso. Redundância é, portanto, uma questão de caminhos compartilhados, e não de contagem de provedores.

### Continuidade de negócio em nível de conselho
O autor chama a falta de continuidade de negócio de "algo difícil para um CIO e sua equipe explicarem ao conselho", especialmente quando a empresa investiu pesadamente em resiliência. É por isso que ele argumenta que a conectividade "merece um lugar à mesa principal". A preocupação é menos com a elegância técnica do que com a capacidade de a liderança explicar como os clientes mantêm o acesso durante uma disrupção.

### Exposição compartilhada e a analogia do banco
O exemplo do banco sustenta o argumento. Um banco que mantém vários fundos que possuem a mesma empresa não está diversificado, e sua equipe de risco faria perguntas sobre essa exposição. O autor pede aos CIOs que examinem a rede da mesma forma, para ver "onde a sua aparente 'independência' realmente termina". A analogia é a forma como o artigo ensina os demais pontos, e ela reaparece na seção final.

## Uma regulação europeia, um problema americano conhecido
O autor argumenta que o Digital Operational Resilience Act (DORA), regulamento da UE, levanta questões que os CIOs dos EUA também deveriam fazer.

### Digital Operational Resilience Act (DORA)
A DORA obriga as instituições financeiras a examinar as dependências por trás de seus serviços digitais, e grande parte delas remonta à arquitetura de rede. Seus requisitos vão desde a gestão do risco de tecnologia até os testes de resiliência, com um mandato para reportar incidentes graves e supervisionar provedores externos. O autor diz que o que diferencia a DORA de outras regulações é que ela leva as instituições a examinar o quanto a capacidade de atender clientes depende de decisões e de infraestrutura amplamente fora do seu controle. Ele acrescenta que os CIOs dos EUA "deveriam prestar atenção", já que "os clientes de um banco americano esperam alcançar o seu dinheiro independentemente de onde seus fornecedores de tecnologia estão sediados". A DORA dá aos CIOs um motivo para fazer a pergunta que o autor coloca no centro do artigo: "O que pode falhar aqui? E o que continuará funcionando quando isso acontecer?"

### Risco de terceiros e os números de incidentes
O primeiro relatório conjunto das Autoridades Europeias de Supervisão constatou que 29% dos principais incidentes de tecnologia reportados no setor financeiro da UE em 2025 tiveram origem em terceiros. Cerca de um terço teve efeitos transfronteiriços, que o autor relaciona à infraestrutura e aos serviços compartilhados que conectam as instituições financeiras. Ele observa que esses números abrangem uma ampla gama de falhas, por isso não comprovam uma causa de rede, mas dão aos CIOs de todo lugar um motivo convincente para verificar onde estão as próprias dependências. Ele também cita o OCC, agência federal dos EUA que supervisiona bancos nacionais, cujo relatório de risco Spring 2025 destacou a resiliência operacional e a gestão do risco de terceiros, apontando disrupções em serviços financeiros e em outros setores.

### Avaliação de fornecedores e garantia de recuperação
O primeiro passo que o autor propõe é mudar a forma como os fornecedores são avaliados. Um provedor com processos fortes de recuperação para os próprios serviços não fica, por isso, imune a disrupções. As empresas precisam entender onde estão as dependências do provedor, inclusive no nível da infraestrutura. Isso exige uma conversa entre as pessoas que compram os serviços, os engenheiros que os conectam e os líderes responsáveis pela continuidade de negócio, e o autor quer que essa conversa acompanhe um serviço crítico até o ponto em que as dependências por baixo dele fiquem expostas.

> ❌ **Anti-padrão — aceitar alegações de recuperação pelo valor de face:** tratar a própria história de recuperação de um provedor como prova de que o serviço sobreviverá a uma disrupção. **Por que as pessoas fazem isso:** o artigo não diz; a documentação de recuperação do provedor é a evidência mais fácil de obter. **Consequência:** "corremos o risco de aprovar um plano de recuperação cujo sucesso depende de premissas que ninguém testou." **Correção:** rastrear as dependências do provedor até o nível da infraestrutura e testar o plano de recuperação contra elas.

## Seguindo as dependências por baixo da nuvem
Esta seção é o método. O autor parte de um serviço voltado ao cliente e segue para baixo.

### Rastreando um serviço crítico de cima para baixo
O autor começaria com um serviço do qual os clientes dependem, como fazer um pagamento por meio de um aplicativo de internet banking, e rastrearia o que precisa funcionar para que o pagamento seja concluído. O aplicativo pode rodar em uma nuvem enquanto recorre a um serviço de detecção de fraude em outra e a um sistema core bancário em outro lugar. Cada parte pode ter seus próprios arranjos de backup, mas saber se o pagamento ainda pode prosseguir exige olhar mais fundo do que esses arranjos. O método é escolher um serviço, listar suas dependências e continuar descendo até a infraestrutura por baixo de cada uma.

### Falha correlacionada por rotas compartilhadas
O autor mostra como conexões aparentemente separadas podem compartilhar um único ponto. Duas conexões contratadas de operadoras diferentes podem usar a mesma rota de fibra subjacente. Rotas totalmente separadas podem entrar em um prédio pelo mesmo duto, ou convergir em uma instalação da qual ambas dependem. Uma falha nesse ponto compartilhado pode eliminar a conexão principal e o seu backup ao mesmo tempo, o que leva à pergunta dele: "isso é realmente um backup?"

🔎 Contexto adicional: Um corte de fibra é uma ruptura física em um cabo óptico. Um duto é uma tubulação de proteção que pode carregar muitos cabos, então cabos que compartilham um mesmo duto podem ser danificados pelo mesmo evento.

> ❌ **Anti-padrão — contar operadoras como independência:** presumir que duas conexões de operadoras diferentes são caminhos separados. **Por que as pessoas fazem isso:** o artigo não diz; operadoras diferentes parecem rotas diferentes em um contrato. **Consequência:** um corte de fibra, o rompimento de um duto ou a falha de uma instalação compartilhada eliminam tanto a conexão principal quanto a de backup. **Correção:** pedir a rota real de cada conexão e as evidências de que os caminhos são separados.

### Perguntas de compras e sinais de alerta
O autor quer que essa discussão chegue ao setor de compras. Um CIO deve pedir aos fornecedores que expliquem como suas rotas propostas diferem e quais evidências sustentam essa separação. Se esse detalhe for difícil de obter, ou se o provedor não for transparente, isso é "seu primeiro sinal de alerta". Quando as dependências ficam visíveis, o negócio pode decidir onde conexões alternativas ajudam. Ele priorizaria os serviços que mais afetam diretamente os clientes, dando-lhes uma rota viável durante uma disrupção, "com capacidade suficiente para sustentar facilmente esses serviços".

### Internet Exchange (IX)
Um Internet Exchange reúne redes para que possam trocar tráfego diretamente. Onde as redes relevantes estão presentes e concordam em se interconectar, o tráfego pode seguir um caminho mais curto, com menos redes intermediárias. Trocar tráfego localmente também evita trajetos desnecessários até hubs distantes, o que torna o desempenho mais previsível. O autor é claro ao dizer que um IX não é exigido pela DORA.

### Conectividade privada com a nuvem e plataformas de interconexão
Por meio de uma plataforma de interconexão, uma instituição pode estabelecer conexões dedicadas com provedores de nuvem, o que lhe dá maior controle sobre como o tráfego chega a esses serviços. Conexões privadas entre redes e conexões privadas entre nuvens atendem a necessidades diferentes. O ponto do autor é que ambas ajudam os engenheiros a construir caminhos mais fáceis de entender e de gerenciar.

> ❌ **Anti-padrão — recriar a dependência por meio da nova plataforma:** migrar para uma instalação de interconexão enquanto todas as rotas até ela ainda compartilham um mesmo link vulnerável. **Por que as pessoas fazem isso:** o artigo não diz; a nova plataforma é a parte que recebe atenção e orçamento. **Consequência:** "uma instituição pode, sem perceber, recriar sua dependência original." **Correção:** aplicar às conexões de acesso à instalação o mesmo escrutínio aplicado à infraestrutura dentro dela, e verificar se elas seguem rotas fisicamente diversas.

### Testando o caminho que sobrevive
O autor julga os projetos pelos testes. Se a conexão principal cair, o caminho que sobrou precisa carregar o tráfego necessário mantendo o serviço utilizável. Ele insiste no significado de "utilizável": "mesmo um pagamento que eventualmente se conclui após repetidos timeouts continua sendo uma disrupção grave aos olhos do cliente."

> ❌ **Anti-padrão — contar o sucesso eventual como recuperação:** tratar uma transação que se conclui após repetidos timeouts como um failover bem-sucedido. **Por que as pessoas fazem isso:** o artigo não diz; a transação ainda termina, então a indisponibilidade parece resolvida nos logs. **Consequência:** o cliente sofre uma disrupção grave que o plano de recuperação registra como aprovada. **Correção:** testar se o serviço continua utilizável no caminho que sobrou, e não apenas se ele eventualmente se conclui.

### Soberania digital
O autor enxerga a interconexão como uma forma de exercer maior controle sobre a infraestrutura da qual o negócio depende. Uma instituição que cumpre suas obrigações da DORA ainda pode fortalecer sua conectividade com a internet por meio de rotas mais independentes e conexões diretas com os serviços online que utiliza. Ele diz que isso é o que soberania digital significa aqui: "ter mais voz sobre como o tráfego trafega e mais opções quando um provedor ou caminho fica indisponível." A opção só é útil se as conexões forem "projetadas e testadas com o mesmo cuidado".

🔎 Contexto adicional: O autor usa o termo de forma restrita, para o controle sobre os caminhos do tráfego e as opções de fallback. Significados mais amplos, como a legislação de residência de dados, não são discutidos no artigo.

## Dando à conectividade um lugar na sala do conselho
A última seção retorna à analogia do banco e a transforma em uma regra de decisão.

### Diversificação por transparência (look-through)
Um banco não consideraria seus investimentos diversificados só porque possui vários fundos; ele examinaria o que os fundos possuem e se compartilham exposição à mesma empresa. Para os CIOs, escolher vários provedores é um começo, mas as conexões precisam ser rastreadas a fundo o suficiente para ver se uma única falha poderia deixar todos inalcançáveis.

### Conectividade nas decisões de investimento
O autor defende que a conectividade precisa entrar na conversa enquanto as decisões de investimento ainda estão sendo tomadas. Se um novo serviço de nuvem deve sustentar uma função essencial do negócio, suas conexões pertencem à discussão sobre como essa função sobreviverá a uma disrupção. O custo de uma rota independente pode então ser comparado às consequências de perder o acesso, e os líderes de negócio devem participar da decisão sobre o nível de risco que a empresa aceitará.

### A DORA como oportunidade para líderes de tecnologia nos EUA
O autor vê o escrutínio da DORA como uma chance para os líderes de tecnologia dos EUA examinarem suas próprias escolhas, fazerem melhores perguntas aos fornecedores e tornarem os planos de recuperação mais críveis. Seu teste final é se alguém consegue explicar "como um cliente ainda alcançará um serviço essencial quando sua conexão habitual desaparecer". Ele descreve essa pergunta, seguida até a arquitetura, como o ponto em que "o trabalho real começa".

## 🧠 Modelos mentais
### Teste de independência
**O modelo:** um fallback só é real se não estiver exposto às mesmas variáveis do principal, como em "um plano B só funciona se não estiver exposto ao mesmo conjunto de variáveis do Plano A."
**Como aplicar:** para cada backup, pergunte "o que pode falhar aqui? E o que continuará funcionando quando isso acontecer?", e rastreie fibra, dutos e instalações compartilhados até que os dois caminhos deixem de convergir.

### Diversificação por transparência
**O modelo:** um conjunto de investimentos só é diversificado quando você olha através dele até o que cada item possui. Vários fundos que compartilham uma mesma empresa subjacente são concentrados, independentemente da quantidade.
**Como aplicar:** conte as dependências compartilhadas, e não os provedores. Liste as conexões de cada provedor e os pontos físicos e operacionais que elas compartilham.

### Rastreie o serviço crítico
**O modelo:** comece por um serviço do qual os clientes dependem, como um pagamento, e siga o que precisa funcionar para que ele tenha sucesso, até a infraestrutura sob cada dependência.
**Como aplicar:** priorize os serviços que mais afetam os clientes e verifique se o pagamento ainda prossegue quando qualquer caminho principal falha.

### Teste o caminho que sobrevive
**O modelo:** um backup só é comprovado quando o caminho que sobrou carrega o tráfego necessário e o serviço continua utilizável, e não apenas quando eventualmente se conclui.
**Como aplicar:** execute testes de failover com o perfil de tráfego real e julgue o resultado pela experiência do cliente, incluindo timeouts repetidos.

## 🔑 Principais conclusões
- Uma segunda nuvem não é um backup real se seu caminho passa pela mesma infraestrutura da primeira, porque uma única disrupção pode deixar os clientes sem acesso enquanto as aplicações continuam rodando.
- Duas conexões de operadoras diferentes podem compartilhar uma mesma rota de fibra, duto ou instalação, então uma única falha pode remover a principal e o backup juntos.
- O caso de teste do autor é um pagamento de cliente, rastreado pelo aplicativo, por um serviço de detecção de fraude, por um sistema core bancário e pela rede por baixo de cada um.
- A forte capacidade de recuperação de um provedor não prova que ele está imune a disrupções; o autor trata a falta de evidências de rota como "seu primeiro sinal de alerta".
- O primeiro relatório conjunto das Autoridades Europeias de Supervisão constatou que 29% dos principais incidentes de tecnologia no setor financeiro da UE em 2025 tiveram origem em terceiros, e cerca de um terço teve efeitos transfronteiriços.
- A interconexão direta por meio de Internet Exchanges e de links privados com a nuvem pode encurtar caminhos e ampliar o controle, mas os links de acesso a essas instalações precisam do mesmo escrutínio que tudo o que está dentro delas.
- A conectividade pertence às decisões de investimento, nas quais o custo de uma rota independente é comparado às consequências de perder o acesso, cabendo aos líderes de negócio decidir o risco aceitável.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Tecnologia (principal: operadoras, rotas de fibra, dutos, conexões com a nuvem, IX e plataformas de interconexão). Aplicação (um app de pagamentos que depende de um serviço de detecção de fraude e de um sistema core bancário). Negócio (continuidade de negócio, apetite de risco do conselho, acesso do cliente ao seu dinheiro). Dados: não coberto. |
| Escopo | Segmento ou capacidade: o caminho de um serviço crítico para o cliente, um pagamento, por vários provedores e pela infraestrutura por baixo deles. Não é corporativo como um todo, nem um projeto único. |
| Stakeholders e preocupações | CIOs e suas equipes (continuidade, explicar a exposição ao conselho). Conselho e líderes de negócio (risco aceitável, custo de uma rota independente versus a perda de acesso). Compras (evidências dos fornecedores sobre a separação de rotas). Engenheiros (caminhos fáceis de entender e de gerenciar). Reguladores, por meio da DORA e do OCC (risco de terceiros, reporte de incidentes). Clientes (alcançar seu dinheiro quando um caminho falha). |
| Frameworks e notações | Nenhum mencionado. DORA e o relatório de risco do OCC são referências regulatórias, não frameworks de arquitetura. |

### Decisões e trade-offs
- **Profundidade da avaliação de fornecedores:** as opções são aceitar os processos de recuperação de um provedor ou rastrear suas dependências até a infraestrutura. O autor recomenda rastrear, acompanhando um serviço crítico até que as dependências compartilhadas fiquem expostas. O custo é uma conversa mais longa entre compradores, engenheiros e líderes de continuidade, e os fornecedores podem precisar divulgar detalhes de rota.
- **Onde ficam as conexões alternativas:** o autor recomenda priorizar os serviços que mais afetam os clientes, com uma rota viável durante uma disrupção e capacidade suficiente. O custo é a capacidade e a despesa de rotas independentes, que, segundo ele, devem ser pesadas contra as consequências de perder o acesso.
- **Interconexão direta versus trânsito por redes intermediárias:** o autor recomenda a troca direta onde as redes concordam em se interconectar, e conexões dedicadas com a nuvem onde se precisa de mais controle. Os benefícios são caminhos mais curtos, desempenho mais previsível e mais controle. Os custos são que os links de acesso precisam do mesmo escrutínio, o desenho precisa ser testado, e um IX não é exigido pela DORA.
- **Quando a conectividade é decidida:** o autor recomenda trazer a conectividade para as decisões de investimento de novos serviços de nuvem, e não depois que eles já estiverem rodando. O custo é o envolvimento antecipado de líderes de rede e de negócio, e uma conversa mais difícil sobre a aceitação de risco.

### Fundamentos em que se apoia
- [Arquitetura Empresarial (EA)](/pt-br/foundations/enterprise-architecture.md) — o artigo a usa para trazer a conectividade para as decisões de investimento e de risco, o que estende "conduzir mudanças" às decisões tomadas por líderes de negócio.
- [Arquitetura de Tecnologia](/pt-br/foundations/technology-architecture.md) — o artigo rastreia a estrutura abaixo de um serviço até operadoras, fibra e dutos, o que é mais granular do que "serviços e componentes de tecnologia" e se concentra em caminhos.
- [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md) — o artigo aplica um serviço crítico em vários provedores, o que é mais amplo do que o escopo de projeto único da definição de trabalho.
- [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md) — o artigo abrange os domínios de tecnologia, aplicação e negócio, e não cobre dados.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um Staff Architect, o passo prático é tornar visíveis as dependências de fornecedores e de rotas no repositório de arquitetura e vinculá-las aos objetivos de recuperação de cada serviço. No trabalho de arquitetura do MBA, a seção sobre o conselho é uma questão de apetite de risco: os líderes de negócio devem decidir quanto risco de rota aceitar, com a arquitetura tornando o trade-off visível. A pergunta que isso levanta para você: para um dos seus serviços críticos, você consegue nomear o ponto físico compartilhado, como uma operadora, um duto ou uma instalação, cuja falha única derrubaria tanto a conexão principal quanto a de backup?

## 📚 Plano de leitura
### A partir do artigo
- [More control, fewer dependencies: Why DORA is forcing a connectivity rethink](https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html) — o original completo, já que o artigo não cita links e a nota não omite nada dele.
### Para se aprofundar
- Digital Operational Resilience Act (DORA) — o regulamento da UE em torno do qual o artigo gira, com seus requisitos de testes, reporte de incidentes e supervisão de terceiros.
- O primeiro relatório conjunto das Autoridades Europeias de Supervisão sobre incidentes graves de tecnologia — a fonte do número de 29% de origem em terceiros.
- O relatório de risco Spring 2025 do OCC — a visão dos EUA sobre resiliência operacional e gestão do risco de terceiros.
- Internet Exchanges e plataformas de interconexão — como o peering direto encurta caminhos e onde entram os links de acesso.
- Testes de continuidade de negócio para serviços críticos — como testar failover com tráfego real e metas de usabilidade.

## 🔗 Conceitos
- [correlated-failure](/pt-br/concepts/correlated-failure.md)
