---
type: Article Summary
title: 'QCon San Francisco 2026: Escalando tecnologia alinhando pessoas'
description: Engenheiros staff-plus podem ampliar o impacto alinhando pessoas em torno de uma estratégia técnica curta, uma north star e apostas explícitas.
resource: https://www.infoq.com/news/2026/11/scalling-tech-humans
tags:
- architecture
- architecture-governance
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/scalling-tech-humans
  title: 'QCon San Francisco 2026: Scaling Tech by Aligning Humans'
  author: Daniel Dominguez
status: stable
source_url: https://www.infoq.com/news/2026/11/scalling-tech-humans
source: InfoQ
source_type: blog
published: '2026-11-21'
domain: architecture
subdomain: architecture-governance
concepts: []
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d815c8baac9e9cc093e1d
foundation_candidates:
- technical strategy
- north star
- strategy bet
lang: pt-br
translation_of: /notes/architecture/2026-11-22-qcon-san-francisco-2026-scaling-tech-by-aligning-humans.md
---

## 🎯 Tese central
Um engenheiro sênior pode ampliar o impacto alinhando pessoas em torno de uma estratégia técnica curta, uma north star e algumas apostas explícitas, e isso pode alcançar mais longe do que qualquer código que um único engenheiro escreva.

## 🧭 Por que o autor escreveu
Várias estratégias de produto começaram a compartilhar riscos técnicos semelhantes, e, sem uma estratégia técnica comum por baixo delas, as equipes otimizavam localmente para objetivos diferentes. Se a tese estiver certa, uma estratégia técnica leve, construída com as pessoas, dá a essas equipes uma direção única mais rápido do que um longo documento técnico.

## Abertura: estratégia como alavanca
Na QCon San Francisco 2026, Scott Behrens, Principal Security Engineer na Netflix, apresentou como engenheiros staff-plus podem definir e alinhar uma estratégia técnica para toda a empresa, com base na experiência da Netflix ao escalar agentes internos de IA. A tese central é que uma estratégia que reúne pessoas pode, muitas vezes, ampliar o impacto mais do que qualquer código que um engenheiro conseguiria escrever sozinho.

### Staff-plus engineer
Na palestra, o staff-plus engineer identifica o problema, escreve o pitch e lidera a equipe que constrói a estratégia. > 🔎 Contexto adicional: "Staff-plus" é um título do setor para engenheiros sêniores que influenciam além da própria equipe sem gerenciar pessoas; o artigo não define o termo.

### Estratégia técnica
Nas palavras de Behrens, uma estratégia é simplesmente um conjunto de escolhas. O ponto central do artigo é que uma estratégia técnica pode complementar uma estratégia de produto sem esperar por ela, de modo que as duas caminhem lado a lado. Behrens argumentou que ela deve ser leve, porque estratégias leves são entregues mais rápido do que longos documentos técnicos. Seu propósito é dar a várias equipes uma direção compartilhada para os riscos técnicos que todas enfrentam.

## Por que o trabalho começou
Várias estratégias de produto, cada uma com sua própria direção, começaram a compartilhar riscos técnicos semelhantes. Sem uma estratégia técnica comum por baixo delas, as equipes otimizavam localmente para objetivos diferentes. Behrens tirou três conclusões: uma estratégia técnica pode complementar a estratégia de produto sem esperar por ela, estratégias leves são entregues mais rápido do que longos documentos técnicos, e uma north star clara orienta mais do que as pessoas esperam.

> ❌ **Anti-padrão — Otimização local:** Cada equipe segue a própria direção enquanto compartilha risco técnico com as outras. **Por que as pessoas fazem isso:** Cada equipe é medida pelos próprios objetivos, então a otimização local parece racional. **Consequência:** As equipes trabalham em direções opostas sobre o mesmo risco. **Correção:** Acordar uma estratégia técnica comum por baixo das estratégias de produto, como recomenda a palestra.
> 🔎 Contexto adicional: As causas e consequências nos blocos de anti-padrão desta nota são a minha leitura, já que a palestra nomeou os modos de falha sem explicá-los.

## O playbook: encontrar o problema certo
Behrens apresentou o playbook como níveis de um videogame, e o primeiro nível é encontrar o problema certo. Ele sugeriu observar as comunicações da empresa, as tendências de incidentes, a dívida técnica, as conversas com outros tech leads e as lacunas entre os charters das equipes. O problema é então testado com três perguntas: vale resolvê-lo agora, ele é fundamental ou cumulativo, e esta é a pessoa certa para liderá-lo?

