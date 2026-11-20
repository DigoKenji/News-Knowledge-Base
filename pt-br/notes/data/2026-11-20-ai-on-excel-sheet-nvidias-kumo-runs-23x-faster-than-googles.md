---
type: Article Summary
title: 'IA em planilha: o Kumo da NVIDIA roda 23x mais rápido que o TabFM do Google'
description: O Kumo da NVIDIA faz previsões a partir de tabelas sem nenhuma etapa de treinamento, e um benchmark independente confirma sua velocidade, mas seus principais concorrentes têm restrições de uso comercial.
resource: https://medium.com/towards-deep-learning/ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-google-s-tabfm-21c0d20518ee
tags:
- data
- data-drift
- concept-drift
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/towards-deep-learning/ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-google-s-tabfm-21c0d20518ee
  title: 'AI on Excel Sheet: NVIDIA''s Kumo Runs 23x Faster Than Google''s TabFM'
  author: Towards Deep Learning
status: stable
source_url: https://medium.com/towards-deep-learning/ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-google-s-tabfm-21c0d20518ee
source: Medium (Towards Deep Learning)
source_type: medium
published: '2026-11-08'
domain: data
concepts:
- data-drift
- concept-drift
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d813d8dfbdcb9a894ec05
lang: pt-br
translation_of: /notes/data/2026-11-20-ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-googles.md
---

## 🎯 Tese central
O Kumo Tabular da NVIDIA consegue prever a partir de uma planilha sem nenhuma etapa de treinamento, é bem menor e mais rápido que os modelos rivais em um benchmark de 19 conjuntos de dados, e é o modelo mais preciso disponível que uma empresa pode usar livremente. Porém, o post no LinkedIn que o divulgou errou um número-chave e deixou de fora as limitações de licença dos dois modelos que o superam.

## 🧭 Por que o autor escreveu
O autor viu uma onda de posts "RIP XGBoost" no LinkedIn afirmando que um novo modelo pequeno substitui o fluxo habitual de treinar um modelo para cada pergunta. Ele rastreou os números do post até suas fontes para verificar se eles se sustentam, e pergunta se o modelo é realmente melhor, o que mais existe e se uma empresa pode usá-lo sem que um advogado bloqueie o trabalho.

## Abertura: a afirmação e as três perguntas
O artigo abre com uma dor conhecida. Por cerca de vinte anos, as árvores de gradiente (o XGBoost é a mais conhecida) têm sido o padrão para prever resultados a partir de tabelas, como cancelamento de clientes, inadimplência em empréstimos ou preços de imóveis. O problema é que cada nova pergunta exige um modelo novo, treinado do zero, o que pode consumir dias de limpeza de dados, construção de variáveis e ajuste de parâmetros.

O post no LinkedIn fazia três afirmações sobre o Kumo, o modelo de tabelas da NVIDIA. Ele tem de 28 a 215 milhões de parâmetros, contra 1,65 bilhão do TabFM, do Google. Em um teste público com 19 conjuntos de dados reais, rodou cerca de 23 vezes mais rápido que o TabFM e terminou entre os três primeiros em 16 deles. O tempo de GPU do teste inteiro custou US$ 1,19 no Kumo e US$ 27,40 no TabFM.

As três perguntas do autor organizam o restante da nota: ele é realmente melhor, o que mais está disponível e pode ser usado no trabalho.

> 🔎 Contexto adicionado: "Entre os três primeiros" significa que a média de posição do modelo nos conjuntos de dados o colocou entre os três melhores modelos testados, sendo a posição de cada modelo em um conjunto a sua colocação na pontuação de precisão daquele conjunto.

## Afinal, o que é o Kumo, em termos simples?
O Kumo Tabular funciona como ensinar um novo colega por exemplo. Você mostra a ele cinquenta registros antigos de clientes, marcados como cancelados ou que permaneceram, depois entrega um registro novo e pede um julgamento. O Kumo recebe uma tabela em que algumas linhas já têm a resposta e outras estão em branco, e preenche os espaços em branco de uma só vez. Não há etapa de treinamento, ajuste de parâmetros nem engenharia de variáveis.

