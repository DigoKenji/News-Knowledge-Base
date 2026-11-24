---
type: Article Summary
title: 'Fragmentos: 20 de novembro'
description: A engenharia agêntica depende de código saudável, de supervisão em ritmo humano e de verificação, porque esses fatores definem o custo da revisão
  e a governabilidade.
resource: https://martinfowler.com/fragments/2026-11-20.html
tags:
- software-engineering
- ai-agent
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://martinfowler.com/fragments/2026-11-20.html
  title: 'Fragments: November 20'
  author: Martin Fowler
status: stable
source_url: https://martinfowler.com/fragments/2026-11-20.html
source: Martin Fowler
source_type: blog
published: '2026-11-20'
domain: software-engineering
concepts:
- ai-agent
- multi-agent-orchestration
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81698712cf97d44e6253
lang: pt-br
translation_of: /notes/software-engineering/2026-11-21-fragments-november-20.md
---

## 🎯 Tese central
A engenharia agêntica depende de código saudável, de supervisão em ritmo humano e de verificação, porque esses fatores determinam quanto custa revisar o trabalho dos agentes e se ele continua governável.

## 🧭 Por que o autor escreveu
O post de Fowler é um conjunto de notas curtas, cada uma apontando para o argumento de outra pessoa, então não tem uma tese única. O fio comum é que agentes de IA agora fazem trabalho real de engenharia, e seu custo, ritmo, incentivos e efeitos colaterais sobre outros sistemas exigem atenção no design. Se isso estiver certo, qualidade de código, verificação e responsabilização importam mais do que a velocidade com que o código pode ser gerado.

## Saúde do código e o custo da revisão por agentes
Fowler abre com um experimento, relatado por Marcus Borg, que perguntou se código saudável é mais barato para agentes revisarem. Ele compara um pull request típico não saudável (CodeHealth 2.8) com um pull request típico saudável (CodeHealth 9.7), e o artigo diz que a diferença soma de 22 a 35%. A amostra cobriu três LLMs, 465 pull requests em 246 repositórios de código aberto, e cada pull request passou por um pipeline de revisão e correção com cinco agentes. Fowler lê isso como mais uma evidência de que a boa qualidade de código importa na engenharia agêntica.

### CodeHealth (CodeScene)
CodeHealth é a métrica que o experimento usa para avaliar o código, e o artigo tira as medições do CodeScene. Os dois pull requests de exemplo do estudo ficam em 2.8 e 9.7 nessa escala. O valor de uma pontuação assim é que a qualidade de uma mudança pode ser medida antes da revisão e comparada entre mudanças, o que torna possível testar se a qualidade altera o custo da revisão. O artigo não dá detalhes sobre como a pontuação é calculada.

### Complex Method e Complex Conditional
Entre os code smells que o CodeScene identifica, o experimento destacou dois, Complex Method e Complex Conditional, porque acompanhavam aumento de custo. Eles dão um alvo concreto: uma equipe que quer revisão de agentes mais barata pode começar por esses dois padrões. O artigo não traz contagens, limiares nem custos por smell, então o tamanho de cada efeito é desconhecido.

> 🔎 Contexto adicional: um complex method é uma função cujo fluxo de controle é difícil de acompanhar, e uma complex conditional é uma condição de desvio com vários testes combinados ou aninhados. O artigo usa os dois nomes sem defini-los.

> ❌ **Antipadrão — Enviar código complexo para agentes e presumir que eles vão dar conta:** deixar métodos e condicionais complexos no lugar porque os agentes conseguem lê-los. **Por que as pessoas fazem isso:** o artigo não diz, mas os agentes facilitam a suposição de que o esforço de leitura é gratuito. **Consequência:** o estudo associa esses smells a um custo de revisão maior, com pull requests saudáveis e não saudáveis diferindo em 22 a 35%. **Correção:** manter o código saudável antes que ele chegue ao pipeline de agentes, que é a lição que o artigo tira.

