---
type: Article Summary
title: A IA está mudando a equação entre construir ou comprar. Os CIOs estão preparados?
description: A IA reduz o custo de construir software, mas não o de mantê-lo, por isso os CIOs precisam avaliar construir ou comprar
  pelo custo total de propriedade (TCO).
resource: https://www.cio.com/article/4250943/ai-is-changing-the-build-versus-buy-equation-are-cios-ready.html
tags:
- architecture
- enterprise-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4250943/ai-is-changing-the-build-versus-buy-equation-are-cios-ready.html
  title: AI is changing the build-versus-buy equation. Are CIOs ready?
  author: CIO.com
status: stable
source_url: https://www.cio.com/article/4250943/ai-is-changing-the-build-versus-buy-equation-are-cios-ready.html
source: CIO.com
source_type: blog
published: '2026-11-20'
domain: architecture
subdomain: enterprise-architecture
concepts: []
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d816aa601e81890e61555
foundation_candidates:
- total cost of ownership
- build-versus-buy decision
- shadow IT
lang: pt-br
translation_of: /notes/architecture/2026-11-21-ai-is-changing-the-build-versus-buy-equation-are-cios-ready.md
---

## 🎯 Tese central
A IA está reduzindo o custo de criar software, mas não o custo de mantê-lo; por isso, os CIOs devem reavaliar as decisões de construir ou comprar com base no custo total de propriedade (TCO), e não apenas no custo inicial de desenvolvimento.

## 🧭 Por que o autor escreveu o artigo
O artigo parte de uma preferência antiga por comprar SaaS em vez de construir. O desenvolvimento assistido por IA agora desafia essa preferência, porque aplicações que antes levavam meses às vezes podem ser prototipadas em horas ou dias. Se a tese estiver correta, os gastos com SaaS merecem revisão, mas com base no custo total de vida útil de uma aplicação, e não na velocidade com que o código pode ser gerado.

## Abertura (sem título no original)
### Construir ou comprar
Software empresarial sempre levantou a questão de construir uma aplicação internamente ou comprá-la de um fornecedor. A resposta costuma favorecer a compra, porque software comercial, particularmente software como serviço (SaaS), oferece funcionalidades maduras, melhorias contínuas e suporte técnico. Construir aplicações sob medida normalmente exige investimento significativo em desenvolvedores, infraestrutura, testes, segurança e manutenção.

### Vibe coding
Vibe coding é o nome que o artigo dá para construir software com IA a partir de instruções em linguagem natural. Funcionários com pouca experiência em programação podem descrever o que uma aplicação deve fazer e receber uma solução funcional, enquanto desenvolvedores profissionais podem usar IA para gerar código, construir interfaces e automatizar testes. O autor pergunta se, caso a IA torne o desenvolvimento mais rápido e barato, as empresas deveriam reconsiderar quanto gastam com SaaS. A resposta é sim, com uma ressalva: a IA reduz o custo de criar software, mas não eliminou o custo de mantê-lo.

## A IA está mudando a economia do desenvolvimento de software
### Por que as organizações adotaram SaaS
As empresas adotaram SaaS sobretudo pela economia do desenvolvimento. Em vez de construir e manter aplicações, assinavam produtos consolidados e se beneficiavam do investimento contínuo do fornecedor em funcionalidades, segurança, infraestrutura e suporte.

### Complexidade do portfólio SaaS
Com o tempo, os portfólios SaaS ficaram complexos. Muitas empresas operam centenas de aplicações, algumas com funcionalidades sobrepostas, adoção limitada ou custos de assinatura que não param de subir. Unidades de negócio costumam comprar ferramentas especializadas para requisitos específicos, o que cria desafios adicionais de integração e de governança.

### Desenvolvimento assistido por IA como alternativa
O exemplo é o de um departamento que paga uma assinatura anual por uma aplicação simples, que gerencia aprovações, acompanha solicitações ou coordena um fluxo de trabalho específico. Uma substituição já exigiu uma equipe de projeto e vários meses de desenvolvimento. Com o desenvolvimento assistido por IA, ela pode se tornar economicamente atraente, sobretudo se os requisitos forem simples e a organização já tiver a infraestrutura e a expertise necessárias. Em um portfólio inteiro, substituir aplicações selecionadas, eliminar funções sobrepostas ou construir capacidades direcionadas em torno de plataformas existentes poderia gerar economias relevantes. O autor acrescenta que os ganhos de produtividade merecem exame cuidadoso.