O termo da área para isso é aprendizado em contexto (in-context learning). É o mesmo truque que permite a um modelo de chat pegar uma tarefa a partir de exemplos escritos no prompt. Aqui, as linhas com respostas funcionam como esses exemplos. O contraste é entre contratar e treinar um novo especialista para cada pergunta (a abordagem tradicional) e usar um generalista que já estudou milhões de tabelas e só precisa olhar a sua.

O Kumo vem em três tamanhos. O Small tem cerca de 28 milhões de parâmetros, o Medium cerca de 62 milhões e o Large cerca de 215 milhões. O autor observa que isso é minúsculo pelos padrões atuais e pequeno o bastante para rodar em uma única placa de vídeo, o que importa para o custo. Até o Kumo Large tem cerca de metade do tamanho do TabFM.

A NVIDIA lançou o Kumo Tabular em 29 de setembro. O nome vem da Kumo AI, uma startup cofundada pelo professor de Stanford Jure Leskovec. Seus modelos de previsão foram usados por empresas como Reddit e Sainsbury's, e a NVIDIA comprou a empresa em junho por pelo menos US$ 400 milhões, segundo o The Information. A lista de autores inclui Jingang Qu, autor principal do TabICL, o que explica parte do desenho.

O modelo nunca viu uma planilha real durante o treinamento. A NVIDIA escreveu um gerador que inventa pequenos mundos de causa e efeito. Um exemplo é uma variável oculta de "clima" que determina tanto "vendas de sorvete" quanto "queimaduras de sol". Cada mundo inventado vira uma tabela, e a tabela é deliberadamente bagunçada, com valores ausentes, linhas duplicadas que se contradizem e números muito assimétricos. O modelo Large aprendeu com cerca de 137 milhões dessas tabelas sintéticas, da mesma forma que um piloto treina com tempestades simuladas antes de voar em uma real.

## Como algo tão pequeno consegue acompanhar?
O Kumo processa uma tabela em três etapas. Cada etapa tem uma única função, e cada uma é projetada para que o trabalho cresça devagar conforme a tabela aumenta. O artigo diz que é aí que está a maior parte da velocidade.

**Etapa 1: transformar cada célula em números.** O Kumo agrupa as células de uma linha três de cada vez em um único token, o mesmo tipo de unidade que os modelos de linguagem usam para pedaços de texto. Cada valor é descrito por um conjunto de padrões de onda, chamados de variáveis de Fourier (senos e cossenos), para que o modelo consiga distinguir pequenas diferenças. Uma célula ausente é substituída pela média da coluna mais um sinalizador que diz que o valor estava faltando. O usuário nunca precisa preencher os vazios. As linhas com respostas conhecidas carregam a resposta junto.

**Etapa 2: resumir cada linha lendo a tabela em duas direções.** Ler uma coluna de cima para baixo ensina o modelo o que é normal para aquela coluna. Um valor de 42 pode ser uma idade comum ou um número alarmante de reclamações, dependendo da coluna. O Kumo faz isso comparando cada linha com um conjunto fixo de 128 ou 256 pontos de referência, em vez de comparar com todas as outras linhas. Dobrar o número de linhas, portanto, aproximadamente dobra o trabalho, em vez de quadruplicá-lo. Ler uma linha da esquerda para a direita ensina o modelo como as colunas se relacionam. Ao fim desta etapa, cada linha, tenha ela cinco colunas ou noventa, está comprimida em quatro tokens.

**Etapa 3: fazer a previsão sob uma regra rígida.** As linhas com respostas podem se "observar" entre si. As linhas a serem previstas só podem observar as linhas respondidas, nunca umas às outras. O artigo compara isso a alunos de prova que podem ler o livro-texto, mas não as provas uns dos outros. O resultado é que as linhas respondidas são processadas apenas uma vez. Depois disso, novas perguntas podem ser respondidas contra os mesmos exemplos de forma muito barata. É o modo "ajustar uma vez, prever muitas" da biblioteca da NVIDIA.

