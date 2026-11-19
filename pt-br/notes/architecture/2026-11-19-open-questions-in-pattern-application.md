---
type: Article Summary
title: Questões em Aberto na Aplicação de Padrões
description: Um padrão só é aplicado quando existem juntos uma decisão registrada, um registro nos elementos de design e uma verificação de conformidade.
resource: https://www.architectureandgovernance.com/uncategorized/open-questions-in-pattern-application
tags:
- architecture
- integration-and-software-architecture
- adr
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://www.architectureandgovernance.com/uncategorized/open-questions-in-pattern-application
  title: Open Questions in Pattern Application
  author: Architecture & Governance Magazine
status: stable
source_url: https://www.architectureandgovernance.com/uncategorized/open-questions-in-pattern-application
source: Architecture & Governance Magazine
source_type: blog
published: '2026-11-18'
domain: architecture
subdomain: integration-and-software-architecture
concepts:
- adr
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d8105950edfa52c54f634
lang: pt-br
translation_of: /notes/architecture/2026-11-19-open-questions-in-pattern-application.md
---

## 🎯 Tese central
Um padrão só é aplicado quando existem juntos uma decisão, o padrão registrado nos elementos de design que ele realmente toca e uma verificação de que o design corresponde ao padrão.

## 🧭 Por que o autor escreveu o texto
Arquitetos dizem que "aplicam padrões" e "usam registros de decisão", mas, quando pedidos para mostrar onde, a resposta costuma faltar; o autor afirma que isso é repetível em cerca de 1 em cada 500 entrevistas ou sessões de mentoria. Se a tese do autor se confirmar, um padrão poderia ser rastreado da biblioteca até a decisão, o design e o código, e verificado, em vez de argumentado de memória. O autor afirma que o PAL, a ferramenta que descreve, está projetado, mas ainda não está em execução.

## Abertura: o problema da repetibilidade e o esforço de pesquisa

O artigo começa com perguntas: quando aplicar um padrão e quando não aplicar, como um praticante aprendeu isso e qual artefato de design mostra qual padrão se aplica. O autor diz que o campo está cheio de afirmações de que padrões e decisões deveriam ser "incríveis", e de muitas histórias em que não funcionam.

### Uma biblioteca de referência com 200 padrões
O grupo do autor lançou uma biblioteca de 200 padrões industriais comuns e experimentais para arquitetura. Ele diz que ela está longe de estar completa, mas é um excelente começo, e que o trabalho consumiu um ano de sua vida. Antes dela não havia um ponto de referência comum, e os repositórios dos fornecedores discordavam entre si. A biblioteca é a fonte da qual o PAL extrai suas recomendações e verificações.

### Repetibilidade como questão de pesquisa
O autor quer rigor científico ou semicientífico nos métodos de arquitetura: técnicas reutilizáveis com as quais outros possam ter sucesso, construídas a partir de conversas com muitos praticantes. Ele descreve o esforço como uma parceria com Shawn McCarthy, arquiteto-chefe da Manulife e presidente do Chief Architect Forum, além de quatro estudantes e um professor da UC Denver. A lacuna que ele ataca é que o sucesso repetível é raro.

> ❌ **Anti-padrão — Adoção de padrões guiada por marketing:** Slogans como "Apply Patterns! Use Decision Records! Three easy steps." **Por que as pessoas fazem isso:** não declarado no artigo. **Consequência:** o autor relata que a repetibilidade é absolutamente deplorável. **Correção:** construir técnicas a partir de evidências coletadas com muitos praticantes, que é a abordagem declarada pelo autor.

## Ergane e o que está sendo construído

O texto extraído inclui a legenda "Context View of Ergane in Ergane"; a imagem em si não está no texto.

### Ergane, a ferramenta de design baseada no BTABoK
Ergane é a ferramenta de design baseada no BTABoK do autor, para entender, testar e experimentar arquitetura. O PAL foi projetado em um board do Ergane, de modo que o Ergane é tanto a ferramenta usada para projetar o PAL quanto o ambiente em que o PAL está. O artigo descreve o PAL como chamado apenas pelo board service.