### Problema fundamental ou cumulativo
O artigo pergunta se um problema é fundamental ou cumulativo, mas não define nenhum dos dois termos. > 🔎 Contexto adicional: Na minha leitura, um problema fundamental é aquele do qual outros trabalhos dependem, e um problema cumulativo fica mais caro quanto mais tempo espera. A distinção serve para decidir se um problema merece esforço de estratégia agora.

> 📋 **Checklist — Fontes para encontrar o problema certo**
> - [ ] Comunicações da empresa
> - [ ] Tendências de incidentes
> - [ ] Dívida técnica
> - [ ] Conversas com outros tech leads
> - [ ] Lacunas entre os charters das equipes

## O pitch
Escolhido o problema, o próximo passo é uma declaração de problema curta que explique a intenção, o valor esperado, os problemas com evidências de apoio, por que o trabalho importa agora e os próximos passos datados. Behrens sugeriu deixar as abordagens possíveis em aberto para que quem se juntar possa moldá-las, e testar a declaração cedo com alguns colegas, incluindo o provável dissidente.

### Declaração de problema
Uma declaração de problema é um pitch escrito e curto, cuja função é trazer as pessoas para o trabalho. Suas partes são intenção, valor esperado, os problemas com evidências, prazo e próximos passos datados. Deixar as abordagens em aberto convida os outros a moldar o trabalho, em vez de aderir a um plano pronto. Testá-la cedo com o provável dissidente expõe evidências fracas antes que o grupo mais amplo as veja.

> 📋 **Checklist — Conteúdo da declaração de problema**
> - [ ] Intenção
> - [ ] Valor esperado
> - [ ] Problemas, com evidências de apoio
> - [ ] Por que o trabalho importa agora
> - [ ] Próximos passos datados

> ❌ **Anti-padrão — Declaração de problema pouco clara:** Um pitch que deixa de fora intenção, valor esperado ou evidências. **Por que as pessoas fazem isso:** Um pitch pode parecer pronto antes que as razões por trás dele estejam escritas. **Consequência:** As pessoas não conseguem saber se devem se juntar. **Correção:** Use as cinco partes acima e teste a declaração cedo.

## A tiger team
Com uma equipe a formar, Behrens descreveu uma pequena tiger team cuja função é criar a estratégia, e não executá-la. Seus membros devem misturar perspectivas, sentir-se à vontade com a ambiguidade, ter apoio de seus gestores e combinar escopo, cronograma e uma condição de parada clara desde o início. A primeira reunião dele teve gente demais e, ainda assim, deixou de fora funções-chave. Ele enfatizou que o traço mais importante de uma boa equipe é tornar seguro contestar.

### Tiger team
A tiger team é pequena e existe para criar a estratégia, por isso seus membros não a executam. A condição de parada, acordada no início, define quando o trabalho da equipe termina. Misturar perspectivas importa porque as perguntas iniciais não têm respostas assentadas, e por isso o artigo também valoriza pessoas que toleram ambiguidade.

> 📋 **Checklist — Montando a tiger team**
> - [ ] Misturar perspectivas
> - [ ] Escolher pessoas à vontade com ambiguidade
> - [ ] Obter apoio da gestão
> - [ ] Acordar escopo, cronograma e uma condição de parada clara desde o início

> ❌ **Anti-padrão — Primeira reunião superdimensionada:** Convidar gente demais e, ainda assim, deixar de fora funções-chave, como aconteceu na primeira reunião de Behrens. **Por que as pessoas fazem isso:** Um convite amplo parece mais seguro do que escolher quem importa. **Consequência:** O grupo demora a decidir e as funções-chave continuam ausentes. **Correção:** Convide um grupo pequeno que cubra as funções-chave.

## A north star
Com uma equipe no lugar, o próximo passo é uma north star: uma declaração curta e aspiracional que dá direção e ajuda a responder se um trabalho aproxima ou afasta a organização de seu objetivo. Behrens observou que uma boa north star é mais difícil de escrever do que parece, por isso recomendou redigi-la com a equipe e combiná-la com resultados-chave que definam o que precisa ser verdade.

