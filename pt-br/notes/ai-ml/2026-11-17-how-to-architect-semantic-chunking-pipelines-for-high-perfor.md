---
type: Article Summary
title: Como Arquitetar Pipelines de Chunking Semântico para RAG de Alto Desempenho
description: A estratégia de chunking determina o que o RAG recupera, então escolha entre cinco métodos de acordo com a estrutura do documento e as necessidades de recuperação.
resource: https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
tags:
- ai-ml
- rag
- rag
- parent-child-chunking
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
  title: How to Architect Semantic Chunking Pipelines for High-Performance RAG
  author: Kuriko Iwai
status: stable
source_url: https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
source: Medium
source_type: medium
published: '2026-09-29'
domain: ai-ml
subdomain: rag
concepts:
- rag
- parent-child-chunking
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81448119c16027420c89
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-how-to-architect-semantic-chunking-pipelines-for-high-perfor.md
---

# Como Arquitetar Pipelines de Chunking Semântico para RAG de Alto Desempenho

**Fonte:** https://medium.com/@kuriko-iwai/how-to-architect-semantic-chunking-pipelines-for-high-performance-rag-c539ea658865
**Autor:** Kuriko Iwai
**Data:** 2026-09-29
**Conceitos:** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md) · [Chunking pai-filho](/pt-br/concepts/ai-ml/parent-child-chunking.md)

---

## 🎯 Tese central
Chunking é uma decisão de arquitetura de recuperação, não um detalhe de pré-processamento. Como você divide os documentos determina o que um sistema RAG consegue encontrar e quanto contexto o modelo recebe ao responder.

## 🧭 Por que o autor escreveu
O RAG se tornou a forma padrão de ancorar a IA em dados privados, mas suas respostas só são tão boas quanto o que a recuperação devolve. O autor defende que a etapa de divisão merece o mesmo cuidado de design que o próprio pipeline de recuperação, e compara cinco estratégias de chunking sobre um mesmo texto de exemplo, para que as diferenças de trade-off possam ser vistas diretamente.

## Introdução
A introdução descreve o RAG como o padrão para ancorar um modelo em dados privados e afirma que a qualidade da saída é limitada pela informação recuperada. Ela argumenta que as equipes precisam ir além da ingestão simples e projetar pipelines de recuperação de forma deliberada. A alavanca que ela aponta é o chunking: quebrar grandes conjuntos de dados em segmentos digeríveis e com significado semântico. O plano do artigo é explicar primeiro o mecanismo e depois avaliar as principais estratégias usadas em aplicações modernas de IA.

## O que é chunking
Chunking é o processo de dividir grandes volumes de texto em partes menores e mais fáceis de gerenciar, chamadas chunks, antes de serem convertidas em embeddings.

### Chunking no pipeline de ingestão
A Figura A do artigo mostra o fluxo de ingestão de dados. Dados brutos e não estruturados chegam em formatos como texto (PDFs, documentos), imagens, áudio e vídeo. Primeiro eles são recuperados, depois passam por chunking, depois por um modelo de embeddings e, por fim, são gravados em um vector store. A recuperação traz os dados para dentro; o chunking é a segunda caixa, e roda depois da recuperação. No momento da consulta, o sistema pode devolver apenas o chunk que responde à pergunta, em vez do arquivo inteiro.

### Embeddings e o vector store
Um modelo de embeddings transforma cada chunk em um vetor de embedding, que é uma longa lista de números (coordenadas) representando o significado semântico do chunk. O vector store guarda esses embeddings. A Figura A usa três dimensões apenas para demonstração, mas espaços reais costumam ter muitas dimensões. Chunks relacionados ficam armazenados próximos uns dos outros. O exemplo do autor é um chunk sobre "Golden Retrievers" e outro sobre "Labradores", que ficam próximos, de modo que o LLM consegue encontrar material relevante quase instantaneamente.

### Por que o chunking importa
O artigo dá três razões:

- **Relevância:** chunks pequenos permitem que o RAG recupere exatamente o trecho que responde a uma pergunta dentro de um documento grande.
- **Eficiência de custo:** processar trechos pequenos e específicos economiza tokens e tempo de computação.
- **Retenção de contexto:** dados bem divididos mantêm informação suficiente ao redor para que o LLM entenda o porquê e o como.

Juntas, essas razões mostram que o chunking recupera o contexto mais relevante para uma consulta, mantendo os tokens de entrada baixos o suficiente para caber na janela de contexto do modelo.