### PAL, projetado, mas ainda não em execução
O autor é explícito: o PAL ainda não está em execução. O que existe é um projeto, e ele diz que preferiria mostrar o design a vender uma demo. O restante do artigo, portanto, descreve comportamento pretendido e artefatos de design, não resultados medidos.

## O que o PAL faz

O PAL é um serviço que mantém a biblioteca de padrões do BTABoK e tem duas funções. A primeira recomenda padrões que se encaixam em um cartão de design em que o arquiteto está trabalhando. A segunda, que o autor diz ser a que mais lhe importa, verifica se os padrões já escolhidos de fato aparecem no design da forma como a biblioteca os descreve.

### Recomendação de padrões
A primeira função recebe um cartão de design e sugere padrões que possam se encaixar nele. O artigo apresenta essa como a primeira função, mas diz pouco sobre como o encaixe é julgado.

### Verificação de conformidade de padrões
A segunda função verifica os padrões escolhidos contra o design. O autor observa que quase ninguém faz isso: pergunte onde um padrão está no design e no código, e a conversa se cala. Este é o elo que transforma "nós aplicamos padrões" em algo inspecionável.

> ❌ **Anti-padrão — Padrão alegado, mas não demonstrado:** Dizer que um padrão é usado sem apontar onde ele está no design ou no código. **Por que as pessoas fazem isso:** não declarado no artigo; o autor observa que as pessoas conseguiam nomear os padrões que "usaram", mas quase ninguém conseguia mostrar onde. **Consequência:** o padrão "vivia na cabeça de alguém", então nenhuma revisão pode verificá-lo. **Correção:** registrar o padrão nos elementos de design que ele toca e verificá-lo contra eles, que é o que o PAL foi projetado para fazer.

## Nós o projetamos usando o método

Cada parte do PAL foi projetada em um board do Ergane usando os viewpoints do BTABoK, como sete cartões, cada um respondendo a uma pergunta diferente.

### Viewpoints do BTABoK
Cada cartão responde à sua pergunta a partir de um viewpoint, e nenhum cartão isolado dá a resposta completa. A resposta do autor para "qual artefato de design mostra qual padrão se aplica" são vários deles, cada um por um ângulo diferente.

### Visão de contexto primária
Este cartão mostra onde o PAL se situa entre tudo o mais no Ergane. O PAL é um serviço de domínio na rede privada, chamado apenas pelo board service. O artigo diz que o leitor já viu este cartão.

### Requisito arquiteturalmente significativo
Um requisito arquiteturalmente significativo é aquele que muda a forma do sistema se for mal atendido. O requisito do PAL é que ele deve recomendar e verificar padrões a partir da biblioteca publicada atual sem desacelerar o board. O autor diz que este requisito orienta metade do design.

### Cartão de atributo de qualidade com seis cenários
Um cenário é uma situação específica com uma resposta mensurável, e este cartão contém seis. O texto extraído não lista os seis. Ele mostra uma das táticas do cartão: um cache de respostas indexado pela versão do cartão e pela versão da biblioteca.

### Modelo de interação (viewpoint de usabilidade)
Este cartão define exatamente o que o arquiteto vê e faz, passo a passo. O artigo dá a finalidade do viewpoint, mas não reproduz seus passos.

### Interface REST (viewpoint de integração)
Este cartão é o contrato entre o board service e o PAL, cobrindo cada chamada, cada erro e cada timeout. O artigo não informa endpoints, códigos de erro nem valores de timeout.

### Orçamento de latência (viewpoint de desempenho)
Este cartão define quanto tempo cada etapa pode levar. A sequência Cache Aside aparece aqui: o board service consulta o cache de respostas, vai ao PAL em caso de miss e depois armazena a resposta. O artigo não traz valores de orçamento.

### Modelo de implantação
Este cartão diz onde o PAL roda e quem pode acessá-lo. No artigo, a visão de implantação deixa em aberto onde fica o cache.