> 🔎 Contexto adicionado: Atenção (attention) é o mecanismo que permite a um modelo ponderar quais outros itens importam ao processar um item.

**Atenção sensível ao tamanho.** A atenção se dilui à medida que o número de linhas cresce, então um modelo treinado com tabelas de 1.024 linhas poderia perder o foco numa tabela de 50.000 linhas. O Kumo compensa escalando a atenção pelo logaritmo do número de linhas, usando um fator aprendido separadamente para cada cabeça de atenção. O artigo observa que é a mesma ideia básica que o TabICLv2 usou, sob o nome QASSMax.

**Etapas de treinamento.** O treinamento rodou em três fases. Começou com tabelas de 1.024 linhas e até 100 colunas, passou para tabelas de 400 a 10.240 linhas e terminou com tabelas de até 60.000 linhas. O número de 60.000 linhas marca o limite do que o Kumo já viu.

**Ensemble na biblioteca.** A biblioteca da NVIDIA acrescenta uma técnica sobre o modelo. Se você pedir oito estimadores, ela roda o modelo em oito cópias ligeiramente diferentes da sua tabela, cada uma com colunas embaralhadas e números reescalonados, e tira a média dos resultados. É a versão em IA de perguntar a oito colegas e seguir o consenso. A biblioteca mantém no máximo 500 colunas por cópia, o que importa para tabelas muito largas.

## Conferindo o post: é mesmo 23 vezes mais rápido?
Os próprios testes da NVIDIA colocam o Kumo em primeiro em quatro rankings públicos. No TabArena, a NVIDIA diz que ele supera todos os rivais enquanto roda 17 vezes mais rápido que o LimiX-2. O autor trata o material de lançamento com cautela, já que todo gráfico de lançamento mostra o modelo novo vencendo. A evidência mais útil é um benchmark independente da AIMultiple, a fonte dos números do post. Ele rodou 13 modelos nos mesmos 19 conjuntos de dados reais, com o mesmo tipo de GPU, e repetiu cada modelo cinco vezes em fatias diferentes dos dados.

O artigo lê os resultados em um gráfico em que maior significa mais preciso e mais à esquerda significa mais rápido. O canto superior esquerdo é o melhor lugar. Os pontos cheios marcam modelos que uma empresa pode usar livremente, e os círculos vazios marcam modelos cujas licenças gratuitas proíbem o uso comercial.

**O resultado principal se sustenta.** O Kumo precisou de 7,6 segundos por rodada de teste, contra 173,6 segundos do TabFM. Isso dá 166 segundos a menos, ou quase exatamente 23 vezes mais rápido. O Kumo terminou entre os três primeiros em 16 dos 19 conjuntos de dados. Ao preço de US$ 5,98 por hora de GPU, cobrado pela AIMultiple, o benchmark inteiro custou US$ 1,19 no Kumo e US$ 27,40 no TabFM. A média do TabFM é em parte puxada pelas maiores tabelas, já que uma rodada típica dele levou 16,9 segundos. Na primeira execução, ele também gasta mais de 20 minutos de preparação antes de fazer qualquer previsão.

**Um número está muito errado.** O post diz que o TabFM tem 1,65 bilhão de parâmetros. O artigo científico do próprio Google lista 408,7 milhões. O autor sugere que a confusão pode vir do fato de 1,65 estar próximo do tamanho, em gigabytes, de um arquivo de 409 milhões de parâmetros armazenado em precisão total. Com o número correto, o Kumo Large tem cerca de metade do TabFM, e o Kumo Small é aproximadamente 15 vezes menor.

**Os dois modelos que superam o Kumo foram deixados de fora.** O LimiX-2, da Stable AI, é o modelo mais preciso do teste. Ele tem uma posição média de 2,16, contra 2,63 do Kumo, e 11 vitórias absolutas. Levou 111,9 segundos por rodada, 104 segundos a mais que o Kumo, ou cerca de 15 vezes mais lento. Também usou por volta de 124 GB de memória de GPU nos maiores conjuntos de dados, mais do que uma H100 de 80 GB comporta. O TabPFN-3.5, da Prior Labs, vai no sentido oposto. Ele termina uma rodada em 4,3 segundos, 3,3 segundos a mais rápido que o Kumo, com posição média de 3,26, cerca de 0,6 posição atrás.