> 🔎 Contexto adicionado: as três razões puxam em direções diferentes. Chunks pequenos favorecem relevância e custo, enquanto a retenção de contexto favorece chunks maiores. Essa tensão é o motivo pelo qual o artigo trata a escolha da estratégia como a decisão real.

## Análise comparativa: 5 estratégias de chunking padrão da indústria
O artigo afirma que nenhuma estratégia única serve para todos os casos e lista cinco: chunking de tamanho fixo, chunking recursivo por caractere, chunking específico por documento, chunking semântico e chunking pai-filho (hierárquico). Cada uma é aplicada ao mesmo texto de exemplo sobre infraestrutura de energia solar, então as diferenças na saída vêm do divisor, não da entrada.

### Texto de exemplo
O autor usa este exemplo nas cinco estratégias:

```py
text = f"""Solar panels, or photovoltaic cells, convert sunlight into
electricity. This process happens at the atomic level. Some materials exhibit
a property known as the photoelectric effect. This causes them to absorb
photons and release electrons. Beyond the cells, an inverter is required
to convert DC to AC. Large-scale solar farms also require battery storage
systems to manage peak load during non-sunny hours."""
```

**O que isso faz:**
- Constrói uma única string de várias linhas com seis frases sobre painéis solares, o efeito fotoelétrico, inversores e armazenamento em baterias.
- As aspas triplas permitem quebras de linha sem caracteres de escape.
- Espaços no fim das linhas e quebras de linha dentro da string são mantidos, então cada divisor recebe exatamente os mesmos bytes.
- O prefixo `f` não tem placeholders, então não tem efeito aqui.
- A entrada compartilhada é o que torna as cinco estratégias comparáveis.

## 1. Chunking de tamanho fixo
O chunking de tamanho fixo é a abordagem mais simples. Ele define um número fixo de caracteres ou tokens por chunk e, opcionalmente, sobrepõe chunks vizinhos, um método chamado janela deslizante. O exemplo da Figura B do artigo usa chunks de 500 tokens com sobreposição de 50 tokens. A sobreposição garante que o contexto não se perca se uma frase importante cruzar uma fronteira.

### Sobreposição por janela deslizante
A sobreposição repete o final de um chunk no início do próximo. Sem ela, uma frase cortada ao meio ficaria ilegível nos dois chunks. O exemplo do artigo usa uma janela de 100 unidades com sobreposição de 20 unidades.

### Prós
Computacionalmente barato e fácil de implementar.

### Melhor para
- Prototipagem rápida.
- Processamento de textos simples.
- Casos de uso gerais em que a velocidade importa mais do que a precisão semântica detalhada.

### Implementação prática
O artigo usa a classe `CharacterTextSplitter` da biblioteca `langchain_text_splitters`:

```py
from langchain_text_splitters import CharacterTextSplitter

# fixed size chunking
fixed_splitter = CharacterTextSplitter(
    separator="",
    chunk_size=100,
    chunk_overlap=20
)
fixed_chunks = fixed_splitter.split_text(text)
```

**O que isso faz:**
- Importa o divisor baseado em caracteres do pacote de divisão de texto do LangChain.
- `separator=""` significa que não há ponto de quebra preferido, então o divisor corta em tamanhos exatos.
- `chunk_size=100` limita cada chunk a 100 unidades.
- `chunk_overlap=20` carrega as últimas 20 unidades de cada chunk para o próximo.
- `split_text(text)` aplica o divisor à string de exemplo e devolve uma lista de chunks.

> 🔎 Contexto adicionado: `CharacterTextSplitter` mede o tamanho em caracteres, a menos que receba uma função de contagem de tokens. O artigo chama a saída de "100 tokens", mas os chunks que ele mostra têm cerca de 100 caracteres, então leia as unidades como caracteres.

### Chunks resultantes
O artigo relata dois chunks de 100 unidades cada:

- Chunk 1: "Solar panels, or photovoltaic cells, convert sunlight into electricity. This process happens at the a"
- Chunk 2: "ns at the atomic level. Some materials exhibit a property known as the photoelectric effect. This ca"

A palavra "atomic" é cortada na fronteira. O método é o mais rápido dos cinco, mas pode criar ruído semântico, porque o LLM recebe palavras parciais, o que pode degradar a resposta gerada.