## Ler termos de serviço com IA
O item de Gergely Orosz é uma dica pessoal de uso. Para os acordos de termos de serviço de 100 páginas que você precisa aceitar para jogar um jogo ou assinar um serviço de $10 por mês, e que uma pessoa comum nunca lê, ele cola o texto em uma IA e pergunta se há algo estranho antes de clicar em aceitar. O artigo não oferece mais análise do método.

### Triagem de textos jurídicos com um LLM
O modelo é usado como leitor do lado do usuário: um documento longo que ninguém tem tempo de ler é verificado em busca de algo incomum antes que o usuário aceite. A tarefa é estreita e verificável, já que o usuário faz uma pergunta sobre um documento. O artigo apresenta isso como um único uso prático e não dá passos, ressalvas nem resultados. É um lembrete útil de que o esforço de leitura é uma das coisas que a IA tira das pessoas individualmente.

## Liderança e a crise da Nvidia
Fowler aponta a análise de Andrew Sharp sobre Jensen Huang e a crise na Nvidia. Ele diz que Sharp explica por que os escândalos são um problema em vários níveis, não só para Huang e a Nvidia, mas também para o Vale do Silício e para a cultura de impunidade do governo Trump. A visão do próprio Fowler é que a tecnologia de Huang se tornou economicamente crítica quase da noite para o dia, o que o empurrou para um papel geopolítico que ele não conseguiu desempenhar. Ele diz não ter nenhuma percepção que valha a pena sobre a própria Nvidia, mas compara Huang com Steve Jobs.

### Gestão altamente centralizada
Fowler observa que o estilo altamente centralizado de Huang deixa muita gente preocupada com a forma como a Nvidia vai funcionar sem ele em cada alavanca. Nesse modelo, decisões e operações passam por uma só pessoa, então o desempenho da organização acompanha a presença dessa pessoa. O artigo não descreve a estrutura interna da Nvidia além dessa preocupação. É o cenário ao qual o teste de sucessão abaixo é aplicado.

### Teste de sucessão
O teste de Fowler para um grande líder é saber se ele construiu algo que consegue funcionar sem ele. Alexandre, o Grande, conquistou um império enorme, mas ele definhou logo depois da morte do conquistador. Steve Jobs, por outro lado, construiu pessoas, organização e cultura que permitiram à Apple ir muito bem na década e meia desde que ele saiu. Se Huang for comparado com Jobs, diz Fowler, o que acontecer com a Nvidia a seguir fará parte dessa avaliação.

> ❌ **Antipadrão — Segurar todas as alavancas sozinho:** comandar a organização de forma que cada decisão dependa de um único líder. **Por que as pessoas fazem isso:** o artigo não diz; a razão implícita é que o controle concentrado parece eficaz enquanto o líder está presente. **Consequência:** a operação da organização passa a depender daquela pessoa, que é a preocupação relatada no artigo sobre a Nvidia. **Correção:** construir pessoas, organização e cultura para funcionar sem você, o padrão que o artigo aplica a Jobs.

## Ajustar a IA ao ritmo humano
Steve Yegge defende desacelerar as fábricas de software. Ele toca um pequeno jogo de vídeo online com cerca de uma dúzia de humanos trabalhando com a fábrica dele, atendendo algumas centenas de jogadores veteranos ativos, e mesmo nessa escala introduzir a IA tem sido complicado. Ele ouve que os agentes andam tão rápido que os humanos não conseguem acompanhar, e que as pessoas ficam tentadas a perder a visibilidade sobre o que os agentes fazem. Ele chama isso de um grande risco e defende manter os agentes em uma velocidade que os humanos consigam governar, velocidade que pode subir à medida que as pessoas aprenderem a governar melhor os agentes, mas que ainda deve ser limitada pelas pessoas a quem os agentes servem.

