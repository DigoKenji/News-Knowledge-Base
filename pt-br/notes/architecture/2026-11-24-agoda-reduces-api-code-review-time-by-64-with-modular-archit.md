---
type: Article Summary
title: Agoda Reduz em 64% o Tempo de Revisão de Código de API com Arquitetura Modular
description: O framework de modular monolith da Agoda impõe limites de domínio com testes automatizados, reduzindo em 64% a mediana do tempo de revisão, sem reescrita.
resource: https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework
tags:
- architecture
- software-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework
  title: Agoda Reduces API Code Review Time by 64% with Modular Architecture
  author: Leela Kumili
status: stable
source_url: https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework
source: InfoQ
source_type: blog
published: '2026-11-23'
domain: architecture
subdomain: software-architecture
concepts:
- bounded-context
- modular-monolith
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d815ebe26f620ce7071e7
foundation_candidates:
- modular monolith
- architectural fitness function
- data mapper pattern
lang: pt-br
translation_of: /notes/architecture/2026-11-24-agoda-reduces-api-code-review-time-by-64-with-modular-archit.md
---

## 🎯 Tese central
A Agoda consegue impedir que um serviço existente fique cada vez mais emaranhado sem reescrevê-lo: organiza o código em componentes de domínio, impõe os limites desses componentes com testes automatizados e migra de forma incremental. Isso reduziu o tempo de revisão de código.

## 🧭 Por que o autor escreveu o artigo
A API pós-reserva (BAPI) tinha ficado difícil de manter: os merge requests mais que quadruplicaram entre 2022 e 2025, e alguns arquivos ultrapassavam 4.000 linhas. O artigo sustenta que um framework com limites impostos, adotado gradualmente, resolve o problema sem uma reescrita. Se a tese estiver correta, as mudanças ficam restritas a um domínio de negócio e as revisões ficam mais rápidas.

## A abordagem e os principais resultados
A Agoda introduziu um framework de modular monolith na sua API pós-reserva para reduzir o acoplamento de código e impor limites de domínio, sem reescrever o serviço existente. A abordagem combina organização do código em componentes, verificações automatizadas de arquitetura e migração incremental. Em comparação com o código legado, nas mudanças baseadas no framework, a equipe relatou uma redução de 64% no tempo mediano de revisão de código e de 34% no tempo até o primeiro feedback substancial.

### Modular monolith
🔎 Contexto adicional: Um modular monolith é uma única aplicação implantável, dividida em módulos com limites internos impostos, em vez de serviços separados. O artigo usa o termo na frase de abertura e descreve os limites que fazem essa abordagem funcionar.

### Migração incremental
A equipe não começou do zero. Introduziu limites arquiteturais passo a passo, para que o serviço existente continuasse rodando enquanto o código novo seguia a nova estrutura.

## Por que a API precisou mudar
A BAPI coordena cancelamentos, alterações de reservas, reembolsos e comunicações com clientes. A Agoda extraiu a API do seu monólito central, já envelhecido, em 2019, por meio de uma migração em grande parte linha a linha. Depois disso, o desenvolvimento aumentou: os merge requests mais que quadruplicaram entre 2022 e 2025, e alguns arquivos passaram de 4.000 linhas.

### Extração linha a linha
Uma migração linha a linha move o código praticamente como está, sem redesenhá-lo. Foi rápida, mas levou a estrutura antiga para o novo serviço. É por isso que o tamanho e o emaranhado do código cresceram junto com a carga de trabalho da equipe.

## Um trade-off tático e um caminho incremental
A equipe descreve a migração de 2019 como um trade-off tático de design, apoiando-se em *A Philosophy of Software Design*, de John K. Ousterhout:

> We had deliberately traded a strategic design approach for a tactical one to prioritize speed to market. This tactical mindset was exactly what the business needed at the time.

> *Tradução livre:* Tínhamos trocado deliberadamente uma abordagem de design estratégico por uma abordagem tática, para priorizar a velocidade de chegada ao mercado. Essa mentalidade tática era exatamente o que o negócio precisava naquele momento.

Em vez de fazer uma reescrita completa, a equipe introduziu limites arquiteturais de forma incremental.

