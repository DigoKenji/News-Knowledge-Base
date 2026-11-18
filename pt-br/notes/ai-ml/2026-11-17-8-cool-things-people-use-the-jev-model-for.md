---
type: Article Summary
title: 8 coisas legais para as quais as pessoas usam o modelo Jev
description: Oito projetos públicos mostram as decisões estruturadas, rápidas e baratas do Jev alimentando revisão de código, uso de computador, classificação, jogos e pesquisa.
resource: https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
  title: 8 Cool Things People Use the Jev Model For
  author: Jim Clyde Monge
status: stable
source_url: https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
source: Generative AI
source_type: medium
published: '2026-09-20'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81dea956e1a305308ad7
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-8-cool-things-people-use-the-jev-model-for.md
---

# 8 coisas legais para as quais as pessoas usam o modelo Jev

**Fonte:** https://medium.com/generative-ai/8-cool-things-people-use-the-jev-model-for-dd0ed8e785d4
**Autor:** Jim Clyde Monge
**Data:** 2026-09-20
**Conceitos:** [Throughput/latência/custo de inferência](/pt-br/concepts/inference-tradeoffs.md)

---

## 🎯 Tese central
O Jev é um modelo rápido e barato que responde a perguntas estruturadas com decisões e probabilidades que o software pode usar diretamente. Oito projetos reais mostram o modelo funcionando em revisão de código, uso de computador, classificação, jogos, gestão de contexto de agentes, trading e pesquisa.

## 🧭 Por que o autor escreveu
O Jev estava se espalhando rapidamente pelas redes sociais, e o autor queria ir além do hype e ver o que as pessoas estão realmente construindo. Ele reúne oito exemplos, com os custos e tempos informados por seus criadores. Se a leitura dele estiver certa, a pergunta útil não é se um modelo rápido e barato é impressionante, mas quais decisões do dia a dia em software agora podem se dar ao luxo de uma chamada a um modelo.

## Introdução: o que é o Jev
O autor abre dizendo que o Jev é "incrivelmente barato e ridiculamente rápido", e o explica em uma frase: o Jev recebe informações e responde a perguntas estruturadas sobre elas, devolvendo decisões e probabilidades que o software pode usar diretamente. O artigo atribui a velocidade à TypeSafe, que afirma que as probabilidades são produzidas em paralelo. Não há um parágrafo de texto a ser gerado token por token.

### Decisões estruturadas e probabilidades
Em vez de escrever prosa, o modelo devolve uma resposta fixa para uma pergunta fixa, como "isto é um problema de segurança?" ou "qual destas categorias se encaixa?", junto com uma probabilidade para cada resposta. O software pode ramificar a lógica diretamente com base nessa saída, sem precisar interpretar texto livre. Isso é o núcleo de todos os exemplos que seguem.

> 🔎 Contexto adicionado: um token é um pequeno pedaço de texto, muitas vezes uma palavra ou parte de uma palavra, que um modelo de linguagem gera passo a passo. A saída de probabilidades em paralelo evita essa geração passo a passo para a resposta.

### Saída de probabilidades em paralelo
A alegação de velocidade do artigo se apoia na descrição da TypeSafe sobre a saída em paralelo. Como a resposta é um conjunto de probabilidades, e não uma frase, todas podem ser produzidas ao mesmo tempo. O autor trata isso como a razão dos tempos de resposta sub-segundo informados nos exemplos seguintes, embora ele mesmo não tenha medido isso.

## 1. Revisando pull requests a um custo quase zero
Paolo Rosson, um usuário do X, construiu um revisor de código que envia o diff de um pull request ao Jev e recebe 14 verificações em uma única chamada. As verificações cobrem segredos expostos, injeção de SQL, mudanças de autenticação, testes apagados e se a descrição corresponde ao código. O custo informado é de US$ 0,00007 por revisão, com respostas em cerca de meio segundo. Nesse ritmo, 1.000 revisões custam sete centavos, contra cerca de US$ 14,50 na configuração com o Opus 5 do criador.

### Revisão automatizada de pull requests com probabilidades por verificação
Cada verificação é uma pergunta separada, com sua própria probabilidade, e não um veredito geral. O código ao redor transforma esses números em ações: bloquear a mudança, pedir uma revisão de segurança, sinalizar problemas menores ou recomendar o merge. Dividir a revisão em muitas verificações estreitas torna cada decisão mais fácil de auditar do que uma única nota de "este código é bom".

### Escalonamento por faixa de confiança
O autor admira a forma como o sistema lida com a incerteza. Quando uma verificação crítica cai entre 0,35 e 0,65, o sistema a escalona para uma pessoa ou para um modelo maior, de modo que o modelo barato só decide os casos claros. A faixa é ajustável, então o revisor pode ser tornado mais cauteloso ao ampliá-la.