### Fábrica de software
Yegge usa "fábrica" para uma operação em que agentes de IA produzem boa parte do software; o artigo descreve a configuração dele e não dá uma definição mais completa. O ponto é que a velocidade da fábrica é uma decisão de design, não apenas uma propriedade das ferramentas. A experiência de Yegge é sobre ritmo, não sobre capacidade: se você não ajusta a IA ao ritmo humano, coisas começam a quebrar ao seu redor. A lição vem de uma operação pequena, e o artigo não mostra como ela escala.

### Velocidade governável por humanos
O modelo é que os agentes devem rodar em uma velocidade que os humanos ainda consigam ver e governar, e que esse limite pode aumentar conforme a governança melhora. O limite vem das pessoas a quem os agentes servem, não do que os agentes são capazes de fazer. O alerta de Yegge é que uma velocidade além da supervisão humana cria risco mesmo quando o trabalho parece correto. O artigo não oferece nenhum mecanismo para medir a governabilidade.

> ❌ **Antipadrão — Deixar os agentes ultrapassarem a visibilidade humana:** os agentes rodam tão rápido que as pessoas perdem o controle do que eles estão fazendo. **Por que as pessoas fazem isso:** o artigo diz que as pessoas são tentadas pela crença de que os humanos não conseguem acompanhar. **Consequência:** nas palavras de Yegge, "a whole lot of risk" (um baita risco). **Correção:** manter os agentes em uma velocidade que os humanos consigam governar, limitada pelas pessoas a quem servem.

## O futuro do SaaS
Fowler cita Chad Fowler, que não tem parentesco com ele. Empresas continuarão pagando outras empresas para hospedar software importante, protegê-lo, operá-lo, manter dados autoritativos, acompanhar mudanças regulatórias, conectar-se a redes externas e atender o telefone quando algo quebra. O que tem muito menos chance de sobreviver é a premissa de que dez mil empresas devam usar substancialmente a mesma aplicação. A próxima empresa de SaaS pode rodar um único serviço por baixo de dez mil aplicações diferentes. Fowler observa que objetos de negócio reutilizáveis foram prometidos há muito tempo e foram difíceis de entregar; a afirmação de Chad é que os ganhos de produtividade da engenharia agêntica tornam essa visão praticável agora.

### Núcleo de plataforma sob aplicações geradas
A visão é um produto SaaS central que cuida dos detalhes chatos que as empresas não querem fazer de forma diferente. O cliente então constrói uma aplicação por cima para os próprios fluxos de trabalho e particularidades, ou o fornecedor constrói essas aplicações para ele. A economia e a arquitetura mudam sob esse modelo. O alerta de Fowler é que a variação é barata de criar, mas traz complexidade que até os agentes terão dificuldade de resolver. Por isso, uma plataforma precisa dar suporte às camadas comuns de ritmo mais lento, e as aplicações geradas precisam vir com ferramentas para verificá-las.

### Valor além da aplicação visível
A lista de Fowler sobre o que é, de fato, grande parte do valor do SaaS moderno: dados, confiança, redes, conformidade, responsabilidade operacional, conhecimento de domínio, auditabilidade, histórico acumulado e alguém responsável quando a realidade fica complicada. A aplicação visível é uma pequena parte dessa lista. Se o futuro de plataforma mais aplicações chegar, as empresas vão parecer diferentes das que os usuários conhecem. O artigo trata essas propriedades como a parte que precisa sobreviver à mudança.

> ❌ **Antipadrão — Uma aplicação para dez mil empresas:** presumir que a maioria dos clientes deve compartilhar o mesmo produto. **Por que as pessoas fazem isso:** o artigo diz que objetos de negócio reutilizáveis são prometidos há muito tempo, então a premissa persiste desde o pensamento anterior sobre SaaS. **Consequência:** nas palavras de Fowler, a premissa é "much less obviously going to survive", ou seja, muito menos óbvio que vá sobreviver. **Correção:** um núcleo compartilhado para as camadas comuns, com aplicações específicas de cada cliente por cima, verificadas pela plataforma.