> ❌ **Anti-padrão: confiar no gráfico de lançamento:** Aceitar um benchmark porque o gráfico do próprio fornecedor mostra o modelo novo no topo. **Por que as pessoas fazem isso:** Posts de lançamento são persuasivos e fáceis de compartilhar. **Consequência:** Afirmações de velocidade ou precisão passam sem checagem, e a comparação pode deixar de fora os rivais mais fortes. **Correção:** Consulte um benchmark independente que inclua as alternativas mais fortes, e recheque a contagem de parâmetros e os custos contra os artigos originais.

## As letras miúdas que o post deixou de fora
Quase todos os modelos do gráfico podem ser baixados, mas a maioria não pode ser usada livremente em uma empresa. Os criadores dos modelos costumam publicar o código sob uma licença e os pesos treinados, que são justamente o que faz as previsões, sob uma licença mais restritiva. Para esta família de modelos, a licença mais restritiva normalmente limita o uso a pesquisa e testes.

O TabPFN-3.5 é o caso mais estrito. A licença dele proíbe usar até os resultados do modelo para decisões internas de negócio, então uma análise de churn da sua própria empresa conta como uso e exige a licença empresarial da Prior Labs. O TabFM tem o caminho mais viável entre os modelos restritos, já que o Google o executa dentro do seu data warehouse BigQuery. Isso serve a equipes cujos dados já estão lá.

Restam três modelos que qualquer empresa pode usar livremente hoje: o Kumo, o TabICLv2 e o Mitra-v2, da Amazon. O Kumo ficou cerca de quatro posições à frente do TabICLv2 e do Mitra-v2 no teste da AIMultiple. O artigo observa que o Mitra-v2 rodou ali sem a breve etapa de ajuste fino que seus criadores usam nos próprios resultados publicados, então pode estar subestimado. Entre os modelos que uma empresa pode usar livremente, a conclusão do autor é que o Kumo é a primeira escolha, com folga.

> ❌ **Anti-padrão: confundir código aberto com licença livre:** Presumir que um modelo é livre para uso comercial porque seu código ou seus pesos podem ser baixados. **Por que as pessoas fazem isso:** Pesos baixáveis parecem software de código aberto. **Consequência:** Um projeto pode ser construído sobre um modelo cuja licença proíbe o uso comercial, e seus resultados também podem ser restringidos. **Correção:** Leia a licença dos pesos, não só a do código, e verifique se ela cobre resultados usados em decisões internas.

## O que a licença do Kumo permite de fato
Os pesos do Kumo são publicados sob a OpenMDW-1.1, uma licença da Linux Foundation cujo nome significa Open Model, Data and Weights. A NVIDIA a usa em 19 de seus modelos, incluindo a família Nemotron. O autor diz que ela se parece muito com a licença MIT, uma das mais permissivas do software de código aberto. Em termos simples, ela permite usar, modificar, ajustar finamente, compartilhar e vender o que você construir com o Kumo, desde que você inclua uma cópia da licença e os avisos de copyright originais sempre que repassar o modelo a alguém.

Dois outros pontos importam para empresas. As previsões que você obtém do modelo pertencem a você, sem condições atreladas aos resultados. A única forma de perder a licença é processar alguém alegando que o modelo infringe suas patentes ou direitos autorais. O autor observa que a maioria das empresas nunca vai chegar perto dessa cláusula.

O artigo lista três ressalvas.
- O pacote de servidor pronto da NVIDIA, o contêiner NIM, é regido pela licença de software própria da NVIDIA, e não pela OpenMDW, então leia essa licença separadamente se você a usar.
- A receita e o gerador de dados usados para treinar o Kumo foram prometidos, mas não publicados. Você pode usar e ajustar o modelo, mas ninguém fora da NVIDIA consegue reconstruí-lo do zero.
- A Open Source Initiative ainda está revisando a OpenMDW-1.1. Se a sua empresa aprova apenas licenças de código aberto oficialmente listadas, espere uma conversa com o jurídico antes de colocar qualquer coisa em produção.