### O estudo de produtividade da METR
Um estudo randomizado controlado de 2025, realizado pela METR, examinou como ferramentas de IA afetaram desenvolvedores experientes de código aberto trabalhando em bases de código conhecidas. Os desenvolvedores que usaram ferramentas de IA levaram cerca de 19% mais tempo para concluir suas tarefas, mesmo esperando que as ferramentas os tornassem mais rápidos. O autor limita a afirmação: o estudo cobre um único ambiente de desenvolvimento e não é evidência de que a IA, em geral, desacelera o desenvolvimento de software. O ponto é que gerar código mais rápido não significa necessariamente entregar software confiável mais rápido, por isso os CIOs devem medir a produtividade real do desenvolvimento em vez de presumir custos menores.

> 🔎 Contexto adicionado: Em um estudo randomizado controlado, os participantes são designados às condições ao acaso, de modo que as diferenças nos resultados podem ser atribuídas à condição, e não a quem por acaso estava em cada grupo.

> ❌ **Anti-padrão — presumir que a IA reduz o custo automaticamente:** escolher construir porque o código gerado por IA parece rápido, sem medir quanto tempo leva uma entrega confiável. **Por que as pessoas fazem isso:** uma demonstração que funciona é muito vívida, e o artigo observa que ver a IA produzir uma aplicação pode fazer o desenvolvimento tradicional parecer desnecessariamente complicado. **Consequência:** a economia esperada pode não aparecer, já que gerar código mais rápido não garante entregas mais rápidas nem mais confiáveis. **Correção:** medir a produtividade real de desenvolvimento antes de presumir um custo menor.

## Os custos e riscos ocultos de substituir SaaS
### Segurança do código gerado por IA
Um relatório de 2025 da Veracode que examinou código gerado por IA constatou que 45% das amostras avaliadas introduziram vulnerabilidades de segurança. O autor conclui que as organizações não podem presumir que o código gerado por IA segue práticas de segurança estabelecidas, mesmo quando a aplicação parece funcionar. Aplicações empresariais precisam de autenticação, autorização, criptografia, gestão de vulnerabilidades e proteção de dados. Dependendo da sua finalidade, também podem precisar atender a requisitos regulatórios, de privacidade e de auditoria. Essas responsabilidades continuam com a empresa, por mais rápido que a aplicação tenha sido construída.

> ❌ **Anti-padrão — julgar uma aplicação pelo fato de funcionar:** aceitar uma substituição porque ela funciona em uma demonstração. **Por que as pessoas fazem isso:** uma demo funcional é a evidência mais visível disponível, e o artigo observa que os requisitos empresariais vão muito além de o software funcionar durante uma demonstração. **Consequência:** controles ausentes de autenticação, criptografia ou proteção de dados só aparecem depois da implantação. **Correção:** avaliar requisitos de segurança e de conformidade junto com a funcionalidade antes de decidir.

### Manutenção como obrigação interna
Uma assinatura SaaS normalmente inclui melhorias do produto, gestão de infraestrutura, correções e suporte técnico. Quando a organização constrói a própria aplicação, esses itens se tornam obrigações internas. Requisitos de negócio mudam, vulnerabilidades surgem e integrações quebram, então as aplicações precisam ser monitoradas, testadas e mantidas por toda a sua vida útil. O autor aceita que a IA pode ajudar nessas atividades, mas diz que ela não elimina a necessidade de responsabilização.

### Shadow IT gerado pelo vibe coding
O vibe coding pode permitir que profissionais de fora da TI resolvam problemas departamentais sem esperar por recursos formais de desenvolvimento, sobretudo para necessidades simples. Ele também pode criar uma nova geração de shadow IT, com aplicações implantadas sem supervisão suficiente. O artigo pergunta o que acontece quando uma dessas aplicações se torna essencial: quem a mantém depois que o criador sai, quem garante que os dados continuam protegidos e se ela continua funcionando quando os sistemas subjacentes mudam. Essas perguntas pesam mais quando as aplicações sustentam transações financeiras, informações de clientes ou outros processos críticos para o negócio.