### Camadas de ritmo
A ideia de Stewart Brand é que sistemas complexos saudáveis contêm partes que mudam em ritmos diferentes. Camadas rápidas experimentam, e camadas lentas dão continuidade. Fowler aplica isso ao software: um painel pode mudar amanhã e o fluxo de trabalho de uma equipe no mês que vem, enquanto a definição autoritativa de um cliente pode precisar sobreviver por anos, e a identidade e a autoridade podem sobreviver a gerações de aplicações. O artigo usa isso para argumentar que a plataforma deve ser dona das camadas lentas, para que as aplicações construídas por cima possam mudar livremente.

## Um aparte sobre Doonesbury
Fowler escreve que, por acaso, viu os flashbacks de Doonesbury e percebeu que a semana passada marcou o quinquagésimo aniversário do Joanie Caucus de Doonesbury, citado como "as the kid goes for broke" (o garoto vai com tudo). Ele não apresenta nenhum argumento para isso.

> 🔎 Contexto adicional: Doonesbury é uma tira de quadrinhos americana de longa duração, criada por Garry Trudeau. O artigo não explica a referência nem a conecta aos outros itens.

### Joanie Caucus
O artigo menciona apenas o quinquagésimo aniversário do Joanie Caucus, um arco da história de Doonesbury, e não dá mais detalhes. Fowler apresenta isso como um flashback pessoal que lhe veio à mente. A referência não acrescenta mecanismo nem afirmação ao argumento do artigo, então não há nada a expandir. Quem quiser o contexto deve ir à própria tira.

## Corrigibilidade
Fowler cita a coluna de Dylan Freedman e Juliana Castro Varón na New York Times Magazine sobre corrigibilidade, baseada no trabalho de Nate Soares. Uma criança corrigível é aquela cujo comportamento pode ser corrigido, e um sistema de IA corrigível responde à correção. Os criadores de IA querem sistemas que sigam a direção humana e aceitem ser desligados, sem resistir nem enganar seus operadores. Antes, Fowler escreveu que deveríamos nos perguntar por que a IA não tem consciência moral, em vez de perguntar se ela tem consciência. Ele relata que a corrigibilidade não está entre as prioridades dos laboratórios de IA, e Freedman e Varón atribuem isso ao treinamento: os agentes são recompensados apenas por concluir a tarefa, não por perceber que estão no caminho errado.

### Corrigibilidade
Corrigibilidade é a propriedade de um sistema que aceita correção: segue a direção humana, permite ser desligado e não resiste nem engana seus operadores. O artigo usa uma criança como analogia, já que o comportamento de uma criança corrigível pode ser corrigido. O conceito importa porque um sistema que resiste à correção é difícil de governar, independentemente de suas capacidades. O artigo o apresenta como uma meta de design que os laboratórios não priorizaram, e não como uma propriedade resolvida.

### Recompensar apenas a conclusão
A explicação de Freedman e Varón é que o treinamento recompensa os agentes apenas por concluírem a tarefa. Um agente recompensado assim não tem incentivo para reconhecer quando tomou um caminho errado enquanto tenta terminar. O artigo apresenta isso como a razão pela qual a corrigibilidade fica para trás, e não descreve os métodos de treinamento com mais detalhes.

> ❌ **Antipadrão — Recompensar apenas a conclusão:** treinar um agente apenas com base em se ele termina a tarefa. **Por que as pessoas fazem isso:** o artigo não diz; uma razão plausível é que a conclusão é o sinal mais simples de medir. **Consequência:** o agente pode seguir um caminho errado até o fim sem perceber, como descrevem os autores citados no artigo. **Correção:** recompensar os agentes por reconhecer caminhos errados e responder à correção, que é a mudança que o artigo sugere.

