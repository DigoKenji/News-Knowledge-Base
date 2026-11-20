---
type: Article Summary
title: 'Prompting é hoje a forma mais lenta e cheia de bugs de programar com IA. Aqui está a rápida e limpa'
description: 'Prompts não conseguem recuperar regras de negócio ocultas; programar com IA exige especificações explícitas e dependentes de contexto, além de verificação independente, para entregar software confiável.'
resource: https://medium.com/ai-advances/the-top-1-of-ai-coders-barely-prompt-they-do-this-instead-0015703c6064
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/ai-advances/the-top-1-of-ai-coders-barely-prompt-they-do-this-instead-0015703c6064
  title: Prompting Is Now the Slowest, Buggiest Way to Code with AI. Here's the Fast, Clean One
  author: AI Advances
status: stable
source_url: https://medium.com/ai-advances/the-top-1-of-ai-coders-barely-prompt-they-do-this-instead-0015703c6064
source: Medium (AI Advances)
source_type: medium
published: '2026-10-30'
domain: software-engineering
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81ae803ec99e26c14f1a
lang: pt-br
translation_of: /notes/software-engineering/2026-11-20-prompting-is-now-the-slowest-buggiest-way-to-code-with-ai-he.md
---

## 🎯 Tese central
Prompts sozinhos não conseguem tornar correto o software escrito por IA, porque a implementação certa depende de contexto de negócio que o prompt não carrega. Por isso esse contexto precisa ser explícito e verificado de forma independente.

## 🧭 Por que o autor escreveu
O artigo parte da fantasia corporativa do "desenvolvedor 10x" e da promessa de que a IA permitirá que um engenheiro substitua uma equipe inteira. O autor argumenta que as evidências mostram que a IA acelera a geração de código sem produzir de forma confiável software correto e valioso. Se ele estiver certo, a solução é disciplina de engenharia (requisitos explícitos mais verificação independente), não prompts mais inteligentes.

## Introdução: o desenvolvedor 10x mítico
> 🔎 Contexto adicionado: o texto capturado não tem título antes desta abertura, então o título acima é um rótulo descritivo que eu dei, não do autor.

### Produtividade como argumento de headcount
O autor descreve a pressão por corte de custos, em que a gestão pede mais produção com menos desenvolvedores. Ele aponta para o "desenvolvedor 10x mítico" como o número ao qual os executivos recorrem. Também observa que *The Mythical Man-Month*, de Frederick P. Brooks Jr., costuma ser reduzido à afirmação de que adicionar desenvolvedores só atrasa projetos, o que ele considera uma distorção do livro.

### Custo de coordenação
Adicionar desenvolvedores ajuda quando as tarefas são independentes, porque a produção cresce. Quando eles trabalham em código fortemente acoplado, os custos de coordenação podem consumir o ganho. Poucos desenvolvedores e o projeto avança devagar enquanto os custos se acumulam. A conclusão do autor é que não existe tamanho mágico de equipe, e uma escolha errada desperdiça meses antes que alguém perceba. Ele cita pesquisas da Microsoft mostrando que a estrutura organizacional e a coordenação afetam a qualidade do software, mas o texto capturado não traz números.

## Esta bagunça sangrenta fica ainda pior com a IA?
### A visão de um arquiteto, muitos agentes
O autor descreve o sonho executivo: um desenvolvedor dirigindo uma frota de agentes de programação com IA sob uma única arquitetura, com a produção de uma equipe inteira e nenhum custo de coordenação. Ele aceita que essa é a promessa e então argumenta que a realidade não corresponde a ela.

### A IA herda o problema de coordenação
A tese dele é que agentes de IA criam a mesma bagunça de coordenação que desenvolvedores humanos, só que mais rápido. Ele cita a pesquisa DORA do Google como mostrando que a IA amplifica tanto os pontos fortes quanto os fracos das práticas de engenharia existentes. Seu resumo é direto: dê à IA um processo de engenharia quebrado e você terá o mesmo desastre em velocidade de máquina.

## O desastre de produtividade com IA: só 2% relatam ganhos de 10x
### Produtividade autorrelatada
O artigo diz que apenas cerca de 2% dos desenvolvedores relataram ganhos de 10x ou mais, o que significa que cerca de 98% não relataram chegar a esse nível. O autor ressalta que mesmo esses 2% são autorrelatados e não medidos como aumento de software entregue. Gerar muito código e entregar software valioso são coisas diferentes, e os números autorrelatados medem a primeira.