### North star
A north star é curta e aspiracional, para que as pessoas possam lembrá-la e aplicá-la a trabalhos novos. Seu teste é se um trabalho aproxima ou afasta a organização do objetivo. Os resultados-chave que a acompanham declaram o que precisa ser verdade para que a north star se sustente.

> ❌ **Anti-padrão — North star vaga:** Uma declaração geral demais para responder se um trabalho ajuda. **Por que as pessoas fazem isso:** Escrever uma north star precisa é mais difícil do que parece. **Consequência:** As decisões não são orientadas. **Correção:** Redija-a com a equipe e combine-a com resultados-chave.

## Apostas estratégicas
Behrens descreveu a estratégia como um conjunto de escolhas, e cada escolha como uma aposta. Ele sugeriu escrever uma aposta como uma escolha contra outra escolha razoável, nomear explicitamente o trade-off, manter cada aposta curta e revisar as apostas ao longo do tempo. Se uma aposta não parece um pouco desconfortável, disse ele, provavelmente não é uma aposta de verdade, e manter apenas um pequeno conjunto de apostas ativas mantém a estratégia focada.

### Aposta estratégica
Uma aposta estratégica é uma escolha feita contra uma alternativa específica, e por isso o trade-off precisa ser nomeado. O artigo pede apostas curtas para que possam ser lidas rapidamente e revisadas conforme o trabalho muda. Uma aposta que parece confortável geralmente não nomeou um trade-off real.

> ❌ **Anti-padrão — Apostas que não são de fato apostas:** Escolhas sem alternativa real, de modo que nenhum trade-off é nomeado e nada parece desconfortável. **Por que as pessoas fazem isso:** Escolhas confortáveis são mais fáceis de acordar. **Consequência:** A estratégia não tem direção real. **Correção:** Nomeie a alternativa e o trade-off, e reescreva qualquer aposta que pareça confortável.

## Comunicação e feedback
Behrens sugeriu expandir o apoio gradualmente, da equipe imediata para gerentes, diretores e executivos, recrutar campeões em toda a organização e mostrar abertamente como o feedback mudou a estratégia. Ele definiu o sucesso como o momento em que você não está na sala e as pessoas ainda conseguem explicar o trabalho. Ele também revisou modos de falha comuns: declarações de problema pouco claras, a equipe errada, north stars vagas, apostas que não são de fato apostas e estratégias que ninguém sabe que existem.

### Campeão
O artigo recomenda recrutar campeões em toda a organização sem definir o papel. > 🔎 Contexto adicional: Na minha leitura, um campeão é uma pessoa respeitada em outra parte da empresa que leva a estratégia para as próprias discussões, o que permite que ela se espalhe além das pessoas que a escreveram.

> ❌ **Anti-padrão — Estratégia que ninguém sabe que existe:** Uma estratégia mantida dentro da equipe que a escreveu, de modo que os outros não conseguem explicá-la nem agir a partir dela. **Por que as pessoas fazem isso:** Escrever para a própria equipe parece suficiente no início. **Consequência:** Ninguém fora da equipe consegue usar a estratégia. **Correção:** Expanda o apoio gradualmente e mostre abertamente como o feedback mudou a estratégia.

## Encerramento: mantenha simples
A palestra terminou pedindo que engenheiros mantenham a estratégia simples, comuniquem-na amplamente, usem uma north star curta para orientar as decisões e adaptem o playbook ao próprio contexto. Behrens sugeriu que, à medida que as organizações lançam mais iniciativas em paralelo, especialmente em torno de IA, a capacidade de engenheiros sêniores de alinhar muitas equipes em torno de uma direção técnica única pode se tornar tão valiosa quanto os sistemas que constroem.

## 🧠 Modelos mentais
### Playbook como níveis ordenados
**O modelo:** Behrens enquadrou o trabalho como níveis de um videogame, percorridos em ordem, começando por encontrar o problema certo e depois escrever o pitch. **Como aplicar:** Não escreva o pitch até que o problema tenha passado pelas três perguntas do primeiro nível.

### Sucesso é quando você não está na sala
**O modelo:** A estratégia funcionou quando pessoas que não fizeram parte do trabalho ainda conseguem explicá-la. **Como aplicar:** Teste isso pedindo a alguém de fora da equipe que explique de volta para você a north star e as apostas ativas.

### Uma aposta deve parecer um pouco desconfortável
**O modelo:** Se uma aposta não parece um pouco desconfortável, provavelmente não nomeia um trade-off real. **Como aplicar:** Para cada aposta, escreva a alternativa razoável que você rejeitou; se não conseguir nomear uma, reescreva a aposta.