> ❌ **Anti-padrão — uma aplicação essencial sem dono:** um funcionário constrói uma aplicação da qual um departamento passa a depender, e ninguém é responsável por ela. **Por que as pessoas fazem isso:** os recursos formais de desenvolvimento podem não estar disponíveis com rapidez, então construir diretamente parece mais rápido. **Consequência:** manutenção, proteção de dados e continuidade dependem de uma só pessoa e podem falhar quando ela sai ou quando os sistemas mudam. **Correção:** a orientação do artigo é limitar as construções feitas por não-TI a aplicações leves, dentro de limites definidos, com uma governança compatível com o trabalho.

### Integração e escala
Uma aplicação SaaS aparentemente simples pode se conectar a ERP, CRM, gestão de identidades e outros sistemas. Substituí-la pode significar reconstruir integrações, migrar dados e garantir que os fluxos de trabalho existentes continuem funcionando. Uma aplicação que funciona para alguns usuários também pode se comportar de forma diferente quando atende milhares de funcionários ou grandes volumes de transações.

### Custo total de propriedade
A conclusão do artigo é que o vibe coding "pode tornar o software mais fácil de construir", mas "não necessariamente o torna mais fácil de manter". Para os CIOs, a medida relevante é o custo total de propriedade (TCO), e não o custo inicial de desenvolvimento.

## Repensando a decisão de construir ou comprar
O autor não recomenda substituir indiscriminadamente as principais plataformas SaaS. Sistemas ERP, de RH, de folha de pagamento e aplicações financeiras maduras acumulam anos de funcionalidades, regras de negócio e capacidades de conformidade. Recriá-los internamente adicionaria complexidade e risco, mesmo com maior produtividade de desenvolvimento. A oportunidade mais imediata está em aplicações SaaS especializadas, com requisitos restritos e não críticos. Algumas duplicam capacidades já disponíveis em plataformas empresariais, e outras poderiam ser desenvolvidas internamente a um custo de longo prazo menor.

Em vez de aplicar os critérios usados cinco anos atrás, o autor propõe cinco perguntas para avaliar um portfólio.

### A capacidade é estrategicamente diferenciadora?
Desenvolvimento sob medida pode oferecer mais flexibilidade e controle quando a aplicação sustenta um processo de negócio único. Capacidades padronizadas, com pouca diferenciação, podem ser mais bem atendidas por produtos comerciais.

### Qual é a complexidade da aplicação?
Fluxos simples, ferramentas de relatórios e aplicações departamentais podem se adequar ao desenvolvimento assistido por IA. Aplicações que envolvem transações complexas, requisitos regulatórios ou operações críticas exigem mais escrutínio.

### Qual é o custo total de propriedade real?
A análise deve incluir taxas de assinatura, desenvolvimento, infraestrutura, integração, segurança, manutenção e suporte contínuo. Os recursos internos necessários para operar a aplicação também devem ser contabilizados.

### Temos capacidade para sustentar o que construímos?
A IA pode reduzir a programação manual, mas as organizações ainda precisam de padrões de arquitetura, práticas de desenvolvimento seguro, testes e suporte operacional. Sem isso, a dívida técnica pode se acumular rapidamente.

### Podemos alcançar o mesmo resultado estendendo o que já temos?
Antes de substituir uma aplicação SaaS, as organizações devem verificar se as plataformas empresariais existentes já oferecem a funcionalidade necessária. Muitas empresas já pagam por capacidades que não adotaram totalmente. O resumo do autor: a melhor alternativa a comprar mais uma aplicação SaaS pode ser aproveitar melhor a tecnologia já disponível, e não construir uma nova.