> ❌ **Anti-padrão — Julgar produtividade pela autoavaliação:** tomar as estimativas dos desenvolvedores sobre a própria velocidade como evidência de valor entregue. **Por que as pessoas fazem isso:** é barato de coletar e lisonjeiro de relatar. **Consequência:** os ganhos parecem grandes enquanto o software que chega aos usuários não muda. **Correção:** medir a produção entregue e funcionando (releases, funcionalidades aceitas, defeitos), e não a atividade.

### O paradoxo de produtividade: commits versus releases
O gráfico do artigo mostra 240% mais commits, mas apenas 30% mais releases. A produção de código cresce muito mais rápido do que a entrega. O autor chama isso de paradoxo de produtividade da IA: montanhas de código sem ganhos correspondentes no software entregue. Colocar software em produção também não garante que ele tenha valor, porque quem decide é o mercado.

### Software gerado e o mercado
Um segundo gráfico mostra sete vezes mais apps chegando ao mercado, enquanto o app típico por assinatura fatura US$ 72 por mês depois do primeiro ano. O autor é cuidadoso aqui: esses números não provam que a IA causa fracasso comercial. Eles mostram que tornar o software barato de gerar não o torna confiável, útil ou lucrativo.

### O assistente que lê mentes
O autor diz que muitos desenvolvedores imaginam um "homúnculo digital" dentro da máquina, que lê suas mentes e entende o que eles quiseram dizer. Ele pede que o leitor abandone essa imagem. A IA não consegue recuperar de forma confiável regras de negócio ou requisitos ocultos que nunca lhe foram dados, e a frase dele para isso é: você não consegue escapar de uma especificação quebrada com prompts.

> ❌ **Anti-padrão — O assistente que lê mentes:** presumir que a IA infere o que você quis dizer a partir de um pedido curto. **Por que as pessoas fazem isso:** código fluente e plausível parece compreensão. **Consequência:** o código satisfaz o prompt, mas não as regras que ninguém escreveu. **Correção:** escrever requisitos, restrições e invariantes onde o agente e as verificações possam lê-los.

## A ciência por trás do 1% do topo: pare de fazer prompts, comece a engenharia
### Engenharia versus estimativa
O autor afirma que o 1% do topo dos desenvolvedores de IA não estima sua produtividade; eles a projetam. O artigo apresenta isso como contraste e não traz nenhuma medição desse grupo. Ganhos reais, na visão dele, vêm de entender o que a IA não pode saber, o que ela só pode adivinhar e o que você pode forçá-la a acertar.

### Prompts mágicos e frameworks de prompting
Ele deixa de lado os "prompts mágicos", os frameworks de prompting que prometem tudo e os tutoriais que prometem resultados 10x da noite para o dia.

> ❌ **Anti-padrão — Prompting como estratégia:** tratar templates e frameworks de prompting como a principal rota para software profissional. **Por que as pessoas fazem isso:** prompts são rápidos de testar e fáceis de compartilhar. **Consequência:** a saída varia com um contexto que nunca foi declarado, e ninguém consegue checá-la contra os requisitos. **Correção:** usar o prompt como uma etapa preliminar dentro de um processo de engenharia especificado e verificado.

## A armadilha do contexto oculto: sua IA não sabe o que você esqueceu de contar
### O exemplo do reembolso
O exemplo didático do autor é uma função de reembolso para um sistema de pagamentos. A IA devolve código sensato, com tratamento de erros e alguns testes, e tudo passa. Depois, um cliente recebe o reembolso duas vezes. O assistente nunca foi informado de que o mesmo pagamento não pode ser reembolsado duas vezes, e nada foi dito sobre reembolsos parciais, requisições concorrentes ou uma queda de rede no meio de uma operação financeira. Ele chama isso de contexto semântico ausente: as regras de negócio e as dependências que decidem se o código está correto.

### Modelos de tokens e semântica oculta
O autor aceita que os modelos generativos atuais, baseados em tokens, conseguem raciocinar sobre contexto. Seu ponto é que eles não conseguem reconstruir nem verificar de forma confiável a semântica oculta da qual o software profissional depende. A implementação que você precisa depende de mais do que o prompt.

```javascript
y = F(x, z)
```