> ❌ **Anti-padrão — cortar apenas por tamanho:** dividir em uma contagem de caracteres sem considerar palavras ou frases. **Por que as pessoas fazem isso:** é a coisa mais rápida de construir, e funciona em um protótipo. **Consequência:** palavras partidas e frases quebradas chegam ao LLM, o que degrada a qualidade da resposta. **Correção:** use um divisor recursivo ou sensível à estrutura para conteúdo real, e mantenha janelas de tamanho fixo apenas para casos em que a velocidade é a prioridade.

## 2. Chunking recursivo por caractere
O divisor recursivo por caractere usa uma hierarquia de separadores para encontrar um ponto de quebra natural, em vez de cortar em um limite rígido de caracteres. O método tenta primeiro o separador mais significativo e desce na lista até que o tamanho do chunk seja atingido. O diagrama do artigo (Figura C) mostra essa hierarquia, de quebras de linha e pontos até vírgulas e espaços.

### Hierarquia de separadores
Cada separador é um candidato a ponto de quebra, testado em ordem de quão grande é a fronteira que ele marca. Uma fronteira maior, como uma quebra de parágrafo, mantém mais texto relacionado junto do que um espaço. O método só desce para um separador menor quando o maior deixa um chunk grande demais.

### Prós
Mantém ideias relacionadas juntas melhor do que a divisão de tamanho fixo.

### Melhor para
- Preservar a integridade de parágrafos e frases.
- Artigos e blogs.

### Implementação prática
O artigo usa a classe `RecursiveCharacterTextSplitter` do mesmo pacote:

```py
from langchain_text_splitters import RecursiveCharacterTextSplitter

recursive_splitter = RecursiveCharacterTextSplitter(
    chunk_size=100,
    chunk_overlap=20,
    separators=["\n\n", "\n", " ", ""]
)
recursive_chunks = recursive_splitter.split_text(text)
```

**O que isso faz:**
- Importa o divisor recursivo, que testa cada separador em sequência antes de recorrer ao próximo.
- `chunk_size=100` e `chunk_overlap=20` definem o mesmo tamanho e sobreposição do exemplo de tamanho fixo, para que a comparação seja justa.
- `separators=["\n\n", "\n", " ", ""]` lista quebras de linha duplas, quebras simples, espaços e, por fim, qualquer caractere, do mais ao menos preferido.
- A string vazia no fim é o recurso de último caso, que corta em qualquer caractere.
- `split_text(text)` devolve chunks que preferem fronteiras de parágrafo e de palavra.

> 🔎 Contexto adicionado: o texto diz que o método divide primeiro nos pontos, mas a lista de separadores do código não tem entrada para ponto. O código é o guia confiável para saber o que o divisor faz. Além disso, o artigo diz que cada chunk contém "exatamente 100 tokens", mas os chunks que ele mostra são mais curtos. O limite de tamanho não preenche os chunks: um chunk tem apenas o tamanho que o texto permite antes do próximo separador.

### Chunks resultantes
- Chunk 1: 'Solar panels, or photovoltaic cells, convert sunlight into electricity.'
- Chunk 2: 'This process happens at the atomic level. Some materials exhibit a property'

Comparado com a divisão de tamanho fixo, o divisor recursivo para no ponto que termina a primeira frase, então a gramática se mantém intacta. O artigo diz que isso torna a saída mais legível para o LLM.

## 3. Chunking específico por documento
O chunking específico por documento respeita o formato nativo do arquivo, como Markdown, HTML, LaTeX ou código. O divisor usa a própria estrutura do documento como fronteiras:

- **Markdown:** divide por cabeçalhos (`#`, `##`, `###`).
- **HTML:** divide por tags HTML, vírgulas ou pontos.
- **Código:** divide por definições de função ou de classe.

### Fronteiras sensíveis à estrutura
A ideia é que o autor já marcou as seções lógicas. Usar esses marcadores mantém o conteúdo de uma seção junto e impede que seções não relacionadas se misturem.

### Prós
Preserva a hierarquia lógica do documento.

### Melhor para
- Documentação técnica altamente estruturada, como relatórios em PDF e manuais.
- Bases de código.

### Implementação prática
O artigo usa a classe `MarkdownHeaderTextSplitter` para definir os níveis de cabeçalho e dividir o documento:

```py
from langchain_text_splitter import MarkdownHeaderTextSplitter

markdown_document = """
# Solar Energy Guide
## The Physics
Solar panels, or photovoltaic cells, convert sunlight into electricity.
This process happens at the atomic level.
Some materials exhibit the photoelectric effect.
## The Hardware
Beyond the cells, an inverter is required to convert DC to AC.
Large-scale solar farms also require battery storage systems.
### Maintenance
Regular cleaning of panels ensures maximum photon absorption.
"""
# define the headers
headers_to_split_on = [
    ("#", "Header 1"),
    ("##", "Header 2"),
    ("###", "Header 3"),
]
# initialize the splitter
markdown_splitter = MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)
# split
md_header_splits = markdown_splitter.split_text(markdown_document)
```

**O que isso faz:**
- Define um pequeno documento Markdown com um título, duas seções de nível 2 e uma subseção de nível 3.
- `headers_to_split_on` associa cada marcador Markdown a uma chave de metadados, para que cada chunk registre sob quais cabeçalhos está.
- As três tuplas cobrem os níveis de cabeçalho de 1 a 3.
- `MarkdownHeaderTextSplitter(...)` constrói o divisor a partir desse mapeamento.
- `split_text(markdown_document)` devolve um chunk por seção, com o caminho dos cabeçalhos armazenado como metadados.

> 🔎 Contexto adicionado: a linha de import usa o nome de pacote no singular, `langchain_text_splitter`, enquanto os exemplos anteriores usam `langchain_text_splitters`. Como está escrito, o import provavelmente falhará. Verifique o nome do pacote antes de executar este trecho.

### Chunks resultantes
Chunk 1:
- Conteúdo: Solar panels, or photovoltaic cells, convert sunlight into electricity. This pro…
- Metadados: `{'Header 1': 'Solar Energy Guide', 'Header 2': 'The Physics'}`

Chunk 2:
- Conteúdo: Beyond the cells, an inverter is required to convert DC to AC. Large-scale solar…
- Metadados: `{'Header 1': 'Solar Energy Guide', 'Header 2': 'The Hardware'}`

Chunk 3:
- Conteúdo: Regular cleaning of panels ensures maximum photon absorption….
- Metadados: `{'Header 1': 'Solar Energy Guide', 'Header 2': 'The Hardware', 'Header 3': 'Maintenance'}`

O método usa a intenção do autor, então "The Physics" e "The Hardware" nunca se fundem em um mesmo chunk. O artigo diz que isso importa mais para manuais técnicos.

## 4. Chunking semântico
O chunking semântico mede como o significado muda de uma frase para a seguinte. Ele calcula a similaridade de cosseno entre os embeddings de frases consecutivas. Quando a similaridade cai abaixo de um limiar, o método assume que um novo tópico começou e inicia um novo chunk.

### Similaridade de cosseno entre frases consecutivas
A similaridade de cosseno compara a direção de dois vetores. Valores próximos de 1 significam que as frases apontam para o mesmo lado no espaço de embeddings, ou seja, tratam de coisas semelhantes. Valores mais baixos significam que o significado mudou.

> 🔎 Contexto adicionado: o artigo não define similaridade de cosseno nem o limiar. A definição aqui é um conhecimento básico padrão. O valor do limiar nunca é informado no artigo.

### Prós
Alta precisão de recuperação, porque os chunks representam ideias completas.

### Melhor para
- Artigos acadêmicos.
- Documentos muito narrativos.
- Ensaios longos, em que os parágrafos não coincidem com as mudanças de tópico.

### Implementação prática
O artigo primeiro gera embeddings das frases com a classe `SentenceTransformer` da biblioteca `sentence_transformers` e depois compara embeddings consecutivos:

```py
from sklearn.metrics.pairwise import cosine_similarity
from sentence_transformers import SentenceTransformer

# split the text into sentences
sentences = split_into_sentences(text)

# create vector embeddings
model = SentenceTransformer(MODEL)
embeddings = model.encode(sentences)

# compute cosine similarity to split into chunks
chunks = []
current_chunk = [sentences[0]]
for i in range(1, len(sentences)):
    prev_emb = embeddings[i - 1].reshape(1, -1)
    curr_emb = embeddings[i].reshape(1, -1)
    similarity = cosine_similarity(prev_emb, curr_emb)[0][0]
    print(f"Similarity between sentence {i-1} and {i}: {similarity:.3f}")

    # if similarity drops → meaning shift → split
    if similarity < similarity_threshold:
        chunks.append(" ".join(current_chunk))
        current_chunk = [sentences[i]]
    else:
        current_chunk.append(sentences[i])

# add last chunk
if current_chunk:
    chunks.append(" ".join(current_chunk))
return chunks
```