> 📋 **Checklist — cinco perguntas para a decisão de construir ou comprar**
> - [ ] A capacidade é estrategicamente diferenciadora?
> - [ ] Qual é a complexidade da aplicação?
> - [ ] Qual é o custo total de propriedade real?
> - [ ] Temos capacidade para sustentar o que construímos?
> - [ ] Podemos alcançar o mesmo resultado estendendo o que já temos?

### Dois modos de desenvolvimento assistido por IA
O artigo distingue desenvolvedores profissionais, que usam IA aplicando práticas de engenharia consolidadas, de usuários de negócio (descritos como profissionais não-TI, ou "usuários"), que criam aplicações leves dentro de limites definidos. As duas abordagens têm valor, mas exigem níveis diferentes de governança.

### Pressão de preços dos fornecedores
As capacidades de IA que tornam o desenvolvimento sob medida mais atraente também estão disponíveis para os provedores SaaS, que podem usá-las para acelerar o desenvolvimento, automatizar o suporte e adicionar funcionalidades. O autor sugere que os CIOs questionem se a precificação SaaS tradicional ainda faz sentido. As eficiências chegarão aos preços das assinaturas, ou os fornecedores continuarão aumentando preços à medida que acrescentam funcionalidades de IA e novos modelos de licenciamento? Essas perguntas valem a pena nas renovações de software e nas negociações comerciais.

### Combinar construir e comprar
Em muitos casos, a melhor resposta combina os dois. As organizações podem manter plataformas empresariais centrais enquanto desenvolvem aplicações, fluxos de trabalho e agentes de IA direcionados que as estendem. Isso preserva sistemas estáveis e permite que o negócio inove com mais rapidez.

O argumento final é que a IA cria oportunidades para simplificar portfólios de aplicações, reduzir gastos desnecessários com software e construir capacidades que servem ao negócio. Aproveitá-las exige uma avaliação disciplinada, e não entusiasmo com a rapidez com que o software pode ser gerado. A pergunta já não é simplesmente se uma aplicação é mais barata de construir do que de comprar, mas se construir, comprar ou estender a tecnologia existente vai entregar o melhor resultado de negócio ao longo de toda a vida útil da aplicação.

## 🧠 Modelos mentais
### O custo de criação é a primeira linha, não o total
**O modelo:** o preço de construir software é apenas o começo do custo. Ele continua por integração, segurança, manutenção e suporte enquanto a aplicação estiver em uso.
**Como aplicar:** antes de comparar uma estimativa de construção com uma assinatura, liste todos os custos de propriedade da lista de TCO do artigo e compare os valores completos de vida útil.

### Funcionar não é o mesmo que estar pronto para produção
**O modelo:** uma aplicação que funciona durante uma demonstração diz pouco sobre segurança, integração, escala ou suporte de longo prazo.
**Como aplicar:** teste um candidato a substituição quanto a autenticação, proteção de dados, integração e carga antes de considerá-lo uma alternativa ao SaaS.

### Construir, comprar ou estender
**O modelo:** construir versus comprar é uma visão incompleta. O artigo acrescenta uma terceira opção: estender a tecnologia que a organização já possui.
**Como aplicar:** verifique se uma plataforma empresarial existente pode atender à necessidade antes de aprovar uma nova construção ou compra.