**O que isso faz:**
- `x` é o prompt, o pedido em linguagem natural dado à IA.
- `z` é o contexto relevante: regras de negócio, invariantes e dependências que o prompt pode não carregar.
- `y` é a implementação de que o negócio realmente precisa, então a correção é definida pelo par (x, z), e não por x sozinho.
- A notação é o modelo do autor, não código executável. A marcação `javascript` corresponde ao bloco de código original.

### Mesmo prompt, respostas corretas diferentes
Dois sistemas de pagamento recebem o mesmo prompt de reembolso. Um permite múltiplos reembolsos parciais e o outro permite um único reembolso por transação. Ambos precisam de implementações diferentes.

```javascript
F(x, z₁) ≠ F(x, z₂)
```

**O que isso faz:**
- `x` é o mesmo prompt nos dois casos, enquanto `z₁` e `z₂` são dois contextos de negócio diferentes.
- A desigualdade diz que as duas implementações corretas diferem, embora o pedido seja idêntico.
- Uma IA que recebe apenas `x` não tem base para escolher a certa.
- O autor usa isso para justificar tornar `z` explícito e verificável.

### Contextos semânticos aninhados
O autor estende o ponto para software comercial que envolve prêmios de seguro, transferências bancárias, contratos com clientes e estoque de armazém. Ali, as regras de negócio dependem de regulação, do estado do banco de dados e de exceções enterradas em várias camadas. Uma regra pode mudar o sentido de outra. Algumas regras estão no código ou em documentos, e outras vivem na cabeça das pessoas que entendem o negócio. Até que essas regras sejam extraídas e escritas como requisitos explícitos, agentes de IA repetem os mesmos erros.

## O erro de IA mais caro: deixar a IA julgar seus requisitos de negócio
### O pipeline que certifica a si mesmo
O autor descreve o modo de falha: a IA escreve o código, gera os testes, revisa o próprio trabalho e informa que tudo passa. Se o desenvolvedor entendeu mal ou esqueceu um requisito, todos os testes podem ficar verdes enquanto o software está errado. O autor observa que isso é um exemplo simples, mas os mesmos princípios valem para sistemas reais.

> ❌ **Anti-padrão — Correção autocertificada:** deixar a mesma IA escrever o código, os testes e a revisão, e então aceitar o resultado verde como prova. **Por que as pessoas fazem isso:** testes verdes parecem conclusão, e o ciclo é rápido. **Consequência:** um sistema consistente, mas errado, em que todos os testes passam exceto aquele que importava. **Correção:** manter o dono do requisito separado do gerador e verificar a saída de forma independente contra regras escritas e revisadas.

### Quem decide o que é correto
A regra do autor é: deixe a IA escrever o código, mas garanta que alguém que não seja a máquina decida o que significa software correto. Regras que existem apenas no código, em documentos ou na memória das pessoas precisam ser extraídas e transformadas em requisitos explícitos antes que agentes possam ser confiados a elas.

## IA transformativa versus IA transportiva: pare de adivinhar o que você pode preservar
O autor diz ter desenvolvido a distinção em trabalhos anteriores, depois de lutar com a programação com IA em projetos reais. A IA transformativa produz código a partir de padrões aprendidos e do contexto que consegue alcançar, muitas vezes adivinhando relações fora desse alcance. A IA transportiva carrega o significado de forma explícita por todo o processo de desenvolvimento. Ele diz que isso vai além de prompts mais inteligentes.

### Mesmo token, significado diferente
O significado de uma variável depende de onde ela aparece, de qual vinculação ela referencia e das regras da linguagem. O mesmo identificador pode significar coisas diferentes dentro de uma closure, de uma função, de uma classe ou de um módulo. Compiladores resolvem essas relações com precisão, enquanto assistentes de IA com contexto incompleto podem ter que adivinhar o que o compilador já sabe. Daí vem o primeiro princípio da IA transportiva: carregar a informação semântica que o sistema de programação já possui, em vez de reconstruí-la a partir de tokens.

> 🔎 Contexto adicionado: uma vinculação (*binding*) é a declaração à qual um nome se refere em um dado ponto do código. Ferramentas de linguagem usam regras de escopo para decidir a qual vinculação cada uso se refere.

### Mesmo conceito, geometria de negócio diferente
O autor passa da semântica de linguagens de programação para regras de negócio e usa uma analogia comparando dois sistemas jurídicos. A mesma infração pode levar a resultados parecidos em um sistema e muito diferentes em outro, porque camadas de direito e de religião mudam o resultado. O software funciona da mesma forma: um contrato dá reembolso integral e outro dá apenas parte dele. Ambas as implementações são válidas, e nenhuma pode ser trocada pela outra.