O ponto do autor é que um padrão não aparece em um único cartão. Perguntar qual artefato mostra qual padrão se aplica produz várias respostas, e projetar o PAL é uma tentativa de resolver esse problema.

## Um padrão relacionado a cache?

O autor nota o uso de cache no design do PAL e faz as perguntas que uma equipe faria: é Cache Aside, onde está, é Redis, e como mostrar isso visualmente e em código.

### Cache Aside
Cache Aside é o exemplo trabalhado. Você consulta primeiro o cache; se o item não estiver lá, vai à fonte e depois coloca a resposta no cache para a próxima vez. Em um quadro branco é simples, mas em um design real o padrão aparece em pedaços espalhados por um conjunto de visões.

### Correspondência parcial no board do PAL
Cache Aside aparece na sequência do orçamento de latência, na tática de cache de respostas do cartão de atributo de qualidade e em parte na visão de implantação, onde a localização do cache ainda está em aberto. A visão de contexto tem Redis, e sua lista de decisões ainda diz "Caching … undecided". O veredito do autor é "mais ou menos": trata-se de uma correspondência parcial, visível apenas ao olhar todos os diagramas juntos.

> ❌ **Anti-padrão — Forma do padrão presente sem decisão:** Cache Aside aparece em sequências e táticas, mas nenhum registro de decisão existe para ele. **Por que as pessoas fazem isso:** não declarado no artigo. **Consequência:** o artigo diz que ele não tem nenhuma decisão por trás, então a correspondência é parcial, na melhor das hipóteses. **Correção:** registrar uma decisão com seus trade-offs e depois registrar o padrão nos elementos que ele toca.

## Quando aplicar um padrão e quando não aplicar?

O autor retoma as duas funções: sugerir padrões para um design e verificar padrões já escolhidos contra o design.

### Definição em quatro partes de um padrão aplicado
A resposta do autor para "o que significa um padrão estar aplicado" tem quatro partes. Deve haver uma decisão (alguém a escolheu, por quê e quais são os trade-offs). O padrão deve estar registrado nos elementos de design reais aos quais se aplica. Deve haver uma verificação de que o design realmente corresponde ao padrão. A quarta parte, aplicá-lo, é a mais difícil. Remova qualquer uma delas e você volta às opiniões.

### Rascunho, depois aceite
No PAL, aplicar um padrão não muda nada por si só. O PAL redige um registro de decisão com o padrão, os elementos que ele toca e as forças e trade-offs extraídos da biblioteca. O arquiteto edita, argumenta e decide. Só o aceite do rascunho registra o padrão no design, e só então o PAL pode verificá-lo.

### Conforme, parcial, ausente, não avaliado
Depois que um padrão está registrado, o PAL informa se o design está conforme, é parcial (e diz o que falta) ou está completamente ausente. Se não consegue verificar algo, diz "não avaliado". A regra do autor é que o PAL nunca diz que você passou quando ninguém verificou.

> ❌ **Anti-padrão — Aprovação silenciosa:** Dar um padrão como atendido quando ninguém o verificou. **Por que as pessoas fazem isso:** não declarado no artigo. **Consequência:** uma afirmação não verificada passa como comprovada, o que, segundo o autor, mudaria metade das revisões de arquitetura de que ele já participou. **Correção:** informar itens não verificados como "não avaliado".

### Verificação de drift até o código
O trabalho de drift leva a mesma verificação para o código, de modo que um padrão pode ser seguido da biblioteca até a decisão, o design e o que foi construído. O autor diz que deseja essa cadeia há cerca de 20 anos.

## Onde entra a ciência

Um padrão não vive em um único diagrama. O exemplo de Cache Aside mostra o mesmo padrão dividido por um conjunto inteiro de visões, e é por isso que um único diagrama não pode decidir se ele está aplicado.

### Elementos de modelo estruturados
Cada modelo do Ergane é estruturado: todo elemento tem um ID, todo relacionamento tem um tipo e todo cartão conhece seu viewpoint. Isso permite que o PAL compare um padrão com os elementos e relacionamentos reais nos cartões e devolva conforme, parcial ou ausente, junto com o que encontrou. Cada resultado aponta para elementos específicos, para que o arquiteto possa olhar e argumentar.