**O que isso faz:**
- Importa `cosine_similarity` do scikit-learn e `SentenceTransformer` da biblioteca sentence-transformers.
- `split_into_sentences(text)` produz a lista de frases sobre a qual a comparação é feita.
- `model.encode(sentences)` devolve um vetor de embedding por frase, em um único lote.
- Cada par de embeddings vizinhos é remodelado para uma linha 2D, que `cosine_similarity` exige, e a pontuação é impressa para que o limiar possa ser ajustado.
- Uma pontuação abaixo de `similarity_threshold` fecha o chunk atual e inicia outro. Caso contrário, a frase se junta ao chunk atual. O último chunk é anexado após o laço.

> 🔎 Contexto adicionado: o trecho usa `split_into_sentences`, `MODEL` e `similarity_threshold` sem defini-los, e o `return` fica fora de qualquer função. Trate-o como um esboço. O limiar precisa ser escolhido e validado com os seus próprios dados.

### Chunks resultantes
O artigo afirma que o sistema percebe a mudança de significado entre "release electrons" e "Beyond the cells". As pontuações de similaridade de cosseno relatadas são:

- Chunk 1: Solar panels, or photovoltaic cells, convert sunlight into electricity. (0.196)
- Chunk 2: This process happens at the atomic level. (0.313)
- Chunk 3: Some materials exhibit a property known as the photoelectric effect. (0.546)
- Chunk 4: This causes them to absorb photons and release electrons. (0.266)
- Chunk 5: Beyond the cells, an inverter is required to convert DC to AC. (0.336)
- Chunk 6: Large-scale solar farms also require battery storage systems to manage peak load during non-sunny hours.

A conclusão do artigo é que o chunking semântico reconhece a passagem da física para a engenharia e força uma divisão.

> 🔎 Contexto adicionado: o limiar não é informado. A saída tem uma frase por chunk, e a maior pontuação listada (0.546) ainda assim produziu uma divisão. Isso implica um limiar acima de 0.546, o que dividiria todos os pares. Portanto, o exemplo não mostra frases se agrupando. Ele mostra apenas as pontuações, e o limiar precisa ser definido e testado antes que essa saída tenha significado.

## 5. Chunking pai-filho (hierárquico)
O chunking pai-filho armazena os mesmos dados em dois tamanhos: um chunk filho pequeno para busca e um chunk pai maior para contexto. O sistema busca primeiro nos chunks filhos pequenos e específicos. Quando um deles corresponde, recupera o pai maior ao redor dele e entrega esse pai ao LLM. O exemplo do artigo usa cerca de 100 tokens para o filho e cerca de 1000 tokens para o pai.

### Buscar pequeno, devolver grande
A correspondência precisa usa unidades pequenas, e boas respostas precisam do contexto ao redor. A recuperação pai-filho separa as duas coisas. O índice guarda os filhos, e a resposta traz o pai.

### Prós
Evita o problema do lost in the middle ao dar ao LLM muito material de contexto.

> 🔎 Contexto adicionado: "lost in the middle" descreve modelos que usam informações de forma menos confiável quando elas estão no meio de um contexto longo. O artigo nomeia o problema, mas não o explica além disso.

### Melhor para
- Sistemas RAG de nível empresarial.
- Equilibrar busca de alta precisão com contexto abrangente.

### Implementação prática
O artigo cria um chunk pai que contém o texto de exemplo inteiro, depois cria chunks filhos, cada um com uma frase e um embedding:

```py
import re, uuid
from sentence_transformers import SentenceTransformer

# create parent chunk
parent_chunk = {
    "id": str(uuid.uuid4()),
    "text": text.strip() # the entire sample text
}
# load embedding model
model = SentenceTransformer("all-MiniLM-L6-v2")
def create_child_chunks(parent_chunk):
    sentences = split_into_sentences(parent_chunk["text"])

    children = []
    for sentence in sentences:
        child = {
            "id": str(uuid.uuid4()),
            "parent_id": parent_chunk["id"],
            "text": sentence,
            "embedding": model.encode(sentence)
        }
        children.append(child)
    return children
child_chunks = create_child_chunks(parent_chunk)
```