O autor nomeia dois casos. Em concordância contextual, dois contextos dão o mesmo resultado para uma operação. Em divergência contextual, eles dão resultados diferentes. Ele escreve I(o,c) para o que uma operação o significa sob um contexto C, de modo que dois contextos podem concordar sobre uma regra e discordar sobre outra. As fórmulas aparecem como imagens que o texto-fonte não inclui, então esta nota usa apenas os termos que o texto fornece. O ponto do autor é que a geometria ajuda a visualizar essas diferenças, mas o trabalho de engenharia é transformá-las em regras explícitas que o software possa verificar.

## Pare de enterrar regras de negócio em prompts. Dê a elas uma estrutura.
A questão prática é como lidar com contextos semânticos aninhados ao construir software comercial com IA. Explicar as mesmas regras a cada agente que toca o código, as APIs ou o banco de dados é caro e sujeito a erros. O autor propõe uma especificação semiformal que ferramentas de engenharia possam entender e verificar.

### Especificação semiformal
O autor mapeia os conceitos geométricos apresentados antes para contrapartes de software, em uma tabela que o texto capturado não inclui. O objetivo é uma estrutura com a qual ferramentas de programação com IA possam trabalhar, com as regras de negócio, os contextos em que se aplicam e as restrições que precisam valer em todos os lugares.

## Como seu YAML representa a geometria
### A especificação de reembolso em YAML
O autor apresenta uma especificação YAML simplificada para a operação de reembolso. A figura não está no texto capturado. O exemplo a usa para mostrar que um contrato permite reembolso integral, outro permite apenas reembolso proporcional, e nenhum dos dois permite execução duplicada. Contextos diferentes produzem comportamentos diferentes, e algumas regras valem nos dois.

### Um arquivo não é imposição
Escrever as regras em YAML não faz o software obedecê-las. Alguém precisa definir com precisão o que cada regra significa, e um motor de contexto precisa rejeitar requisitos ausentes ou conflitantes, em vez de deixar a IA adivinhar. Verificações independentes ainda são necessárias para pegar uma implementação que quebre uma regra.

> ❌ **Anti-padrão — Declarar a regra em um arquivo:** escrever `duplicate_execution: Forbidden` e presumir que o software agora respeita a regra. **Por que as pessoas fazem isso:** um arquivo declarativo parece um controle. **Consequência:** nada impede uma implementação de quebrar a regra, e o reembolso continua acontecendo duas vezes. **Correção:** fazer um motor de contexto rejeitar requisitos ausentes ou conflitantes, e rodar verificações independentes contra a implementação.

### O motor de contexto
O trabalho do motor de contexto é descobrir quais regras se aplicam em um dado contexto, como elas interagem e se algo está faltando ou se contradiz. Sua saída alimenta a etapa de geração de código e dá às verificações um alvo preciso.

## Da geometria semântica a uma máquina de programação com IA que funciona
### A máquina
O pipeline completo do autor tem quatro etapas. Você escreve uma especificação semiformal das regras de negócio e de seus contextos. O motor de contexto decide quais regras importam e sinaliza lacunas ou contradições. Agentes de IA geram o código. A verificação independente checa se o resultado satisfaz os requisitos. O diagrama não está no texto capturado, mas sua legenda descreve a mesma sequência.

### O que significa uma verificação aprovada
O autor alerta contra ler a máquina como uma garantia. Se o código passa, ele cumpriu as verificações para as regras de negócio que foram especificadas, e nada além disso. O sistema também registra quais regras foram usadas, qual contexto se aplicou e quais verificações passaram. Quando os requisitos mudam, esse registro mostra o que deve ser revisto de novo.

### Transformações entre contextos
Cada contexto C tem sua própria especificação semântica Sc, e mover-se entre contextos exige uma transformação. Quando vários contextos estão envolvidos, as transformações precisam seguir regras de composição definidas. Algumas regras sobrevivem à mudança, outras mudam, e contextos incompatíveis podem tornar a transformação impossível. Essas fórmulas também são imagens que o texto capturado não inclui. A conclusão do autor é que o engenheiro passa de descrever o significado contextual para computar com ele.