### Descrições de padrões como imagens
Repositórios de fornecedores descrevem padrões como uma figura e um parágrafo. O autor diz que isso serve para aprender um padrão, mas é inútil para verificá-lo.

> ❌ **Anti-padrão — Padrão como figura e parágrafo:** Definir um padrão apenas por um esboço e prosa. **Por que as pessoas fazem isso:** não declarado no artigo; o autor diz que isso serve para aprender. **Consequência:** não há nada com que comparar um design, então nenhuma verificação é possível. **Correção:** descrever cada padrão por visão (sequência, implantação, contexto), com as decisões e os atributos de qualidade que ele toca.

### Questões em aberto na correspondência de padrões
O autor lista perguntas que ninguém respondeu: como julgar uma correspondência, quais partes de um padrão devem aparecer em qual visão, o que conta como parcial, como distinguir o Cache Aside real de um parecido, e se um padrão ausente na visão de implantação significa que o design ou o diagrama está errado.

### Fazendo crescer a biblioteca
Cada um dos 200 padrões precisaria dizer como ele aparece em uma sequência, em uma visão de implantação e em uma visão de contexto, e quais decisões e atributos de qualidade ele toca. O autor encerra pedindo um líder de padrões para ajudar a transformar a biblioteca em algo que a profissão possa provar que funciona.

## 🧠 Modelos mentais

### Teste em quatro partes para um padrão aplicado
**O modelo:** Um padrão só está aplicado quando existe uma decisão, o padrão está registrado nos elementos de design que ele toca, uma verificação compara o design com o padrão e a aplicação é levada até o fim. Remova qualquer parte e o que resta é opinião.
**Como aplicar:** Quando alguém disser "usamos Cache Aside", peça a decisão, os elementos que ele toca e o resultado da verificação.

### Correspondência entre visões
**O modelo:** Um padrão é reconhecido por suas partes em várias visões, como sequência, táticas, implantação e contexto, e não por um único diagrama. Uma correspondência é parcial quando algumas visões mostram o padrão e outras não.
**Como aplicar:** Liste as partes que cada viewpoint deveria mostrar e verifique cada visão. Uma lacuna em uma visão é uma lacuna de design ou uma lacuna de diagrama, e alguém precisa decidir qual.

### Status honesto: conforme, parcial, ausente, não avaliado
**O modelo:** Um resultado de verificação tem quatro estados, e "não avaliado" é uma resposta legítima, não uma aprovação.
**Como aplicar:** Informe qualquer item não verificado como "não avaliado", para que ninguém o leia como aprovado.

## 🔑 Principais conclusões
- Um padrão só conta como aplicado com uma decisão registrada, o padrão registrado em elementos de design específicos e uma verificação contra o design.
- O PAL está projetado, mas ainda não em execução, então o artigo descreve comportamento pretendido, e não resultados medidos.
- O Cache Aside no board do PAL aparece na sequência de latência, na tática de cache de respostas e em parte na implantação, mas não tem decisão por trás, o que o torna uma correspondência parcial.
- O PAL redige registros de decisão a partir da biblioteca e não muda nada até que o arquiteto aceite o rascunho.
- A verificação do PAL informa conforme, parcial (com o que falta), ausente ou não avaliado, e nunca aprovação quando ninguém verificou.
- As entradas da biblioteca de padrões precisam de descrições por visão e das decisões e atributos de qualidade que tocam, ou não podem ser verificadas.
- O autor relata que cerca de uma em cada 500 entrevistas ou sessões de mentoria mostrou alguém aplicando um padrão de forma repetível.