**O que isso faz:**
- Cria o registro pai com um UUID e o texto de exemplo completo, sem espaços nas extremidades.
- Carrega `all-MiniLM-L6-v2`, um pequeno modelo de embeddings de frases da biblioteca sentence-transformers.
- `create_child_chunks` divide o pai em frases, e cada frase vira um filho com seu próprio UUID.
- Cada filho armazena `parent_id`, que é o vínculo que o recuperador segue do filho encontrado até o pai.
- Cada filho armazena seu embedding, então o índice de busca é construído sobre os filhos.

> 🔎 Contexto adicionado: o código gera um filho por frase, mas o texto do artigo descreve filhos de cerca de 100 tokens e pais de cerca de 1000 tokens. O código também importa `re` sem usá-lo, e `split_into_sentences` não está definido. Trate o código como um padrão e dimensione os filhos e os pais de acordo com os seus dados.

### Resultados
Contexto pai devolvido ao LLM:

> Solar panels, or photovoltaic cells, convert sunlight into electricity.
> This process happens at the atomic level.
> Some materials exhibit a property known as the photoelectric effect.
> This causes them to absorb photons and release electrons.
> Beyond the cells, an inverter is required to convert DC to AC.
> Large-scale solar farms also require battery storage systems to manage peak load during non-sunny hours.

Frase filha correspondente:

> Large-scale solar farms also require battery storage systems to manage peak load during non-sunny hours.

O recuperador compara a consulta com os embeddings dos filhos e depois devolve o chunk pai, em vez de apenas a frase correspondente. O LLM pode então ler o parágrafo completo e ver como painéis solares, inversores e armazenamento em baterias se relacionam. Nas palavras do artigo, isso explica o "porquê" da infraestrutura, não apenas o "o quê" das baterias.

> ❌ **Anti-padrão — devolver apenas o fragmento correspondente:** enviar o pequeno chunk encontrado diretamente ao LLM. **Por que as pessoas fazem isso:** é o pipeline mais simples, e a correspondência parece ser a própria resposta. **Consequência:** o modelo recebe uma frase sem o raciocínio ao redor, então consegue explicar o que é um componente, mas não por que ele está ali. **Correção:** armazene os filhos para busca e devolva os pais para contexto.

## Encerramento
A conclusão do artigo é que chunking não é apenas fatiar dados. Com uma estratégia adequada, ele se torna otimização semântica. Um bom agrupamento faz com que a pergunta de um usuário retorne um trecho coerente de informação, não um fragmento que deixa a IA adivinhando.

### Como escolher a estratégia ideal
O artigo oferece quatro pontos para escolher uma estratégia.

#### 1. Identifique padrões e estruturas lógicas
Procure repetição, sequência ou conexões inerentes nos dados. Em conteúdo técnico, isso significa cabeçalhos, tags Markdown ou quebras de parágrafo, para que um chunk não corte uma frase ou ideia vital. A pergunta-chave: os dados têm um layout ou uma sintaxe previsível que carrega significado? Se sim, use **chunking específico por documento (sensível à estrutura)**, com parsers que respeitem o formato nativo (Markdown, HTML ou LaTeX) e dividam em fronteiras lógicas, em vez de contagens de caracteres.

#### 2. Mantenha o contexto semântico (o mnemônico para IA)
Humanos usam mnemônicos para ligar ideias, e sistemas de IA usam chunks sobrepostos com o mesmo propósito. Incluir uma pequena parte do chunk anterior no início do próximo cria uma ponte narrativa, para que o modelo não perca o contexto mais amplo. A pergunta-chave: se eu lesse este chunk isoladamente, ainda entenderia do que trata a frase? Se não, use **chunking de tamanho fixo com janela deslizante**, com uma sobreposição de 10 a 20 por cento, para que o fim de um chunk e o início do seguinte compartilhem texto de conexão suficiente.

#### 3. Priorize o agrupamento por categoria
Organize as informações por categoria ou hierarquia, como uma lista de compras agrupada por hortifrúti ou laticínios. A pergunta-chave: qual é o nível de granularidade de que os usuários precisam, fatos específicos ou visões gerais? A estratégia é o **divisor recursivo por caractere**: comece com um separador grande, como uma quebra de linha dupla, e passe para separadores menores (espaço, depois caractere) até atingir o tamanho alvo. Isso mantém ideias vizinhas no mesmo chunk.