## 🧠 Modelos mentais
### Custo de coordenação segue o acoplamento
**O modelo:** adicionar pessoas ou agentes ajuda quando o trabalho é independente e atrapalha quando ele é fortemente acoplado, então o custo de coordenação cresce com o acoplamento, e não com o número de pessoas.
**Como aplicar:** antes de adicionar agentes a uma base de código, verifique quantos módulos compartilham estado e interfaces. Divida o trabalho em costuras que não alterem umas às outras.

### O prompt não é a especificação
**O modelo:** a implementação correta é uma função do prompt e do contexto, y = F(x, z), e o mesmo prompt sob contextos diferentes tem respostas corretas diferentes.
**Como aplicar:** quando um pedido for ambíguo, escreva o contexto do qual ele depende, como limites, invariantes e contratos, antes de pedir código.

### Concordância e divergência contextuais
**O modelo:** o significado de uma operação depende do contexto em que ela é avaliada, então dois contextos podem concordar sobre uma regra e discordar sobre outra.
**Como aplicar:** compare as regras entre os contratos ou mercados que você atende, marque cada regra como compartilhada ou específica de contexto antes de construir uma única implementação.

### Não deixe o gerador se avaliar
**O modelo:** a entidade que escreve o código não pode ser o único juiz de se o código atende ao requisito.
**Como aplicar:** escreva requisitos e critérios de aceitação em uma forma que um humano tenha revisado, e rode-os contra uma saída que o gerador não escreveu.

### Transportiva, não transformativa
**O modelo:** carregue pelo processo o significado que ferramentas e especificações já mantêm, em vez de pedir ao modelo que o reconstrua a partir de tokens a cada vez.
**Como aplicar:** forneça aos agentes os resultados de resolução do compilador, a especificação de regras e os identificadores de contexto, e verifique as saídas contra eles.

## 🔑 Principais conclusões
- Apenas cerca de 2% dos desenvolvedores relataram ganhos de produtividade de 10x, e o autor observa que até esse número é autorrelatado, e não medido como software entregue.
- O gráfico do artigo mostra 240% mais commits, mas apenas 30% mais releases, o que o autor chama de paradoxo de produtividade da IA.
- O app típico por assinatura fatura US$ 72 por mês depois do primeiro ano, enquanto o mercado tem sete vezes mais apps. O autor diz que isso não prova que a IA causa fracasso comercial.
- O mesmo prompt de reembolso tem implementações corretas diferentes sob contextos diferentes, expresso como F(x, z₁) ≠ F(x, z₂), então o contexto de negócio precisa fazer parte da entrada.
- Uma IA que escreve o código, os testes e a revisão pode certificar um sistema errado, então os requisitos precisam ser de propriedade e verificados por alguém que não seja o gerador.
- As regras de negócio devem viver em uma especificação semiformal que um motor de contexto valida quanto a lacunas e conflitos, e que verificações independentes testam contra a implementação.
- Uma verificação aprovada significa apenas que as regras especificadas foram cumpridas; o sistema deve registrar quais regras, contextos e verificações se aplicaram, para que as mudanças possam ser rastreadas.

## 🏛️ Para Kenji
> Esta é a sua interpretação, não a do autor.

Para um arquiteto, o artigo transforma regras de negócio em um artefato de primeira classe, que agentes de IA precisam consumir e que a verificação precisa testar, o que se encaixa na governança de arquitetura. Regras semelhantes a reembolsos, que variam por contrato ou jurisdição, seriam especificadas por contexto, com o registro de qual contexto se aplicou. Uma questão que ele levanta: em seus sistemas, quais regras de negócio existem hoje apenas na cabeça das pessoas ou no código legado, e o que seria preciso para escrevê-las em um lugar onde um agente e uma verificação possam lê-las?

## 📚 Plano de leitura
### Do artigo
- Nenhum. O texto capturado não contém links. Ele cita *The Mythical Man-Month* de Brooks (1975), a pesquisa da Microsoft sobre estrutura organizacional e qualidade de software, e a pesquisa DORA do Google, sem URLs.

### Para aprofundar
- Frederick P. Brooks Jr., *The Mythical Man-Month* — a fonte do argumento de coordenação e do modelo de equipe cirúrgica que o autor revisita.
- Relatórios de pesquisa DORA do Google — a base do autor para dizer que a IA amplifica os pontos fortes e fracos de engenharia já existentes.
- Pesquisa da Microsoft sobre estrutura organizacional e qualidade de software — o artigo a cita para efeitos de coordenação, mas não dá título.
