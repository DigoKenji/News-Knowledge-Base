---
type: Article Summary
title: 'O que desenvolvedores e QA precisam um do outro quando a IA escreve o código'
description: A IA acelera a mudança de código, então a prova vira o gargalo, e a passagem entre Desenvolvimento e QA precisa carregar um contrato de evidências
  compartilhado.
resource: https://www.cio.com/article/4251514/what-developers-and-qa-need-from-each-other-when-ai-writes-the-code.html
tags:
- software-engineering
- verification-tax
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4251514/what-developers-and-qa-need-from-each-other-when-ai-writes-the-code.html
  title: What developers and QA need from each other when AI writes the code
  author: CIO.com
status: stable
source_url: https://www.cio.com/article/4251514/what-developers-and-qa-need-from-each-other-when-ai-writes-the-code.html
source: CIO.com
source_type: blog
published: '2026-11-20'
domain: software-engineering
concepts:
- verification-tax
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81ec8788c10b9dd22f33
lang: pt-br
translation_of: /notes/software-engineering/2026-11-21-what-developers-and-qa-need-from-each-other-when-ai-writes-t.md
---

## 🎯 Tese central
A IA torna as mudanças de código baratas o suficiente para que a prova se torne o gargalo, então as equipes precisam desenhar a passagem entre Desenvolvimento e QA em torno de um contrato compartilhado de evidências antes de acelerar a entrega.

## 🧭 Por que o autor escreveu
O artigo parte de uma pequena mudança, que passou sem problemas, em um componente compartilhado e que quebrou o acesso de um cliente pagante; só um alerta do sistema em produção captou o problema. Se a tese estiver certa, as equipes devem avaliar a entrega assistida por IA pela pergunta de se cada release está comprovada a partir de fora da mudança, e não pela velocidade de produção do código nem pelo fato de o build estar verde.

## A mudança que passou em todas as verificações
Uma pequena edição em um componente compartilhado passou na revisão de código e no QA. Os testes estavam verdes e o código fazia o que o ticket pedia. Normalmente, quando um cliente comprava uma assinatura e fechava o navegador, o sistema transformava esse pedido em acesso funcionando em segundo plano. A mudança quebrou essa etapa, e um alerta dos logs do sistema em produção captou a falha que o QA e os revisores não viram. O diagnóstico do autor é que os testes não foram o que falhou. A falha estava na passagem: ninguém tinha documentado quais outros serviços usavam o componente, nem a ordem em que eles deveriam ser alterados. O trabalho de desenvolvimento e o de QA se encontram nessa passagem, e a IA a sobrecarrega ao multiplicar pequenas edições em código compartilhado mais rápido do que alguém consegue documentar suas dependências.

### Build verde
Um build verde mostra apenas que o código passou nos próprios testes. O alerta do artigo é que esses testes geralmente foram escritos a partir da mesma leitura do ticket que o código, então concordam com ele por construção. Uma release que possa ser defendida para o negócio precisa, portanto, de prova vinda de fora da mudança, e a passagem decide se essa prova chega.

### Estabilidade de entrega
Estabilidade de entrega é a frequência com que as mudanças falham em produção ou precisam de correções não planejadas. O estudo DORA de 2025, com quase 5.000 profissionais de tecnologia, constatou que as equipes que adotaram IA entregaram mais, enquanto essa estabilidade piorou. A constatação importa porque mostra que mais produção, por si só, não indica entrega saudável.

> 🔎 Contexto adicional: DORA é o programa DevOps Research and Assessment, e a taxa de falha de mudanças, que o artigo retoma depois, é uma das métricas padrão de entrega do programa.

## Desenhe a passagem antes de começar a entrega
Na maioria das equipes de entrega com as quais o autor trabalha, a passagem de Desenvolvimento para QA é um ticket mudando de status e pouco mais. A correção proposta é um acordo entre Arquitetura, Desenvolvimento, QA e Produto sobre o contexto e as evidências mínimas que toda mudança deve carregar. Esse acordo se torna o contrato de passagem e aprovação.

### Contrato de passagem e aprovação
O contrato define o que a mudança deve fazer, em termos de aprovado ou reprovado. Ele nomeia os componentes afetados, os serviços dependentes e as interfaces ou contratos de dados alterados. Lista os itens em aberto, como perguntas não resolvidas e trabalho de agentes ainda não concluído, e define as evidências exigidas em cada portão. Também declara quem pode aceitar o risco residual. O objetivo é que cada etapa receba o que, de outro modo, teria de reconstruir.