## 🔑 Principais conclusões
- Uma estratégia técnica curta pode ampliar o impacto de um engenheiro sênior além de qualquer código que ele escreva, argumentou a palestra.
- A estratégia técnica pode complementar a estratégia de produto sem esperar que ela exista.
- Behrens argumentou que estratégias leves são entregues mais rápido do que longos documentos técnicos.
- O pitch deve declarar intenção, valor esperado, evidências, por que o trabalho importa agora e próximos passos datados, e deixar as abordagens em aberto.
- A tiger team cria a estratégia em vez de executá-la, com uma condição de parada acordada desde o início e a regra de que contestar é seguro.
- Mantenha apenas um pequeno conjunto de apostas ativas, cada uma nomeando seu trade-off, e oriente as decisões com uma north star curta.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Negócio e Tecnologia. Negócio cobre estratégias de produto e como as equipes são alinhadas; Tecnologia cobre os riscos técnicos compartilhados e a estratégia técnica. Dados e Aplicações não são cobertos; agentes de IA aparecem apenas como pano de fundo do trabalho da Netflix. |
| Escopo | Empresarial: a palestra enquadra o trabalho como uma estratégia técnica para toda a empresa, embora comece com uma equipe pequena. |
| Stakeholders e preocupações (concerns) | Engenheiros staff-plus, tech leads, equipes de produto, gerentes, diretores e executivos. Preocupações declaradas ou implícitas: equipes otimizando para objetivos diferentes, problemas pouco claros, escolha das pessoas certas para liderar e se a estratégia é compreendida além de seus autores. |
| Frameworks e notações | nenhum citado |

### Decisões e trade-offs
- **Estratégia técnica junto à estratégia de produto:** O autor recomenda escrevê-la sem esperar pela estratégia de produto. O artigo não cita nenhum custo.
- **Estratégia leve em vez de longos documentos técnicos:** O autor recomenda a forma curta, que, segundo ele, é entregue mais rápido. O artigo não cita nenhum custo.
- **Pequena tiger team que cria a estratégia:** O autor recomenda uma equipe pequena com condição de parada. O artigo não cita nenhum custo; o trade-off implícito é que os membros da equipe não estão executando durante esse trabalho.
- **Abordagens em aberto no pitch:** O autor recomenda deixar as abordagens em aberto para que quem se junta possa moldá-las. O artigo não cita nenhum custo; o trade-off implícito é que o pitch se compromete com menos.
- **Um pequeno conjunto de apostas ativas:** O autor recomenda manter poucas apostas ativas. O artigo não cita nenhum custo; o trade-off implícito é que o trabalho fora das apostas ativas recebe menos direção.

### Fundamentos em que se apoia
- [Arquitetura de Negócio](/pt-br/foundations/business-architecture.md) — O artigo relaciona uma estratégia técnica às estratégias de produto, o que estende o foco da definição em estratégias e produtos para uma estratégia técnica correndo ao lado deles.
- [Arquitetura Empresarial](/pt-br/foundations/enterprise-architecture.md) — A palestra busca orientar mudanças entre equipes rumo a uma direção única, mas não cobre processos nem informação, que a definição também inclui.
- [Arquitetura](/pt-br/foundations/architecture.md) — A "estratégia" do artigo significa um conjunto de escolhas sobre direção, e não as propriedades do sistema ou os princípios de design que a definição descreve.

## 🏛️ Para Kenji
Esta é a sua interpretação, não a do autor. Para um Staff Architect, a palestra é um relato prático de como moldar a direção técnica entre equipes sem depender apenas de código, e seu foco em trade-offs explícitos está próximo do trabalho de estratégia de um MBA. Pergunta: quando várias equipes já executam suas próprias estratégias, qual risco técnico compartilhado você colocaria como primeira aposta, e qual alternativa razoável nomearia contra ela?

## 📚 Plano de leitura
### Do artigo
O artigo não cita links, então não há nada a seguir a partir dele.
### Para ir além
- Arquitetura de negócio no TOGAF 9.2 — para comparar a estratégia técnica da palestra com a visão mais ampla do TOGAF sobre estratégias e produtos.
- Trabalho de agentes de IA internos da Netflix — a experiência em que a palestra se baseou, embora o artigo não cite a fonte.