> 🔎 Contexto adicionado: um pull request é um conjunto proposto de mudanças no código, enviado para revisão antes de ser integrado à base principal de código.

O autor o usaria como uma primeira passagem nos seus próprios projetos de sites. Uma pequena mudança de estilo poderia receber uma revisão mais leve, enquanto qualquer coisa que tocasse em autenticação ou no banco de dados receberia mais atenção.

## 2. O Jev é muito bom em uso de computador
Shiv compartilhou uma ferramenta de uso de computador construída sobre o Jev. Ela responde em menos de um segundo e custa quase nada por interação. No fluxo demonstrado, ela também decide o que digitar, então não é necessário um modelo de linguagem separado. O projeto é o Third Hand no GitHub, e o artigo aponta para um segundo exemplo de uso de computador, do Instant Rice Cook, para comparação.

### Laço de agente de uso de computador
Um agente de uso de computador observa a tela atual, escolhe uma ação, como abrir um menu, selecionar um campo ou digitar um texto, executa essa ação e então verifica o resultado antes de continuar. O ponto do artigo é que a escolha do modelo é apenas uma parte do laço. O software ainda precisa identificar os controles disponíveis, executar a ação escolhida e verificar o que aconteceu depois.

### A latência por ação se acumula
O autor observa que um agente que percorre menus e várias telas executa muitas ações em sequência. Um pequeno atraso em cada ação se soma ao longo da tarefa inteira, e é por isso que a resposta sub-segundo importa mais aqui do que em uma chamada isolada.

> ❌ **Antipadrão — Ignorar a etapa de verificação (implícito):** tratar a ação escolhida pelo modelo como concluída, sem conferir a tela depois. **Por que as pessoas fazem isso:** a ação em si parece a tarefa inteira, e a verificação parece um encanamento extra. **Consequência:** uma ação errada ou com falha passa despercebida e a sequência continua a partir de um estado ruim. **Correção:** confirmar cada passo contra o que deveria ter mudado na tela antes de seguir em frente.

O primeiro experimento do autor seria uma tarefa repetitiva de painel administrativo no blog técnico dele, a Zeniteq: abrir um artigo, escolher uma categoria, mudar uma configuração e voltar à lista. Como ele já sabe o que cada ação deve realizar, os erros seriam fáceis de identificar.

## 3. Encontrando links internos em um site inteiro
Outro usuário do X usou o Jev em um fluxo que leu 586 páginas de um site e reconstruiu o mapa de links internos em 45,1 segundos. A execução inseriu 584 links ao custo de US$ 0,21. Ela também se recusou a linkar 139 páginas, porque não encontrou uma correspondência adequada, e o autor diz que esse detalhe o interessou mais do que a contagem de páginas.

### Pontuação de relevância para links internos
O fluxo reúne páginas candidatas, pede ao Jev que pontue a relevância de cada uma e sugere links onde os leitores se beneficiariam de mais contexto. O autor quer que o texto âncora proposto e o destino sejam mostrados juntos, para que ele possa julgar se o link soa natural na frase. Uma boa sugestão leva o leitor de um guia sobre ferramentas de programação com IA a uma explicação detalhada do MCP, e não a um post sem relação que apenas menciona IA.

> 🔎 Contexto adicionado: uma auditoria de SEO é uma revisão da estrutura, do conteúdo e dos links de um site para encontrar problemas que afetam a forma como os buscadores encontram e ranqueiam as páginas.

### O que a execução faz e não prova
O autor enfatiza que a execução mostra um trabalho de linkagem interna, que é apenas uma parte de uma auditoria de SEO, e que ela não comprova nenhuma melhora de ranqueamento. O benefício imediato que ele vê é tornar artigos úteis mais fáceis de encontrar, gastando menos tempo procurando no próprio arquivo. Ele começaria com uma prévia das mudanças sugeridas na Zeniteq, revisaria uma amostra e daria atenção especial aos links adicionados a posts antigos, cujas informações talvez precisem ser atualizadas.

## 4. Organizando documentos fiscais a cerca de US$ 0,001 por página
Outro usuário construiu um classificador de documentos fiscais em torno do Jev e informou um custo de cerca de US$ 0,001 por página. A equipe descreve o sistema como 34 vezes mais barato e seis vezes mais rápido do que o pipeline anterior com LLM, com todos os documentos do corpus classificados corretamente. O classificador lê o texto das páginas e pede ao Jev que identifique o formulário a partir de descrições predefinidas de formulários, devolvendo uma categoria de formulário e um valor de confiança para a próxima etapa do software.