### Revisor de requisitos com IA
Durante a revisão, um revisor de requisitos com IA lê o ticket e verifica se o código cumpre o que ele promete. O QA faz outra pergunta: se o que o ticket pediu é seguro para liberar. Isso inclui verificar se as especificações estavam certas e se o sistema em torno da mudança continua funcionando. Como as duas perguntas são diferentes, a aprovação na primeira não responde à segunda.

### Sinal de release recíproco
A comunicação vai nos dois sentidos. O QA dá ao Desenvolvimento um sinal de release que separa defeitos do produto de ruído dos testes e do ambiente. O QA também aponta evidências faltantes e diz quais partes da passagem ajudaram. Esse retorno alimenta o contrato, a estratégia de testes e as regras de automação para a próxima mudança.

## O que o QA precisa do ticket e da mudança
O QA precisa saber o que "correto" significa antes de olhar o código. Essa definição vem do ticket, como escopo aprovado e critérios de aceitação que possam ser marcados individualmente como aprovados ou reprovados.

### Testes a partir dos requisitos
Os testes que acompanham uma mudança geralmente se apoiam na mesma leitura dos requisitos que o código, então confirmam a história do próprio código. O exemplo do artigo é um ticket que diz que uma assinatura é cancelada ao fim do período pago. Se o QA escrever testes a partir dessa afirmação, uma implementação que cancela imediatamente falha neles. Se, em vez disso, o QA escrever testes a partir do código pronto, o mesmo cancelamento imediato passa, e os testes o certificam como correto. Por isso, testar a partir dos requisitos precisa vir primeiro.

> ❌ **Antipadrão — Escrever testes a partir do código pronto:** os testes descrevem o que o código já faz. **Por que as pessoas fazem isso:** 🔎 a implementação é o artefato mais visível quando os testes começam; o artigo não dá motivo. **Consequência:** os testes passam e confirmam um comportamento errado como correto. **Correção:** escrever os testes a partir dos critérios de aceitação do ticket antes de ler a implementação.

### Dois mapas: solicitado e tocado
O QA compara dois mapas. Um mostra o que o ticket pediu. O outro mostra o que a passagem diz que foi tocado. Mesmo quando o ticket não menciona, um serviço compartilhado redeployado pertence ao segundo mapa. Se serviços dependentes não foram atualizados, ou se trabalho extra não foi declarado, a mudança está incompleta. Serviços que apenas pegam carona ampliam o plano de testes. Ler o código ainda importa depois, para diagnóstico, como estabilizar um teste que continua falhando sem que exista um defeito de produto por trás.

## Ajuste a garantia e as evidências à mudança
Três fatores tornam uma mudança mais difícil de comprovar que outra: se o código é novo ou antigo, quanto da funcionalidade foi gerado pela IA e quanta liberdade as ferramentas receberam. Tarefas reais envolvem uma mistura dos três.

### Eixos de dificuldade
O lado fácil é uma pequena funcionalidade nova em que o engenheiro ainda controla as ferramentas e as verificações de qualidade já estão no lugar. O lado difícil é uma mudança grande em um sistema antigo, construído com muito pouca supervisão. Ali o contexto precisa ser reconstruído, então a mesma mudança custa mais para ser comprovada. A prova pode exigir testes de contrato, verificações de integração, evidências de segurança ou desempenho e aprovação humana explícita.

### Raio de impacto (blast radius)
Quanta prova uma mudança exige depende do impacto no negócio e do raio de impacto, isto é, de quanto mais pode ser derrubado pela mudança. A pequena edição ilustra isso, porque quebrou a etapa que dá acesso funcionando a um cliente pagante.

### Pontos de checagem de evidências
As equipes coletam provas em pontos diferentes, como testes concluídos, revisões finalizadas e requisitos verificados. Quanto mais a mudança se aproxima da release, mais alta é a barra. A passagem carrega o que a etapa seguinte, de outro modo, teria de reconstruir.

### Quatro estados de resultado
Cada teste é registrado como aprovado ou reprovado. Um teste que foi iniciado mas não pôde ser concluído é marcado como bloqueado. Um teste que nunca foi executado fica vazio. Bloqueado e vazio mostram onde a prova está faltando, para que um build verde não finja que todos os testes passaram.