## 🔑 Principais conclusões
- A IA reduz o custo de criar software, mas, segundo o autor, ela "não eliminou o custo de manter software".
- O estudo METR de 2025 constatou que desenvolvedores experientes que usaram ferramentas de IA levaram cerca de 19% mais tempo nas tarefas atribuídas, apesar de esperarem ser mais rápidos. O autor limita isso a um único ambiente de desenvolvimento.
- Um relatório da Veracode de 2025 constatou que 45% das amostras de código gerado por IA avaliadas introduziram vulnerabilidades de segurança.
- Substituir uma aplicação SaaS pode significar reconstruir integrações com ERP, CRM e sistemas de identidade, migrar dados e lidar com volumes de usuários e transações muito maiores.
- Plataformas maduras de ERP, RH, folha de pagamento e finanças não devem ser substituídas indiscriminadamente. A oportunidade está em aplicações SaaS especializadas e não críticas.
- Cinco perguntas estruturam a decisão: diferenciação, complexidade, custo total de propriedade, capacidade de sustentar o que é construído e se as plataformas existentes podem entregar o resultado.
- Fornecedores SaaS têm acesso às mesmas capacidades de IA, então a precificação SaaS deve ser questionada na renovação.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Negócio (construir ou comprar como decisão de negócio, capacidades, resultado de negócio) e Aplicação (portfólio SaaS, substituição, integração). Tecnologia e Dados aparecem apenas como itens de custo (infraestrutura, migração e proteção de dados), e não como domínios analisados. |
| Escopo | Empresarial: todo o portfólio de software, com aplicações individuais como unidades de decisão. |
| Stakeholders e preocupações | CIOs (decisões de portfólio e renovação, custo total); unidades de negócio (ferramentas específicas, requisitos); funcionários e criadores não-TI (velocidade, propriedade após a saída deles); segurança e conformidade (vulnerabilidades, auditoria); fornecedores SaaS (precificação sob IA). |
| Frameworks e notações | Nenhum citado. |

### Decisões e trade-offs
- **Construir, comprar ou estender, por aplicação:** as opções são manter o SaaS, construir com IA ou estender uma plataforma existente. O autor recomenda avaliar cada uma pelas cinco perguntas e, em muitos casos, combinar construir e comprar. O custo é a responsabilidade interna por manutenção, segurança e integração, além da dívida técnica se faltarem capacidades de suporte.
- **Substituir plataformas empresariais maduras:** as opções são substituí-las ou mantê-las. O autor as mantém e direciona a substituição primeiro para SaaS especializados. O custo é a economia perdida, em troca de evitar a complexidade e o risco de recriar anos de funcionalidades e conformidade.
- **Quem pode construir:** desenvolvedores profissionais que usam IA dentro de práticas de engenharia, versus usuários de negócio dentro de limites definidos. O custo são dois níveis de governança para operar e manter.
- **Renovações de assinatura:** as opções são aceitar os preços atuais ou questioná-los à luz da economia do desenvolvimento impulsionada pela IA. O autor recomenda levantar a questão nas renovações e negociações. O artigo não apresenta custo quantificado para nenhuma das escolhas.

### Fundamentos em que se apoia
- [Arquitetura Empresarial](/pt-br/foundations/enterprise-architecture.md) — O artigo aplica o escopo de toda a empresa a um portfólio de software e aos seus gastos, não ao alinhamento de processos, informação e tecnologia com a estratégia.
- [capacidade de negócio](/pt-br/foundations/business-capability.md) — O artigo usa "capacidades" para habilidades organizacionais (por exemplo, "as capacidades para sustentar o que construímos"), em um sentido mais amplo do que a definição do fundamento sobre o que o negócio faz.
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md) — O artigo trata aplicações e suas integrações como as unidades a manter, substituir ou construir, o que se encaixa no agrupamento de aplicações em torno de funções de negócio do fundamento.

## 🏛️ Para o Kenji
Na atividade de um arquiteto, isso se aplica quando uma ferramenta SaaS departamental entra em renovação, consolidação ou substituição. A lista de TCO e a verificação de integração são reutilizáveis em revisões de portfólio. O tema também toca arquitetura de negócio e o caso financeiro que um MBA em arquitetura avaliaria. Pergunta levantada: como você estimaria o custo total de uma aplicação construída internamente que precisa continuar funcionando depois que quem a construiu sair? Esta é a sua interpretação, não a do autor.

## 📚 Plano de leitura
### Do artigo
- O texto extraído mantém o texto do link para o estudo randomizado controlado da METR de 2025 e para o relatório da Veracode de 2025 sobre código gerado por IA, mas nenhuma URL sobreviveu, então nenhuma é listada aqui. Leia essas duas fontes primeiro para verificar os números de 19% e 45%.

### Para aprofundar
- Custo total de propriedade para portfólios de software — para embasar a comparação de custos em um modelo padrão.
- Governança de shadow IT para aplicações construídas pelo negócio — para definir limites para construtores não-TI.
- Integração e migração de dados ao substituir SaaS — para dimensionar os custos ocultos que o artigo descreve.