## 🏗️ Lente de arquitetura
| Aspecto | No artigo |
|---|---|
| Domínios do TOGAF | Aplicação (o PAL como serviço de domínio com uma interface REST para o board service) e Tecnologia (modelo de implantação, Redis, orçamento de latência). Dados são tocados apenas de leve pela chave do cache de respostas. Negócio não é coberto. |
| Escopo | Solução: o artigo trata de um serviço (PAL) projetado em um board do Ergane, embora a biblioteca de 200 padrões seja pensada como referência compartilhada. |
| Stakeholders e preocupações (concerns) | Arquitetos, que precisam ver onde os padrões estão e discutir os rascunhos; revisores de arquitetura, que precisam de evidências e não de afirmações; o board service, que precisa do contrato, dos timeouts e da latência de que depende; o papel de líder de padrões que o autor quer preencher; o autor, preocupado com repetibilidade e rigor. O requisito declarado é que o PAL não pode desacelerar o board. |
| Frameworks e notações | BTABoK (citado como fonte dos viewpoints, da biblioteca de padrões e do papel de líder de padrões). Nenhum TOGAF, ArchiMate, BPMN ou C4 é citado. REST e Redis são citados como tecnologias, não como notações. |

### Decisões e trade-offs
- **Rascunho, depois aceite pelo arquiteto:** As opções eram registrar um padrão no design automaticamente ou redigir uma decisão para o arquiteto editar e aceitar. O autor escolheu o rascunho. O artigo não informa custo; o custo implícito é que cada padrão aplicado precisa de revisão humana antes de ser registrado.
- **Verificação acima de recomendação:** O autor considera a verificação o trabalho que mais lhe importa, enquanto recomendar é a primeira função. O artigo não descreve um custo desse foco.
- **"Não avaliado" em vez de aprovação:** Itens não verificados são informados como não avaliados. O artigo não informa custo; o custo implícito são mais resultados que parecem incompletos.
- **Serviço de domínio privado, chamado apenas pelo board service:** O artigo dá essa posição e não discute alternativas nem custos.
- **Cache de respostas indexado pela versão do cartão e pela versão da biblioteca, com Redis:** A tática e a tecnologia são descritas, mas a decisão de cache ainda está registrada como "undecided" na visão de contexto, e a localização do cache está em aberto na visão de implantação.

### Fundamentos em que se apoia
- [Arquitetura](/pt-br/foundations/architecture.md) — "requisito arquiteturalmente significativo" é usado como requisito que muda a forma do sistema, o que se encaixa na definição de trabalho de propriedades e princípios que governam como um sistema é construído.
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md) — o PAL é descrito como um serviço de domínio com uma interação definida com o board service, o que se encaixa na estrutura e na interação das aplicações; o artigo não trata da estrutura de aplicações em toda a empresa.
- [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md) — o trabalho é restrito a um serviço projetado em um board, o que corresponde ao escopo de solução única, embora a biblioteca de padrões compartilhada amplie esse escopo.
- [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md) — o artigo nunca cita os quatro domínios do TOGAF e organiza seu design por viewpoints, então usa a ideia de modo diferente da definição de trabalho.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Na governança de arquitetura, o artigo sugere que uma decisão sobre padrão poderia ser revisada contra o registro de decisão, os elementos de design que ela toca e um resultado de verificação, em vez de ser afirmada em uma reunião. Para o seu MBA, ele se liga à tomada de decisão baseada em evidências. Quais das suas revisões de design atuais falhariam no teste do "mostre onde", e que evidências substituiriam isso?

## 📚 Plano de leitura
### Do artigo
- Patterns and Reference Models (apenas o texto do link; o texto extraído do artigo não traz URL, então nenhuma é escrita) — a biblioteca de 200 padrões do autor, a fonte da qual o PAL extrai.
### Para ir além
- Cache Aside — o exemplo trabalhado; leia como ele aparece em uma sequência e em uma implantação com cache.
- Registros de decisão de arquitetura — o registro de decisão do artigo é a unidade que o PAL redige e um arquiteto aceita.
- Viewpoints e biblioteca de padrões do BTABoK — o corpo de conhecimento em que o método do artigo se baseia.
- Cenários de atributos de qualidade — o cartão de seis cenários do artigo; leia como uma resposta mensurável é especificada.

## 🔗 Conceitos
- [adr](/pt-br/concepts/adr.md)