## Meça a confiança por unidade de tempo
Confiança por unidade de tempo é a rapidez com que uma equipe transforma uma mudança em uma decisão de release respaldada por prova. O artigo a define como a completude das evidências dividida pelo lead time de validação, contada somente depois que a completude ultrapassa o limite definido para aquela classe de risco. A regra do artigo é que validar mais rápido não é uma melhoria se a completude cair.

### Completude das evidências
Completude é a parcela dos itens de evidência exigidos que foram registrados como aprovados ou reprovados para aquela classe de risco. Itens bloqueados e vazios não contam. A própria mudança do artigo mostra o efeito: o build estava verde, mas as verificações dos serviços dependentes estavam vazias. Isso significava baixa completude e uma release inacabada. O autor observa que verificações vazias são comuns.

> ❌ **Antipadrão — Tratar um build verde como prova completa:** o status do build parece concluído enquanto verificações exigidas ainda estão vazias. **Por que as pessoas fazem isso:** 🔎 um único status verde é o sinal mais visível; o artigo não dá motivo. **Consequência:** uma release é aprovada com base em evidências parciais. **Correção:** contar apenas os itens aprovados ou reprovados em relação ao conjunto exigido para a classe de risco.

### Lead time de validação
O lead time de validação vai do momento em que uma mudança está pronta para revisão até a decisão de release registrada. Ele é um segmento do *lead time for changes* do DORA.

### Sete sinais e três comparações
O artigo diz que sete sinais bastam para começar. A taxa de falha de mudanças permanece como está. Os outros seis são lidos como taxas ou durações:

- Taxa de defeitos escapados: defeitos de produção como parcela de todos os defeitos encontrados.
- Parcela de escapes críticos: a parcela desses escapes classificada como crítica.
- Lead time de validação, como definido acima.
- Taxa de falhas que não são do produto: falhas de teste ou de ambiente como parcela de todas as falhas.
- Retrabalho em código gerado dentro de uma janela fixa, como duas semanas após a aceitação.
- Completude das evidências, a única medida sem um equivalente no conjunto padrão.

Acrescente dois dados de contexto: a velocidade do build e com que frequência uma pessoa precisa intervir. Depois compare a velocidade do build com o lead time de validação, os defeitos escapados com a gravidade deles e a taxa de falha de mudanças, e o retrabalho com as falhas que não são do produto e com a intervenção humana.

O artigo alerta que contagens brutas enganam. Se a produtividade dobra e os defeitos escapados aumentam 50%, a qualidade melhorou, mesmo que uma contagem bruta aponte uma regressão. 🔎 Por mudança, isso equivale a 0.75 da taxa de escape anterior.

> ❌ **Antipadrão — Ler contagens brutas como qualidade:** um número crescente de defeitos é tomado como regressão sem considerar o volume. **Por que as pessoas fazem isso:** 🔎 contagens são os números mais simples de reportar; o artigo não dá motivo. **Consequência:** uma equipe que está melhorando parece estar piorando. **Correção:** normalizar pelo volume e acompanhar taxas.

## Espere uma queda no início.
Segundo o artigo, a adoção inicial não é uma linha reta para cima.

### Curva J
A pesquisa DORA sobre o ROI do desenvolvimento de software assistido por IA descreve o caminho inicial como uma curva J. As equipes veem uma queda temporária de produtividade e um período de instabilidade enquanto se ajustam. 🔎 Contexto adicional: uma curva J é um padrão de desempenho que cai antes de subir, então os resultados iniciais podem parecer piores do que a linha de base.

### Imposto de verificação
A queda vem do que a pesquisa chama de imposto de verificação: o tempo extra gasto revisando código gerado conforme o volume aumenta. O imposto cresce com o volume, então a capacidade de checagem precisa acompanhar a capacidade de geração. O teste do artigo para isso é se a checagem acompanha a geração.

## Comece com um caminho de entrega
A pesquisa State of AI 2026 da McKinsey constatou que quase três quartos dos de alto desempenho em IA tinham redesenhado fundamentalmente seus fluxos de trabalho, em comparação com cerca de um quarto dos demais respondentes. O artigo observa que o redesenho não precisa começar em todo lugar. Um caminho de entrega real basta.