### Design tático versus estratégico
O design tático foca em entregar a próxima funcionalidade rapidamente. O design estratégico investe antecipadamente em uma estrutura que torna as mudanças futuras mais baratas. O artigo não afirma que a escolha tática estava errada: ela era adequada ao negócio na época, e o custo chegou depois, na forma de pressão de manutenção.

### Introdução incremental em vez de reescrita
A equipe colocou limites em torno do código existente aos poucos. Isso mantém as entregas fluindo e evita uma substituição arriscada de uma só vez, ao custo de estruturas antigas e novas coexistirem por um período.

## Platform: organização por domínio de negócio
O framework, chamado Platform, organiza o código em torno de domínios de negócio, como cancelamentos, remarcações e pagamentos, em vez de camadas técnicas. Cada componente encapsula sua lógica de negócio e o acesso a dados atrás de interfaces definidas, com as camadas application, service, repository e model contidas dentro do componente. O artigo inclui um diagrama dos limites dentro de um módulo de domínio; a imagem em si não está no texto extraído.

### Package-by-component
Em vez de agrupar todos os controllers juntos, todos os services juntos e todos os repositories juntos, o código é agrupado por capacidade de negócio. Uma mudança em cancelamentos passa a tocar um único componente, e não todas as camadas da aplicação. O objetivo é localizar as mudanças e reduzir as dependências entre domínios de negócio.

### Encapsulamento atrás de interfaces
Cada componente expõe interfaces definidas e mantém suas camadas internas ocultas. Outros componentes chamam a interface, não os internos, então os internos podem mudar sem afetar o restante do sistema.

## Data Mapper e builders tipados
A Agoda usa o padrão Data Mapper para manter modelos específicos de repositório, incluindo entidades de banco de dados e payloads de API, fora da lógica de negócio. Os builders tipados centralizam a criação de objetos e as transformações, facilitando para os desenvolvedores encontrar onde os objetos de domínio e as entradas de serviço são construídos.

### Padrão Data Mapper
Um data mapper traduz entre o formato dos dados armazenados ou transportados e os objetos de domínio com que a lógica de negócio trabalha. Assim, a lógica de negócio nunca depende de entidades de banco de dados nem de tipos de payload de API, e mudanças de armazenamento ou de payload ficam restritas à camada de mapeamento.

> ❌ **Anti-padrão — modelos de repositório na lógica de negócio:** o código de negócio lê e grava diretamente entidades de banco de dados ou payloads de API. **Por que as pessoas fazem isso:** o artigo não diz; é o caminho mais curto quando o modelo já existe. **Consequência:** a lógica de negócio fica presa aos formatos de armazenamento e de API, então toda mudança de schema se propaga por ela. **Correção:** direcionar todas as conversões por um Data Mapper, para que a lógica de negócio veja apenas objetos de domínio.

### Builders tipados
Um builder tipado é um objeto dedicado que cria um objeto de domínio ou uma entrada de serviço em um único lugar. Centralizar a criação facilita para o desenvolvedor encontrar onde um objeto é construído e transformado, em vez de procurar em toda a base de código.

## Regras de arquitetura como testes no CI
A equipe usa o ArchUnit para codificar restrições arquiteturais como testes automatizados no pipeline de integração contínua. Por exemplo, modelos específicos de um repositório só podem ser acessados pelos respectivos repositórios. As violações quebram o build, em vez de depender de revisores para identificar manualmente dependências não autorizadas.

> ❌ **Anti-padrão — depender dos revisores para os limites:** um revisor humano é a única verificação de dependências não autorizadas. **Por que as pessoas fazem isso:** o artigo não diz; a revisão é a rede de segurança padrão já existente. **Consequência (conforme sugerido pelo artigo):** a estrutura se desvia, e os revisores a percebem tarde ou nunca. **Correção:** codificar a regra como um teste que quebra o build.

### ArchUnit
🔎 Contexto adicional: ArchUnit é uma biblioteca para escrever regras de arquitetura como verificações no estilo de testes unitários, por exemplo, que um pacote não pode depender de outro. As regras rodam como qualquer outro teste, então as violações aparecem no CI.

### Architectural fitness functions
A equipe descreveu essas restrições como architectural fitness functions, um conceito discutido por Neal Ford e Mark Richards em *Software Architecture: The Hard Parts*. Um engenheiro explicou:

> By codifying our layer boundaries into an ArchUnit test suite, our CI pipeline serves as an automated guardrail, catching structural drift on every pull request before a human reviewer ever sees it.