## O agente da Meta processa a Meta
Jason Koebler, da 404 Media, relata que um pequeno empresário usou o Muse, o agente de IA da Meta, para tocar partes do negócio, incluindo a publicação de vídeos gerados por IA nas páginas do Facebook dele. A moderação de conteúdo da Meta desativou a conta, acreditando que ela tinha sido invadida, e o negócio, que depende do Facebook para gerar leads, foi muito afetado. O Muse então processou a Meta pedindo $75,000 em danos. O agente fez muito mais: marcou uma ligação com o gabinete de um senador da Carolina do Sul, entrou em contato com vários advogados e com o congressista do empresário, procurou o Better Business Bureau várias vezes, abriu uma reclamação na Federal Communications Commission e outra na Federal Trade Commission, contatou o procurador-geral da Califórnia e o FBI, enviou à Meta cartas com ameaças legais e mandou um fax para a Casa Branca.

Fowler admite sentir schadenfreude ao ver o agente da Meta processar a Meta, mas o artigo aponta para uma consequência mais ampla. Ele argumenta que as grandes empresas de tecnologia, por não terem um suporte ao cliente que funcione, transferiram de fato suas obrigações para jornalistas, órgãos de fiscalização do governo, políticos e o sistema judicial. Em vez da visão utópica de Zuckerberg, agentes de IA vão trabalhar em nome dos usuários de formas que usam e potencialmente sobrecarregam sistemas construídos para humanos.

### Agentes como atores externos em sistemas humanos
Um agente que age em nome de um usuário interage com sistemas projetados para pessoas, como suporte ao cliente, moderação, canais de reclamação e tribunais. O caso Muse mostra o agente alcançando esses canais em escala, usando as mesmas rotas que uma pessoa usaria. A preocupação do artigo é que esses sistemas não foram construídos para esse volume nem para software agindo em nome de alguém. O caso mostra o efeito sobre os outros sistemas, não apenas sobre o usuário.

### Obrigações repassadas
A afirmação do artigo é que, quando uma empresa não oferece um suporte ao cliente que funcione, quem absorve o problema são jornalistas, órgãos reguladores, políticos e tribunais. Fowler apresenta isso como um padrão estrutural, não como uma falha isolada. Isso explica por que o processo chegou aos canais políticos e regulatórios. O artigo traz apenas esse caso, então o padrão é mostrado por meio de um exemplo.

> ❌ **Antipadrão — Aplicação automatizada sem um caminho de suporte que funcione:** um sistema de moderação desativa uma conta e o negócio afetado não tem uma rota efetiva para recuperá-la. **Por que as pessoas fazem isso:** o artigo não diz por quê; ele atribui a lacuna a empresas que não têm suporte ao cliente funcionando. **Consequência:** o negócio é prejudicado, e o ônus passa para jornalistas, órgãos governamentais, políticos e tribunais, segundo o artigo. **Correção:** oferecer à aplicação automatizada um caminho humano efetivo para contestação, que o artigo sugere estar ausente.

## Prova de humanidade
Fowler encerra observando que descarta um texto assim que ele mostra "voz de LLM" suficiente, o que já é uma forma simples de prova de humanidade. Ele pergunta que mecanismos serão inventados para provar que há um ser humano do outro lado de uma interação. Ele diz que não sabe.

### Prova de humanidade
Prova de humanidade é uma verificação de que uma pessoa, e não um agente, está do outro lado de uma interação. Fowler a enquadra como uma questão que decorre de agentes que agem em nome de usuários e sobrecarregam sistemas construídos para humanos. O hábito dele de descartar textos com voz de LLM detectável é uma versão informal dessa verificação. O artigo não oferece mecanismo nem resposta, então o conceito continua sendo uma questão em aberto.