### Classificação com limiares de confiança e precisão estrita
Um classificador que devolve um rótulo mais um valor de confiança permite que o sistema trate páginas incertas de forma diferente das confiantes. O repositório open source informa 100% de precisão estrita em 261 formulários do IRS, a cerca de US$ 0,001 por página, e nenhum erro em 314 páginas de formulários preenchidos. Um conjunto separado de 753 páginas de formulários em branco não teve rótulos errados, mas 38 páginas ficaram abaixo do limiar de confiança e foram contadas como erros estritos. O autor valoriza essa distinção: uma página enviada para uma nova análise é um resultado diferente de uma página enviada com confiança para o lugar errado.

### Ordenação repetitiva como caso de uso alvo
O autor diz que este é o exemplo menos vistoso, mas um que uma equipe poderia usar mais no dia a dia do que uma demonstração de jogo. Ordenar milhares de páginas é repetitivo, e um erro de classificação pode desviar o restante de um processo. Ele aplicaria a mesma abordagem a faturas ou recibos, definindo categorias com cuidado, inspecionando os resultados incertos e medindo quanta ordenação manual ainda resta.

> ❌ **Antipadrão — Forçar um rótulo em cada página (implícito):** exigir uma categoria para cada entrada, mesmo quando o modelo não tem certeza. **Por que as pessoas fazem isso:** uma saída completa parece mais limpa e dispensa uma fila de revisão. **Consequência:** páginas de baixa confiança são classificadas no lugar errado, sem nenhum sinal para quem revisa. **Correção:** definir um limiar de confiança e encaminhar as páginas abaixo dele para uma pessoa, contando-as separadamente dos erros confiantes, como o repositório faz.

## 5. Jogando Subway Surfers em 50 partidas
Max Blade usou o Jev para jogar Subway Surfers em uma velocidade que ele descreve como sobre-humana, incluindo uma demonstração de 50 partidas rodando ao mesmo tempo. Ele diz que a execução apresentada custou menos de um centavo. O autor acha esse tipo de experimento útil porque jogos mostram rapidamente se uma sequência de decisões funciona: o personagem evita o obstáculo ou bate nele.

### Agente de jogo e espaço de ações limitado
Um jogo de corrida tem poucos movimentos legais, como trocar de faixa ou pular. Um controlador avalia a situação, escolhe uma ação e repete o processo à medida que o jogo avança. O autor faz a ressalva de que o desempenho do vídeo depende de toda a configuração, incluindo as informações de jogo utilizáveis e o software que executa cada movimento, e que o Jev não conseguiria simplesmente observar qualquer jogo e jogá-lo igualmente bem.

### Demonstração de direção e seus limites
JP Schroeder descreveu ter recriado o Tesla Full Self-Driving com o Jev em menos de uma hora. O autor considera essa descrição ambiciosa para um experimento e diz que precisaria de bem mais evidências antes de compará-la com um sistema que dirige em vias reais. Ele se manteria em jogos e ambientes simulados, onde construiria um pequeno jogo de navegador com personagens que reagem de forma diferente à mesma situação e observaria como mudar os critérios de decisão deles altera os resultados. Um personagem cauteloso poderia evitar obstáculos logo no início, enquanto outro pegaria rotas mais arriscadas, e o espaço de ações pequeno tornaria cada movimento ruim fácil de explicar.

## 6. Limpando o contexto de um agente com mais rapidez
Tamara Tran usou o Jev para compactação, que reduz o histórico de conversa que um agente carrega para o turno seguinte. O projeto dela pontua cada chamada de ferramenta e cada resultado, e então remove ou encurta o material considerado desnecessário. O conteúdo mantido conserva a redação original. O autor gosta da perspectiva de preservar uma mensagem de erro ou um comando exato, enquanto limpa saídas antigas que já cumpriram sua função.

### Compactação de contexto para agentes
Uma longa sessão de programação acumula material que não é mais necessário: saídas de testes que deixaram de ser relevantes após uma correção, ou um arquivo lido que foi superado por várias edições. Mantê-lo deixa a conversa mais longa sem ajudar a próxima decisão. A parte difícil é prever o que terá importância depois. Um detalhe menor de um resultado de ferramenta no início pode explicar uma falha muito mais adiante na tarefa, então a compactação precisa ser conservadora sobre o que remove.

O código é o fast-jev-compaction, um plugin do Claude Code que substitui o resumo de compactação por decisões do Jev. O artigo descreve o comportamento documentado: as mensagens recentes são protegidas, as chamadas de ferramenta permanecem pareadas com seus resultados, e o texto do usuário e do assistente fica inalterado na conversa resultante.