#### 4. Otimize para recuperação e relevância
Teste o tamanho do chunk com consultas reais, porque chunks pequenos demais carecem de contexto, e chunks grandes demais acrescentam ruído que pode confundir o LLM. A pergunta-chave: estou recuperando texto irrelevante que desperdiça a janela de contexto, ou deixando de encontrar a resposta? Se sim, faça experimentos. O benchmark do artigo executa o chunking em tamanhos diferentes (256, 512 e 1024 tokens) e pontua cada um com Hit Rate ou MRR (Mean Reciprocal Rank), para que você veja qual tamanho dá consistentemente as respostas mais precisas para a tarefa.

## 🧠 Modelos mentais
### Chunk como unidade de resposta
**O modelo:** um bom chunk é uma resposta autocontida a alguma pergunta possível, não uma fatia arbitrária de bytes.
**Como aplicar:** execute o teste de isolamento do artigo em chunks de exemplo. Se um chunk não faz sentido quando lido sozinho, a fronteira está errada.

### Buscar pequeno, devolver grande
**O modelo:** precisão de recuperação e contexto da resposta exigem tamanhos de chunk diferentes, então um mesmo índice deve armazenar os dois.
**Como aplicar:** indexe chunks filhos pequenos para correspondência, mantenha um parent_id em cada um e devolva o pai ao LLM.

### Sobreposição como ponte narrativa
**O modelo:** a sobreposição carrega texto de conexão através de uma fronteira, então uma frase ou ideia dividida continua compreensível de qualquer um dos lados.
**Como aplicar:** comece com uma sobreposição de 10 a 20 por cento entre chunks vizinhos e ajuste-a com base nos resultados de recuperação.

### Faça benchmark, não adivinhe
**O modelo:** o tamanho certo de chunk é um resultado empírico para os seus dados e consultas, não uma constante.
**Como aplicar:** teste tamanhos como 256, 512 e 1024 tokens com consultas reais, e compare Hit Rate e MRR para cada um.

## 🔑 Principais conclusões
- A tese central do artigo é que as fronteiras dos chunks determinam o que a recuperação consegue devolver, o que torna o chunking a principal alavanca da qualidade da saída do RAG.
- A divisão de tamanho fixo é a opção mais rápida, mas pode cortar palavras no meio e acrescentar ruído semântico à entrada do LLM.
- A divisão recursiva tenta fronteiras de parágrafo, de linha e de espaço antes de recorrer a caracteres, então as frases se mantêm intactas sempre que possível.
- A divisão sensível à estrutura usa os cabeçalhos Markdown para que seções distintas, como "The Physics" e "The Hardware", nunca se fundam em um só chunk.
- O chunking semântico começa um novo chunk quando a similaridade de cosseno entre embeddings de frases consecutivas cai abaixo de um limiar que precisa ser ajustado.
- A recuperação pai-filho busca nos pequenos chunks filhos e devolve o pai, para que o LLM receba o "porquê" além do "o quê".
- Escolha o tamanho do chunk fazendo benchmark de candidatos como 256, 512 e 1024 tokens com consultas reais, pontuados com Hit Rate ou MRR.

## 🏛️ Para Kenji
Esta é a sua interpretação, não a do autor. O chunking define ao mesmo tempo a granularidade da recuperação, o custo em tokens e o contexto da resposta, então pertence à revisão de arquitetura, não a um script de pré-processamento. A recuperação pai-filho é o padrão mais relevante para RAG empresarial, porque separa a unidade de indexação da unidade de contexto, o que importa para respostas governadas e citáveis. A pergunta que ela levanta para o seu trabalho: qual métrica, Hit Rate ou MRR, detectaria uma regressão de recuperação depois que seus documentos ou seu modelo de embeddings mudarem?

## 📚 Plano de leitura
### Do artigo
O artigo não cita links.

### Para se aprofundar
- Documentação dos text splitters do LangChain (as classes `CharacterTextSplitter`, `RecursiveCharacterTextSplitter` e `MarkdownHeaderTextSplitter`) — a referência para o comportamento e os parâmetros dos divisores usados nos exemplos.
- Documentação do sentence-transformers (`SentenceTransformer`, `all-MiniLM-L6-v2`) — o modelo de embeddings e a API de encoding por trás dos exemplos semântico e pai-filho.
- Hit Rate e Mean Reciprocal Rank como métricas de avaliação de recuperação — a base do benchmark de tamanho de chunk que o artigo recomenda.
- Pesquisas sobre "lost in the middle" sobre como modelos de linguagem usam contextos longos — o pano de fundo da motivação pai-filho.