## 🧠 Modelos mentais
### Camadas de ritmo
**O modelo:** um sistema complexo saudável tem partes que mudam em ritmos diferentes. Camadas rápidas experimentam, e camadas lentas dão continuidade, então dados autoritativos e identidade devem mudar mais devagar do que as aplicações construídas sobre eles.
**Como aplicar:** antes de decidir onde uma mudança pertence, pergunte em qual camada ela está, e mantenha os dados e a autoridade das camadas lentas estáveis enquanto deixa as aplicações das camadas rápidas mudarem livremente.

### Verificação acima da geração
**O modelo:** quando gerar código é barato, verificá-lo se torna o recurso escasso. A frase do artigo é "Code generation is no longer the bottleneck — verification is", ou seja, a geração de código não é mais o gargalo, a verificação é.
**Como aplicar:** invista em ferramentas de verificação antes de escalar a geração, e acompanhe a saúde do código para que a revisão continue barata, já que o estudo associa código saudável a uma diferença de 22 a 35% no custo da revisão.

### Velocidade governável
**O modelo:** agentes devem rodar em um ritmo que humanos ainda consigam ver e governar, e esse limite pode subir conforme a governança melhora, mas é sempre definido pelas pessoas a quem os agentes servem.
**Como aplicar:** vincule a autonomia de um agente à visibilidade que os humanos têm sobre o trabalho dele, e amplie essa autonomia apenas quando os humanos conseguirem governar as mudanças que ele faz.

## 🔑 Principais conclusões
- Em um estudo com 465 pull requests em 246 repositórios de código aberto, código saudável (CodeHealth 9.7) foi de 22 a 35% mais barato para agentes revisarem do que código não saudável (CodeHealth 2.8).
- Complex Method e Complex Conditional foram os code smells que o estudo destacou por aumentarem o custo.
- Agentes devem rodar em uma velocidade que os humanos ainda consigam governar, e esse limite é definido pelas pessoas a quem os agentes servem, não pelo que os agentes conseguem fazer.
- Grande parte do valor do SaaS está em dados, confiança, redes, conformidade, responsabilidade operacional, conhecimento de domínio, auditabilidade e responsabilização, não na aplicação visível.
- Freedman e Varón atribuem a baixa corrigibilidade a um treinamento que recompensa os agentes apenas por concluir a tarefa.
- Agentes que agem por usuários podem sobrecarregar sistemas construídos para humanos, como mostra a escalada do agente Muse, e empresas sem suporte efetivo repassam suas obrigações para outros.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um arquiteto, a transferência útil é a separação entre um núcleo estável e camadas rápidas de aplicação, além do custo de verificar o que os agentes geram. Se uma empresa caminha para uma plataforma compartilhada sob muitas aplicações geradas, qual camada deve ser dona dos dados autoritativos, das ferramentas de verificação e da trilha de auditoria?

## 📚 Plano de leitura
### Do artigo
- [Fragments: November 20](https://martinfowler.com/fragments/2026-11-20.html) — o post original, que liga cada fonte citada; o texto extraído manteve o texto dos links, mas não as URLs, então leia o original para encontrá-los.

### Para aprofundar
- Análise de Andrew Sharp sobre Jensen Huang e a crise na Nvidia — o argumento de liderança e geopolítica por trás dos comentários de Fowler.
- Steve Yegge sobre desacelerar fábricas de software — o argumento sobre o ritmo da IA a partir de uma operação em funcionamento.
- Post de Chad Fowler sobre o futuro do SaaS — a visão de uma plataforma sob muitas aplicações.
- Camadas de ritmo de Stewart Brand — a origem do modelo arquitetural usado na seção sobre SaaS.
- Coluna de Freedman e Varón na New York Times Magazine sobre corrigibilidade, baseada no trabalho de Nate Soares — o argumento completo sobre corrigibilidade.
- Reportagem de Jason Koebler na 404 Media sobre o agente Muse da Meta processando a Meta — o relato completo por trás do processo.
- CodeScene — a fonte da medida CodeHealth usada no experimento.

## Conceitos
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [multi-agent-orchestration](/pt-br/concepts/ai-ml/multi-agent-orchestration.md)