### Testando a compactação e medindo a pausa
O autor testaria a compactação voltando a um requisito anterior depois dela e verificando se o agente ainda tem o que precisa. Ele observa que "instantâneo" descreve a ambição da demonstração, e não uma garantia. O repositório permite que históricos maiores exijam várias requisições, e reabrir arquivos repetidamente consumiria o tempo economizado. Ele mediria a pausa em suas próprias sessões antes de julgar o quanto a experiência fica mais rápida.

> ❌ **Antipadrão — Julgar pela velocidade da demonstração (implícito):** tratar a velocidade anunciada como a experiência de uso real. **Por que as pessoas fazem isso:** demonstrações mostram o melhor cenário, com entradas pequenas. **Consequência:** históricos grandes que exigem várias requisições apagam a economia de tempo esperada. **Correção:** medir a latência em sessões representativas antes de adotar a ferramenta.

## 7. O Jev como robô de trading
Jarrod Watts construiu um robô que usa o Jev para decidir se compra ou vende com base no feed de preços de um par de ativos. Ele diz que o robô executa negociações reais pelo livro de ofertas da Kuru na Monad, visando o intervalo de 300 ms entre blocos da rede. O modelo fornece a decisão, e a aplicação ao redor envia a ordem. O autor considera isso uma boa demonstração de quão rapidamente a saída de um modelo pode alimentar outros softwares.

### Laço de decisão e orçamento de latência
O laço é: ler o feed de preços, pedir ao modelo uma compra ou venda, executar e repetir a cada bloco. O intervalo de 300 ms define o orçamento de tempo para o ciclo inteiro, e não apenas para a chamada ao modelo. A demonstração Jev Trader do artigo inclui um modo de simulação (dry run) e uma instrução permanente para enviar uma oferta de compra ou venda no livro MON/USDC a cada bloco, sem opção de abster-se.

### Por que uma decisão rápida diz pouco sobre lucro
O autor se interessa mais pela implementação do que por ganhar dinheiro. Uma compra ou venda rápida nos diz pouco sobre se ela é lucrativa. A avaliação exige resultados ao longo do tempo, incluindo custos de negociação e os resultados quando os preços se movem contra o robô. O experimento dele ficaria na simulação: registrar cada decisão, compará-la com uma estratégia fixa simples e procurar onde o modelo repetidamente faz escolhas ruins. Mesmo um resultado não lucrativo poderia ensinar algo sobre como construir um laço de decisão responsivo, mas ele gostaria de ver o gráfico de desempenho ao lado do gráfico de latência antes de se empolgar.

> ❌ **Antipadrão — Laço de decisão que sempre age (implícito):** exigir que o modelo escolha um lado a cada tick. **Por que as pessoas fazem isso:** mantém a demonstração ativa e o caminho de decisão simples. **Consequência:** sinais fracos são negociados mesmo assim, então o robô não pode ser comparado com uma versão que espera. **Correção:** permitir um resultado de abstenção e comparar os dois comportamentos em simulação.

## 8. Organizando mais de 1.000 artigos de pesquisa em IA
Hassan usou o Jev para classificar 1.018 artigos de pesquisa em IA. Ele informa um custo total de classificação de US$ 0,08 e uma latência mediana de ponta a ponta de 256 ms por artigo. O projeto finalizado permite que os leitores explorem a coleção por tema. O fluxo primeiro resume cada artigo com o DeepSeek V4 Flash e então envia cada título e resumo ao Jev, junto com 24 temas possíveis.

### Pipeline de resumir e depois classificar
Dividir o trabalho entre dois modelos torna o papel de cada um fácil de perceber. O DeepSeek cria os resumos, e o Jev atribui os temas usando os títulos e resumos. Os custos informados são de US$ 3,99 pelos resumos via Together AI e oito centavos pelas chamadas de classificação do Jev via TypeSafe, para um total de inferência informado de US$ 4,07.

### Taxonomia de temas para navegação
O autor chama este de seu exemplo favorito, porque usaria algo parecido para pesquisa de artigos. Navegar por um tema pode revelar um artigo que ele nunca pensaria em buscar. Para a própria versão, começaria com uma coleção menor sobre agentes de programação com IA, ajustaria as categorias durante a navegação e verificaria se elas ajudam a encontrar trabalhos relacionados. Ele diz que gastaria de bom grado alguns dólares em uma ferramenta de pesquisa construída em torno dos seus temas.