> *Tradução livre:* Ao codificar nossos limites de camada em uma suíte de testes ArchUnit, nosso pipeline de CI funciona como uma barreira automatizada, detectando o desvio estrutural em cada pull request antes que um revisor humano o veja.

A ideia é que uma propriedade arquitetural seja verificada de forma automática e contínua, para que não se erode sem ser notada.

## Adoção padrão e resultados medidos
A Platform agora é o padrão para novos módulos, enquanto as funcionalidades existentes são migradas de forma incremental, quando o desenvolvimento de uma funcionalidade oferece uma oportunidade. A Agoda comparou merge requests baseados no framework com os legados no segundo trimestre de 2026. O tempo mediano para concluir a revisão foi de 1,08 hora para mudanças baseadas no framework, contra 3,02 horas para mudanças legadas. O tempo até o primeiro feedback substancial foi de 10,34 horas contra 15,77 horas.

### Platform como padrão
Novos módulos começam no framework, e a migração do código antigo acontece apenas quando uma funcionalidade oferece um motivo natural para tocá-lo. Isso evita um projeto de migração dedicado enquanto cresce a parcela de código estruturado.

### Uso de prompts de IA
Entre junho e agosto, as mudanças baseadas no framework usaram 29% menos prompts de IA por 100 linhas alteradas, embora a mediana de prompts por merge request tenha sido semelhante (20 contra 22). A Agoda ressalvou que os resultados de IA são direcionais, devido ao período curto de observação. Leia o resultado de IA como um sinal, não como uma conclusão.

## Uma abordagem relacionada
Uma abordagem relacionada é o Spring Modulith, que oferece verificação de módulos e checagens de ciclos de dependência para aplicações Spring Boot.

### Spring Modulith
🔎 Contexto adicional: Spring Modulith é um projeto Spring Boot para estruturar uma aplicação em módulos. Suas verificações de módulos e de ciclos são o mesmo tipo de imposição automatizada de limites que o artigo descreve com o ArchUnit.

## 🧠 Modelos mentais
### Design tático versus estratégico
**O modelo:** o trabalho tático otimiza a próxima entrega, enquanto o trabalho estratégico investe em uma estrutura que compensa ao longo do tempo. A velocidade tática no início é uma escolha legítima quando o negócio precisa dela, mas a dívida é real.
**Como aplicar:** ao aceitar um atalho tático, registre qual estrutura você está adiando e quando vai pagá-la.

### Architectural fitness functions
**O modelo:** uma propriedade arquitetural, como "esta camada não pode chamar aquela", é um teste executado a cada mudança.
**Como aplicar:** escolha as duas ou três fronteiras que mais se desviam na sua base de código e codifique cada uma como um teste que falha no CI.

### Package-by-component
**O modelo:** agrupe o código por capacidade de negócio, com as camadas dentro, para que uma mudança em um domínio fique em um só lugar.
**Como aplicar:** antes de criar um módulo, pergunte qual capacidade de negócio é dona da mudança; se a resposta forem várias, o limite está errado.

## 🔑 Principais conclusões
- O framework da Agoda reduziu em 64% a mediana do tempo de revisão de código para mudanças baseadas no framework, em comparação com o código legado.
- Os limites foram adicionados sem reescrita, migrando o código de forma incremental e tornando o framework o padrão para novos módulos.
- A Platform agrupa o código por domínio de negócio, como cancelamentos, remarcações e pagamentos, e não por camada técnica.
- O padrão Data Mapper mantém entidades de banco de dados e payloads de API fora da lógica de negócio.
- Testes ArchUnit no CI quebram o build em dependências proibidas, para que os revisores não precisem identificá-las manualmente.
- A extração de 2019 foi um trade-off tático deliberado em favor da velocidade de chegada ao mercado, e o custo de manutenção apareceu quando os merge requests quadruplicaram.
- Os resultados de prompts de IA são apenas direcionais, porque o período de observação foi curto.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Aplicação é o principal (componentes, camadas, interfaces). Dados aparecem apenas como modelos de repositório e acesso a dados encapsulado; nenhuma arquitetura de dados é discutida. Negócio aparece apenas como os domínios que organizam o código; nenhum trabalho de arquitetura de negócio é descrito. Tecnologia não é coberta além do pipeline de CI que executa os testes. |
| Escopo | Solução: uma API (BAPI) e seus módulos de domínio, dentro de um único serviço. |
| Stakeholders e preocupações | Desenvolvedores e revisores, que se importam com o tempo de revisão, com encontrar onde os objetos são construídos e com dependências não autorizadas. O negócio, que precisava de velocidade de chegada ao mercado em 2019 e ainda depende dos fluxos de cancelamento, reembolso e comunicação da BAPI. |
| Frameworks e notações | Nenhuma notação de modelagem é mencionada (sem TOGAF, ArchiMate, BPMN ou C4). Ferramentas e padrões citados: ArchUnit, padrão Data Mapper, architectural fitness functions, Spring Modulith. |