## Experimente você mesmo em uma dúzia de linhas de Python
A configuração exige Python 3.11 ou mais recente e PyTorch 2.7 ou mais recente. Instalar a biblioteca de modelos de dados estruturados da NVIDIA leva um comando, e os pesos do Kumo são baixados do Hugging Face no primeiro uso. Uma GPU deixa tudo mais rápido, mas o exemplo oficial volta para uma CPU comum, o que serve para tabelas pequenas.

```bash
pip install structured-data-models
```

> 🔎 Contexto adicionado: A fonte marca este comando de shell como javascript. O código em si não foi alterado.

**O que isso faz:**
- Instala a biblioteca da NVIDIA, que fornece o pacote sdm usado nos exemplos.
- Baixa os pesos do Kumo do Hugging Face automaticamente na primeira vez que um modelo é usado.
- Não exige configuração extra para treinamento, porque não há etapa de treinamento.

O primeiro exemplo completo usa o conjunto de dados de câncer de mama do scikit-learn. As primeiras 300 linhas, cujas respostas são conhecidas, servem de exemplos, e o Kumo prevê as 269 restantes.

```py
import torch
from sklearn.datasets import load_breast_cancer

import sdm
df = load_breast_cancer(as_frame=True).frame  # 569 rows, 30 measurements, "target" is the answer
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
# Turn the pandas table into a format the library understands
table = sdm.TableTensor.from_pandas(
    df=df,
    stypes=sdm.infer_stypes(df, overrides={"target": "categorical"}),
    device=device,
)
# Pick a size: "small", "medium" or "large" (the default)
model = sdm.models.KumoTabular(task="classification", size="small", device=device)
with torch.amp.autocast(device.type, dtype=torch.float16, enabled=device.type == "cuda"):
    probs = model(
        x_context=table[:300].drop_columns("target"),  # the examples
        y_context=table[:300, "target"],               # their answers
        x_query=table[300:].drop_columns("target"),    # the rows to predict
        num_estimators=8,                              # 8 shuffled versions, averaged
    )
probs_df = probs.to_pandas()           # one column per possible answer
print(probs_df.head())                 # the probabilities
print(probs_df.idxmax(axis=1).head())  # the most likely answer for each row
```

**O que isso faz:**
- Converte um DataFrame do pandas para o formato de tensor da biblioteca e marca "target" como uma resposta categórica.
- Seleciona o tamanho small do modelo, o mais rápido dos três.
- Passa as primeiras 300 linhas como contexto (exemplos e suas respostas) e as demais como consulta, que é todo o "treinamento".
- Usa autocast em float16 na GPU para acelerar o cálculo. O código volta para a precisão total na CPU.
- Faz a média de oito versões embaralhadas da tabela, o que deixa as probabilidades mais estáveis.

O segundo exemplo mostra o padrão de ajustar uma vez e prever muitas, para quando você faz muitas perguntas contra os mesmos exemplos.

```py
model.fit(x=table[:300].drop_columns("target"), y=table[:300, "target"], num_estimators=8)
batch_a = model.predict(table[300:450].drop_columns("target"))
batch_b = model.predict(table[450:].drop_columns("target"))
model.clear()  # free the memory when you're done
```

**O que isso faz:**
- Chama fit uma vez para processar os exemplos, então o trabalho caro de lê-los acontece apenas uma vez.
- Roda predict em lotes separados de linhas, que reutilizam os exemplos já processados.
- Chama clear no final para liberar a memória da GPU ou da CPU retida pelos exemplos.

O terceiro exemplo trata um alvo numérico. Ele usa o conjunto de dados de diabetes do scikit-learn e, em vez de um palpite por linha, o modelo devolve 999 colunas de quantis que descrevem a faixa provável.