## Considerações finais
O autor chama o Jev do maior avanço de IA de 2026 até agora, o que é uma opinião dele, e não uma afirmação medida. Ele aponta revisões de código, controle de computador, ordenação de documentos e jogos como evidência de que muitas tarefas do dia a dia poderiam se beneficiar de decisões rápidas e baratas. A Vercel deixou o Jev gratuito até 25 de setembro, o que ele considera um forte motivo para experimentar. Ele encerra observando que ainda há muito a aprender sobre o quão confiavelmente esses projetos funcionam além das demonstrações, e diz que o exemplo do artigo de pesquisa é o mais promissor, porque combina o Jev com outro modelo.

## 🧠 Modelos mentais
### Probabilidades como sinais de roteamento
**O modelo:** um modelo que devolve um rótulo e uma probabilidade permite que o software decida o que acontece em seguida, em vez de o modelo decidir tudo.
**Como aplicar:** defina o que cada faixa de probabilidade dispara, como ação automática, revisão humana ou um modelo maior.

### Escalone o meio incerto
**O modelo:** decida os casos claros de forma barata e envie a faixa ambígua para um caminho mais cuidadoso.
**Como aplicar:** escolha limiares, como a faixa de 0,35 a 0,65 do revisor, e torne-os ajustáveis para que a equipe possa apertá-los com o tempo.

### Separe a decisão da ação
**O modelo:** o modelo escolhe; o código ao redor executa e verifica.
**Como aplicar:** em qualquer agente ou robô, escreva o executor e a etapa de verificação após a ação com o mesmo cuidado que o prompt.

### A latência por etapa se acumula
**O modelo:** um pequeno atraso repetido em muitas ações sequenciais se torna o custo dominante de toda a tarefa.
**Como aplicar:** meça o tempo de cada etapa em uma execução representativa e estime o total antes de escolher um modelo para cada etapa.

## 🔑 Principais conclusões
- O Jev devolve decisões estruturadas e probabilidades, e o artigo atribui a sua velocidade à saída de probabilidades em paralelo.
- As 14 verificações separadas do revisor, cada uma com uma probabilidade, permitem que o código ao redor bloqueie, escalone ou aprove mudanças.
- Uma verificação crítica entre 0,35 e 0,65 vai para uma pessoa ou para um modelo maior no desenho do revisor.
- Os custos informados são muito baixos, como US$ 0,00007 por revisão de PR e cerca de US$ 0,001 por página de formulário fiscal, mas são números dos próprios criadores.
- As alegações de precisão de classificação dependem de como as páginas incertas são contadas: 38 páginas em branco de baixa confiança foram pontuadas como erros estritos.
- A compactação que mantém as chamadas de ferramenta pareadas com seus resultados e deixa o texto do usuário e do assistente inalterado é uma restrição de design concreta para agentes de programação.
- Velocidade e custo em demonstrações não comprovam lucratividade, ganhos de ranqueamento nem comportamento em produção, e o autor diz isso em vários exemplos.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. O padrão mais transferível é a divisão do revisor entre um modelo barato que decide os casos claros e uma faixa de escalonamento para os casos incertos, que se mapeia para controles de governança sobre decisões de IA. Onde isso se aplicaria no trabalho de um arquiteto é em qualquer caminho de revisão ou classificação de alto volume em que o custo de uma decisão automática errada seja alto. Uma pergunta que isso levanta: quem define e audita os limiares de escalonamento, e como você demonstraria que uma faixa de 0,35 a 0,65 se sustenta ao longo do tempo?

## 📚 Plano de leitura
### Do artigo
- [generativeai.pub](http://generativeai.pub) — o artigo anterior do autor, "New Jev Model is Insane! 200x Faster & 400x Cheaper Than Frontier Models", que explica como o Jev funciona.
- [github.com/shhivv/third-hand](http://github.com/shhivv/third-hand) — o projeto de uso de computador Third Hand, para ver como o estado da tela se transforma em escolhas do modelo.
- [github.com/kyotofin/tax-doc-classifier](http://github.com/kyotofin/tax-doc-classifier) — o classificador de formulários fiscais e sua avaliação publicada, incluindo as 38 páginas em branco de baixa confiança.
- [github.com/tamaratran/fast-jev-compaction](http://github.com/tamaratran/fast-jev-compaction) — o plugin de compactação do Claude Code e seu comportamento documentado.
### Para ir além
- Limiares de confiança e predição seletiva — como decidir quando a resposta de um modelo é boa o bastante para agir sobre ela.
- Compactação de contexto para agentes de programação — o que manter e o que descartar em sessões longas com uso de ferramentas.
- Pipelines de resumir e depois classificar — quando dividir o trabalho entre dois modelos ajuda em custo e clareza.