### Decisões e trade-offs
- **Design tático versus estratégico (2019):** a equipe escolheu uma extração tática, linha a linha, para priorizar a velocidade de chegada ao mercado. O custo foi a crescente pressão de manutenção, com os merge requests mais que quadruplicando e arquivos com mais de 4.000 linhas.
- **Mudança incremental versus reescrita:** a equipe escolheu limites incrementais em vez de uma reescrita completa. O custo é que estruturas antigas e novas coexistem enquanto a migração avança.
- **Organização por componente versus por camada:** a equipe escolheu componentes por domínio de negócio. O objetivo é localizar as mudanças, ao custo de uma nova organização a aprender.
- **Data Mapper para modelos de repositório:** a equipe mantém modelos de persistência e de payload fora da lógica de negócio. O artigo não discute o custo da camada extra de mapeamento.
- **Testes automatizados de limites versus revisão manual:** a equipe escolheu testes ArchUnit que quebram o build. O artigo apresenta isso como uma forma de detectar desvios mais cedo, mas não descreve o custo de manter as regras.
- **Platform como padrão para novos módulos:** o trabalho novo começa no framework, e o código antigo só é movido quando o trabalho de funcionalidade o toca.

### Fundamentos em que se apoia
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md) — o artigo a usa como uma estrutura de componentes que agrupam capacidades e gerenciam seus dados, o que corresponde à definição.
- [Capacidade de negócio](/pt-br/foundations/business-capability.md) — o artigo organiza o código por domínios de negócio, mas esses são limites de código, não um mapa de capacidades do negócio.
- [Arquitetura](/pt-br/foundations/architecture.md) — os limites e as restrições automatizadas são os princípios de design a que a definição se refere, aplicados ao longo da evolução do sistema.
- [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md) — a API pós-reserva é uma operação discreta, restrita a um único serviço, o que corresponde a esta definição.

## 🏛️ Para o Kenji
Este é um caso útil para um arquiteto que herdou um serviço emaranhado e não pode reescrevê-lo. A ideia mais transferível é que um limite só se torna real quando o build falha em uma violação, o que também é um mecanismo de governança. O vínculo com o MBA é o trade-off entre estratégia e tática na decisão de 2019. Esta é a sua interpretação, não a do autor. Pergunta para você: qual decisão na sua própria plataforma é tática hoje, e qual verificação automatizada mostraria quando ela começa a custar tempo de revisão?

## 📚 Plano de leitura
### A partir do artigo
- [InfoQ: Agoda Reduces API Code Review Time by 64% with Modular Architecture](https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework) — a notícia completa, com os mesmos números em contexto.
### Para se aprofundar
- Blog post da própria Agoda sobre a Platform — a fonte do diagrama e da descrição detalhada do framework; o artigo a cita, mas nenhum link é fornecido no texto extraído.
- John K. Ousterhout, *A Philosophy of Software Design* — a origem da distinção entre design estratégico e tático.
- Neal Ford e Mark Richards, *Software Architecture: The Hard Parts* — a fonte da ideia de architectural fitness function.
- ArchUnit — a biblioteca para codificar regras de arquitetura como testes.
- Spring Modulith — verificação de módulos e checagens de ciclos de dependência para Spring Boot.
- Padrão Data Mapper — o padrão de persistência usado para manter modelos de armazenamento fora da lógica de negócio.

## 🔗 Conceitos
- [bounded-context](/pt-br/concepts/architecture/bounded-context.md)
- [Modular monolith](/pt-br/concepts/architecture/modular-monolith.md)