### Um caminho de entrega real
Antes que a próxima mudança gerada por IA chegue a esse caminho, as quatro funções precisam ter o contrato de passagem e aprovação em vigor. Elas devem medir a passagem contra a própria linha de base e depois observar se a checagem acompanha a geração.

### Liberdade de ferramenta conquistada
Ferramentas de IA só podem receber mais liberdade quando a confiança por unidade de tempo melhora e a estabilidade da entrega se mantém. A liberdade é, portanto, conquistada a partir de prova medida, não concedida no início.

### Avaliação de papéis
O QA está deixando de repetir as verificações óbvias para projetar a prova. O Desenvolvimento está produzindo mudanças que a etapa seguinte consegue confirmar sozinha. Nenhum dos dois papéis deve ser avaliado pelo número de bugs encontrados. O artigo termina com o ponto de que, se você acelerar sua codificação sem acelerar sua prova, a IA só vai ajudar sua equipe a chegar mais rápido ao próximo gargalo.

## 🧠 Modelos mentais
### Dois mapas
**O modelo:** o ticket define o escopo solicitado, e a passagem define o escopo tocado. Uma mudança é verificada contra os dois.
**Como aplicar:** antes de o QA executar qualquer coisa, liste lado a lado o escopo solicitado e o escopo tocado. Qualquer serviço tocado sem teste é uma lacuna.

### O raio de impacto define a barra de prova
**O modelo:** a prova que uma mudança exige cresce com seu impacto no negócio e com seu raio de impacto, não com o fato de o código ser novo.
**Como aplicar:** avalie cada mudança pelo que mais ela pode derrubar, e então defina os itens de evidência exigidos e o nível de aprovação a partir dessa avaliação.

### A prova precisa acompanhar a geração
**O modelo:** gerar mais rápido só ajuda se a checagem acompanhar. Caso contrário, o gargalo passa para a prova.
**Como aplicar:** acompanhe juntas a confiança por unidade de tempo e a completude das evidências. Não aceite um ganho de lead time que reduza a completude.

## 🔑 Principais conclusões
- Um build verde mostra que o código passou em testes escritos a partir da mesma leitura do ticket. Isso não mostra que a mudança é segura para liberar.
- A passagem entre Desenvolvimento e QA deve ser um contrato escrito que nomeie critérios de aprovado ou reprovado, componentes afetados, serviços dependentes, itens em aberto e as evidências exigidas por cada portão.
- O QA deve escrever os testes a partir dos critérios de aceitação antes de ler a implementação, e depois comparar o escopo solicitado com o escopo tocado.
- Resultados bloqueados e vazios marcam onde a prova está faltando, e não contam para a completude das evidências.
- Meça a confiança por unidade de tempo como completude das evidências dividida pelo lead time de validação, e somente depois que a completude ultrapassar o limite da classe de risco.
- Espere uma curva J: uma queda temporária de produtividade enquanto o trabalho de verificação cresce com o volume de código gerado.
- Dê às ferramentas de IA mais liberdade só quando a confiança por unidade de tempo melhorar e a estabilidade da entrega se mantiver.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um Staff Architect, o contrato de passagem e aprovação parece um artefato de arquitetura por si só, já que define quais dependências e interfaces uma mudança deve declarar antes de ir para produção. Isso está próximo do trabalho de governança que você já faz em governança de IA. A pergunta que ele levanta: que mapa de dependências e interfaces sua equipe precisaria ter antes que uma mudança em um componente compartilhado pudesse ir para produção com uma ordem de implantação documentada?

## 📚 Plano de leitura
### Do artigo
- Nenhum. O artigo não cita links, então nenhum é listado aqui.

### Para aprofundar
- Estudo DORA de 2025 sobre adoção de IA e estabilidade de entrega (citado sem título no artigo): a fonte do dado de quase 5.000 profissionais.
- Pesquisa Tricentis 2026 Quality Transformation: a fonte do índice de 60% de itens não testados que chegam à produção.
- Pesquisa DORA sobre o ROI do desenvolvimento de software assistido por IA: a fonte da curva J e do imposto de verificação.
- Pesquisa State of AI 2026 da McKinsey: a fonte da comparação de redesenho de fluxos de trabalho.

## Conceitos
- [verification-tax](/pt-br/concepts/software-engineering/verification-tax.md)