```py
from sklearn.datasets import load_diabetes

df_reg = load_diabetes(as_frame=True).frame  # 442 rows, 10 measurements, numeric "target"
reg_table = sdm.TableTensor.from_pandas(df=df_reg, stypes=sdm.infer_stypes(df_reg), device=device)
reg = sdm.models.KumoTabular(task="regression", size="medium", device=device)
quantiles = reg(
    x_context=reg_table[:350].drop_columns("target"),
    y_context=reg_table[:350, "target"],
    x_query=reg_table[350:].drop_columns("target"),
    num_estimators=8,
).to_pandas()
# q500 is the middle guess, and q050 to q950 brackets a 90% range
print(quantiles[["q050", "q500", "q950"]].head())
```

**O que isso faz:**
- Muda a tarefa para regressão e usa o tamanho medium do modelo.
- Dá ao modelo 350 linhas de exemplo e pergunta sobre as 92 restantes.
- Devolve colunas de quantis de q001 a q999, então a saída é uma faixa de valores prováveis, e não um número único.
- Lê as colunas do 5º, 50º e 95º percentis para obter um palpite central e uma faixa de 90%.

O artigo acrescenta que o código-fonte da biblioteca trata algumas perguntas automaticamente. Uma pergunta com mais de 10 respostas possíveis é dividida em subperguntas menores, por trás dos panos. Para comparar com os rivais, você muda uma linha, como `sdm.models.TabICLv2(device=device)` para o TabICLv2 ou `sdm.models.TabFM(task="classification", device=device)` para o TabFM. A opção do TabFM primeiro pede que você aceite a licença não comercial dele. Texto livre e datas precisam ser convertidos em números antes que o Kumo possa usá-los, e a biblioteca inclui funções auxiliares para isso. Para dados espalhados por várias tabelas ligadas, `sdm.models.KumoRelational` e o notebook de previsão de churn da NVIDIA são os pontos de partida recomendados. Quem prefere não rodar código pode testar uma versão hospedada em [build.nvidia.com](http://build.nvidia.com), ou usar o contêiner NIM nos próprios servidores.

## Quando eu ainda escolheria o XGBoost
O Kumo tem limites claros. Ele entende apenas números e categorias, então texto e datas precisam ser convertidos antes. Nunca viu uma tabela com mais de 60.000 linhas ou 100 colunas, e a NVIDIA alerta que a precisão pode cair bem além desses tamanhos.

O maior risco envolve o tempo. O Kumo assume que as linhas que você quer prever se parecem com as linhas que você deu como exemplo. Prever o churn do próximo trimestre a partir dos clientes do ano passado, depois que os preços ou o mercado mudaram, é exatamente a situação em que os resultados podem piorar sem alarde. O artigo diz que a própria NVIDIA lista isso como limitação.

> ❌ **Anti-padrão: reutilizar exemplos atravessando uma mudança no tempo:** Alimentar um modelo com exemplos de um período anterior e tratar as respostas dele como válidas para um período posterior, cujas condições mudaram. **Por que as pessoas fazem isso:** O fluxo sem treinamento torna fácil reutilizar os mesmos exemplos para cada nova pergunta. **Consequência:** As previsões podem piorar sem nenhum erro visível, porque o modelo continua devolvendo probabilidades com aparência confiável. **Correção:** Verifique se os exemplos correspondem à situação que você está prevendo, e monitore os resultados contra os desfechos reais à medida que as condições mudam.

O veredito do autor tem três partes. Para uma tabela com menos de 60.000 linhas e uma pergunta a responder, o Kumo Tabular é a primeira coisa a testar antes de qualquer ajuste. Leva cerca de uma dúzia de linhas, informa o quanto está seguro de si, fica no grupo de topo de um teste independente custando uma fração dos modelos pesados, e é o modelo mais preciso dessa lista que uma empresa pode usar sem assinar nada. Se a licença não for uma preocupação, vale testar também o LimiX-2. Se você pontua milhões de linhas por dia ou busca o último ponto de precisão em uma tabela muito grande, as árvores de gradiente continuam sendo a ferramenta certa.

## Referências
- NVIDIA, anúncio do Kumo Tabular no blog do Hugging Face, 29 de setembro de 2026
- NVIDIA, model card do Kumo-Tabular e model card do NIM
- NVIDIA, página de desenvolvedor do NVIDIA Kumo
- NVIDIA, pacote e código-fonte structured-data-models 0.2.1
- AIMultiple, Tabular Models Benchmark: Performance Across 19 Datasets, atualizado em 12 de outubro de 2026
- Let's Data Science, Nvidia acquires Kumo AI predictive model maker, junho de 2026
- The Linux Foundation, submissão da OpenMDW-1.1 à lista de revisão de licenças da OSI, agosto de 2026
- Google Research, Introducing TabFM, o artigo do TabFM e o model card do TabFM

## 🔑 Principais conclusões
- O Kumo Tabular preenche valores desconhecidos de uma tabela em uma única passada, lendo as linhas que já têm respostas, então não precisa de treinamento, ajuste ou engenharia de variáveis por pergunta.
- No teste de 19 conjuntos de dados da AIMultiple, o Kumo precisou de 7,6 segundos por rodada, contra 173,6 segundos do TabFM, cerca de 23 vezes mais rápido, e terminou entre os três primeiros em 16 dos 19 conjuntos.
- O post de lançamento exagerou o tamanho do TabFM. O artigo do Google lista 408,7 milhões de parâmetros, não 1,65 bilhão, então o Kumo Large tem cerca de metade do TabFM.
- O modelo mais preciso do teste (LimiX-2) e o mais rápido (TabPFN-3.5) têm licenças que restringem o uso comercial, então o Kumo é a opção mais precisa que uma empresa pode usar livremente.
- O desenho em três etapas do Kumo (células em tokens, resumos de linha usando 128 ou 256 pontos de referência, e uma etapa de resposta em que as linhas desconhecidas não se observam entre si) mantém o trabalho crescendo de forma quase linear com o tamanho da tabela.
- Os pesos do Kumo estão sob a OpenMDW-1.1, que permite uso comercial com licença e aviso de copyright, mas a receita de treinamento não foi publicada e a licença ainda está em revisão pela OSI.
- O principal risco é a deriva temporal: as previsões podem se degradar quando as linhas a prever deixam de se parecer com os exemplos, e o Kumo não avisa sobre isso.

## 🏛️ Para o Kenji
Para um arquiteto, o Kumo muda o custo de uma funcionalidade de ML. Um pipeline de treinamento por pergunta pode se transformar em uma chamada de ajustar-uma-vez, prever-muitas atrás de uma fronteira de serviço, o que afeta como você dimensiona e governa essa chamada. A pergunta que isso levanta para o seu trabalho de governança é quem é dono do monitoramento de deriva quando os exemplos deixam de corresponder aos dados em produção. Esta é a minha interpretação, não a do autor.

## 📚 Plano de leitura
### A partir do artigo
- [build.nvidia.com](http://build.nvidia.com) — a versão hospedada da NVIDIA, para testar o Kumo sem instalar nada.

### Para se aprofundar
- Benchmark de modelos tabulares da AIMultiple (19 conjuntos de dados, atualizado em 12 de outubro de 2026) — o teste independente por trás dos números de velocidade e precisão.
- Post de lançamento do Kumo Tabular no blog do Hugging Face da NVIDIA (29 de setembro de 2026) — os detalhes de design e as afirmações da própria NVIDIA.
- Artigo do TabFM e model card do Google Research — para conferir a contagem de parâmetros e os termos de licença.
- Artigos do TabICL e do TabICLv2 — a família de modelos em que o desenho do Kumo se baseia, incluindo a ideia de escalonamento da atenção.
- Texto da licença OpenMDW-1.1 — leia antes de usar os pesos em um produto comercial.
- Documentação das árvores de gradiente (XGBoost) — a referência de comparação quando a tabela é grande ou o orçamento de velocidade é apertado.
