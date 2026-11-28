---
type: Article Summary
title: 'OKF + RAG: A arquitetura definitiva para agentes de IA'
description: Conhecimento curado em Markdown somado à busca RAG dá aos agentes fatos determinísticos e amplo alcance, evitando respostas alucinadas sobre dados críticos.
resource: https://medium.com/@ravishkhullar/okf-rag-the-ultimate-ai-agent-architecture-26b9ceed44f1
tags:
- ai-ml
- rag
- embedding
- semantic-search
- ai-agent
- large-language-model
- version-control-code-review
- hallucination
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-28T06:47:00-03:00'
sources:
- resource: https://medium.com/@ravishkhullar/okf-rag-the-ultimate-ai-agent-architecture-26b9ceed44f1
  title: 'OKF + RAG: The Ultimate AI Agent Architecture'
  author: Ravish Khullar
status: stable
source_url: https://medium.com/@ravishkhullar/okf-rag-the-ultimate-ai-agent-architecture-26b9ceed44f1
source: Medium save
source_type: medium
published: '2026-11-20'
domain: ai-ml
concepts:
- rag
- embedding
- semantic-search
- ai-agent
- large-language-model
- version-control-code-review
- hallucination
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/426baf74172d81dda71dfd3a5709695a
lang: pt-br
translation_of: /notes/ai-ml/2026-11-28-okf-rag-the-ultimate-ai-agent-architecture.md
---

## 🎯 Tese central
Agentes empresariais precisam de dois sistemas de memória: um conjunto curado e versionado de fatos autoritativos (OKF) para respostas exatas, e busca RAG sobre material não estruturado para alcance, com roteamento entre os dois.

## 🧭 Por que o autor escreveu
Agentes respondem bem a fatos bem conhecidos, mas inventam respostas com confiança sobre fatos específicos da empresa, como uma fórmula de churn, o esquema de uma API interna ou um runbook de failover de banco de dados. Isso impede que empresas confiem neles para trabalho real. Se a tese estiver certa, fatos exatos devem vir de uma fonte revisada e determinística, e a recuperação deve cobrir todo o resto.

## O problema e a proposta

### Alucinação
Alucinação é quando um modelo produz uma resposta que soa fluente e certa, mas está factualmente errada. Os exemplos do autor são fatos específicos e privados: a fórmula da taxa de churn de uma empresa, o esquema de sua API interna e o runbook de failover em produção. Um modelo não consegue conhecer esses fatos a partir do treinamento, então preenche a lacuna com texto plausível. O artigo chama a alucinação de "o maior obstáculo isolado que impede as empresas de confiarem em agentes de IA para trabalho real".

### Geração aumentada por recuperação (RAG)
Nos últimos dois anos, a resposta da indústria tem sido o RAG: conectar o modelo a um banco de dados vetorial de documentos da empresa, pesquisar nele e colar os resultados no prompt. O autor reconhece que o RAG funciona bem para grandes corpora não estruturados. Sua falha é ser probabilístico: ele encontra documentos semelhantes à pergunta, não documentos autoritativos. Para uma fórmula de receita ou um contrato de API de pagamentos, o autor diz que "semelhante" não basta.

### Open Knowledge Format (OKF)
Em junho de 2026, o Google Cloud apresentou o OKF como uma especificação aberta e neutra em relação a fornecedores para armazenar conhecimento curado e autoritativo como arquivos Markdown simples. A proposta do autor é combinar a precisão determinística do OKF com o alcance probabilístico do RAG. O restante do artigo explica cada metade e como elas se juntam.

## Os dois sistemas de memória
O artigo abre com uma analogia: um especialista humano que entra numa empresa no Dia 1 recebe duas coisas.

### A wiki da empresa (OKF)
Uma wiki interna curada e bem organizada, com definições canônicas, documentação de APIs, fórmulas de métricas e runbooks. Tudo nela foi revisado, aprovado e versionado. Quando o especialista precisa da resposta oficial, é aqui que ele procura.

### O mecanismo de busca para todo o resto (RAG)
Acesso a milhares de mensagens do Slack, threads de e-mail, chamados de suporte e transcrições de reuniões. Quando a pergunta é o que as pessoas disseram sobre um tema, ou a informação está enterrada em arquivos não estruturados, o especialista pesquisa aqui.

O ponto do autor é que nenhum dos dois sistemas basta sozinho; juntos, eles cobrem toda a gama do conhecimento organizacional.

*[Imagem: diagrama, sem legenda. Não capturada no texto de origem.]*

## O que é OKF? (Open Knowledge Format)
O OKF é deliberadamente simples. Um "pacote de conhecimento" OKF é um diretório de arquivos Markdown armazenado no Git. Cada arquivo descreve um conceito, como uma tabela de banco de dados, uma métrica, um endpoint de API ou um runbook, e começa com um bloco de frontmatter YAML com metadados legíveis por máquina. O artigo dá um exemplo, para uma métrica de negócio.

### Arquivo de pacote de conhecimento OKF
```markdown
---
type: metric
title: "Monthly Churn Rate"
description: "Official formula for calculating monthly customer churn."
owner: "data-engineering"
tags: [revenue, kpi, board-report]
timestamp: 2026-06-20T10:00:00Z
---

# Monthly Churn Rate

The official churn rate formula used in all board reports and investor decks:

    Churn Rate = (Customers Lost During Month / Customers at Start of Month) × 100

### Rules
- **Do NOT** use trial accounts in the denominator.
- **Do NOT** count plan downgrades as churn.
- Source of truth: `analytics.monthly_churn_summary` table.

### Related
- [Monthly Active Users](mau.md)
- [Revenue Dashboard](revenue_dashboard.md)
```

**O que isso faz:**
- O frontmatter dá às ferramentas metadados tipados (`type`, `title`, `description`, `owner`, `tags`, `timestamp`) que podem ser lidos sem analisar o texto corrido.
- O corpo reapresenta a fórmula como um cabeçalho e uma linha de fórmula, para que pessoas e o agente leiam a mesma definição.
- A seção `### Rules` registra exclusões que uma definição genérica de churn erraria: contas de teste ficam fora do denominador, e downgrades de plano não são churn.
- A linha `Source of truth` nomeia a tabela de onde vem a métrica, para que a definição aponte de volta para os dados.
- Os links de `### Related` usam caminhos de arquivo relativos, então o pacote forma um conjunto de arquivos navegáveis e interligados.

> 🔎 Contexto adicionado: YAML é um formato de texto simples para dados de chave-valor e listas. Um bloco de frontmatter é a seção de metadados no início de um arquivo, delimitada por linhas `---`.

### Por que isso importa
#### Sem matemática vetorial, sem alucinação
Quando o agente precisa da fórmula de churn, ele lê esse arquivo diretamente. Não há embedding, nem similaridade de cosseno, nem "top-k vizinhos mais próximos". Ele recebe a resposta exata e aprovada toda vez.

#### Nativo de GitOps
Como o pacote fica no Git, toda mudança passa por pull requests, revisões de código e histórico de commits. O resultado é uma trilha de auditoria de quem mudou a fórmula de churn, quando e por quê.

> 🔎 Contexto adicionado: o Git registra cada mudança como um commit com autor, carimbo de data e hora e mensagem. Pull requests acrescentam uma etapa de revisão antes de a mudança ser mesclada.

#### Zero infraestrutura
Sem banco de dados vetorial, sem pipeline de embeddings, sem estratégia de chunking e sem jobs de reindexação. O conhecimento são apenas arquivos em uma pasta.

## O que é RAG? (Retrieval-Augmented Generation)
O artigo apresenta o RAG como o padrão para conectar um LLM a grandes dados não estruturados. Ele tem seis etapas: ingerir documentos como PDFs, e-mails, páginas do Confluence e mensagens do Slack; fragmentá-los em segmentos menores; gerar um embedding de cada fragmento como vetor; armazenar os vetores num banco de dados vetorial como Pinecone, Weaviate ou pgvector; na hora da consulta, gerar o embedding da pergunta no mesmo espaço vetorial e recuperar os fragmentos mais semelhantes; e então aumentar o prompt do LLM com esses fragmentos para que ele possa responder.

### Fragmentação (chunking)
A fragmentação divide cada documento de origem em segmentos menores, para que cada um possa ser transformado em embedding e recuperado sozinho. O artigo a lista como a etapa 2 e não dá tamanho nem método de fragmento. Na comparação com o OKF, ela é tratada como um custo: a abordagem baseada em arquivos não precisa de nenhuma estratégia de chunking.

### Embedding
Um embedding é uma lista de números que representa o significado de um fragmento, de modo que o software possa comparar significados com um cálculo de distância simples. O artigo gera embeddings tanto dos fragmentos armazenados quanto da pergunta recebida, no mesmo espaço vetorial, o que torna a comparação possível.

> 🔎 Contexto adicionado: "espaço vetorial" é o sistema de coordenadas comum que todos os embeddings compartilham; a distância nesse espaço representa a proximidade de significado.

### Banco de dados vetorial
O banco de dados vetorial armazena os fragmentos com embeddings e devolve os mais próximos de uma consulta. O artigo cita Pinecone, Weaviate e pgvector como exemplos. Ele ordena os resultados por similaridade semântica, não por autoridade.

### Onde o RAG tem dificuldades
O RAG depende de matemática vetorial, então pode retornar texto relacionado à pergunta sem ser autoritativo. O exemplo do artigo são três threads do Slack que definem a taxa de churn de forma ligeiramente diferente; o RAG pode recuperar a errada. O RAG se destaca no alcance sobre milhares de documentos e tem dificuldade com a precisão.

> ❌ **Antipadrão — RAG como fonte da verdade para fatos exatos:** o agente responde a uma pergunta sobre fórmula, contrato ou runbook a partir dos fragmentos mais bem ranqueados. **Por que as pessoas fazem isso:** o artigo não diz; ele descreve o RAG como a resposta padrão dos últimos dois anos. **Consequência:** uma definição errada ou conflitante pode ser recuperada e apresentada como resposta, porque a recuperação não distingue texto autoritativo de texto apenas semelhante. **Correção:** armazene a definição aprovada no OKF e direcione as perguntas de fatos exatos para lá, mantendo o RAG para descoberta.

## A arquitetura híbrida: OKF + RAG
A arquitetura de agente definitiva usa um roteador inteligente que classifica cada consulta recebida e a direciona para o sistema de memória apropriado.

### Roteador inteligente
O roteador decide, consulta por consulta, se a resposta deve vir do OKF ou do RAG. O artigo descreve isso apenas como classificação e roteamento. O exemplo de código abaixo implementa uma ordem de fallback: tenta primeiro a fonte determinística e depois a probabilística.

### Como o roteador decide
A tabela de decisão do roteador do artigo é uma imagem, e seu conteúdo não foi capturado no texto de origem. O artigo não dá nenhuma regra além do código abaixo.

### Exemplo de implementação
O artigo traz um esboço simplificado em Python do roteador. É um esboço: a busca RAG é um placeholder, e a consulta ao OKF é baseada em palavras-chave.

```py
from openai import OpenAI
import os
import glob
import yaml

client = OpenAI()

# 1. Load the OKF knowledge bundle from the local directory
def load_okf_bundle(bundle_path: str) -> dict:
    """Reads all Markdown files in the OKF directory into a searchable dict."""
    knowledge = {}
    for filepath in glob.glob(f"{bundle_path}/**/*.md", recursive=True):
        with open(filepath) as f:
            content = f.read()
            # Extract the title from YAML frontmatter
            if content.startswith("---"):
                _, frontmatter, body = content.split("---", 2)
                meta = yaml.safe_load(frontmatter)
                title = meta.get("title", os.path.basename(filepath))
                knowledge[title.lower()] = body.strip()
    return knowledge

# 2. Search OKF (deterministic, keyword-based)
def search_okf(query: str, okf_knowledge: dict) -> str | None:
    """Simple keyword match against OKF titles."""
    for title, content in okf_knowledge.items():
        if title in query.lower():
            return content
    return None

# 3. Search RAG (probabilistic, vector-based)
def search_rag(query: str) -> str:
    """Placeholder for your vector DB search (Pinecone, Weaviate, etc.)."""
    # results = vector_db.similarity_search(query, top_k=5)
    return "RAG context: [retrieved chunks would appear here]"

# 4. The Intelligent Router
def answer_query(query: str, okf_bundle_path: str) -> str:
    okf_knowledge = load_okf_bundle(okf_bundle_path)
    
    # Try OKF first (deterministic path)
    okf_result = search_okf(query, okf_knowledge)
    
    if okf_result:
        context = f"[SOURCE: Official Knowledge Base (OKF)]\n{okf_result}"
    else:
        # Fall back to RAG (probabilistic path)
        context = f"[SOURCE: Document Search (RAG)]\n{search_rag(query)}"
    
    # Send to LLM with the retrieved context
    response = client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {"role": "system", "content": "Answer using ONLY the provided context."},
            {"role": "user", "content": f"Context:\n{context}\n\nQuestion: {query}"}
        ]
    )
    return response.choices[0].message.content
```

**O que isso faz:**
- `load_okf_bundle` percorre recursivamente o diretório do pacote, lê cada arquivo Markdown e indexa seu corpo pelo `title` do frontmatter em minúsculas, com o nome do arquivo como alternativa. O pacote é relido do disco a cada chamada.
- `search_okf` devolve um corpo somente quando um título armazenado aparece literalmente na consulta em minúsculas; assim, uma pergunta parafraseada não encontra correspondência e cai no RAG.
- `search_rag` é um placeholder. A linha comentada mostra a chamada pretendida, `similarity_search(query, top_k=5)`, que devolveria cinco resultados.
- `answer_query` tenta o OKF primeiro e rotula o contexto com sua origem, `[SOURCE: Official Knowledge Base (OKF)]` ou `[SOURCE: Document Search (RAG)]`, para que o leitor veja qual memória respondeu.
- A chamada ao LLM usa `gpt-4o` com uma mensagem de sistema que manda responder usando apenas o contexto fornecido.

*[Imagem: diagrama, sem legenda. Não capturada no texto de origem.]*

## Conclusão
A conclusão do artigo é que o futuro dos agentes empresariais é híbrido, não uma escolha entre OKF e RAG.

### Precisão e alcance juntos
O OKF dá ao agente uma memória curada e à prova de alucinação para os fatos que mais importam: KPIs, contratos de API e runbooks operacionais. O RAG permite explorar grandes acervos de conhecimento não estruturado que nenhuma pessoa conseguiria organizar à mão. O resultado combinado é uma base de conhecimento determinística, controlada pelo Git, somada à busca vetorial probabilística, de modo que o agente seja preciso quando precisa e exploratório quando precisa descobrir.

## 🧠 Modelos mentais
### Especialista do primeiro dia com dois recursos
**O modelo:** Um recém-contratado recebe uma wiki curada da empresa para respostas oficiais e um mecanismo de busca para todo o resto. Nenhum dos dois, sozinho, cobre o conhecimento da organização.
**Como aplicar:** Ao projetar um agente, decida quais perguntas precisam da resposta aprovada e quais precisam de descoberta, e dê a cada uma sua própria fonte.

### Determinístico primeiro, probabilístico como alternativa
**O modelo:** Consulte primeiro a fonte da verdade, e use a recuperação aproximada apenas quando ela não tiver resposta.
**Como aplicar:** Direcione cada consulta para uma busca exata, e chame a busca por similaridade apenas quando a busca exata falhar, como faz o código do roteador.

### Semelhante não é autoritativo
**O modelo:** A similaridade semântica encontra documentos relacionados, não documentos autoritativos.
**Como aplicar:** Para fatos de missão crítica, exija uma fonte aprovada e versionada, e nunca deixe o fragmento mais bem ranqueado ocupar o lugar dela.

## 🔑 Principais conclusões
- Alucinação sobre fatos específicos da empresa, como KPIs, contratos de API e runbooks, é o que impede as empresas de confiarem em agentes para trabalho real.
- O RAG recupera documentos semanticamente semelhantes a uma pergunta, o que não é o mesmo que recuperar documentos autoritativos.
- O OKF armazena conhecimento curado como um diretório de arquivos Markdown no Git, um conceito por arquivo, cada um com frontmatter YAML.
- O histórico do Git dá ao OKF uma trilha de auditoria de cada mudança: quem mudou uma fórmula, quando e por quê.
- O roteador tenta primeiro a consulta ao OKF e recorre à busca vetorial como alternativa, e o LLM recebe a instrução de responder usando apenas o contexto que recebe.
- A infraestrutura é o compromisso que o autor enfatiza: a fonte baseada em arquivos não precisa de banco de dados vetorial, pipeline de embeddings, chunking nem reindexação.
- Use os dois: OKF para fatos exatos e revisados, e RAG para explorar acervos não estruturados.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. A governança de arquitetura pode tratar o conhecimento que um agente pode citar como uma fonte autoritativa, com um responsável, uma etapa de revisão e um histórico de mudanças, junto com a governança de dados e a catalogação de metadados. A pergunta que isso levanta é: quais fatos da sua organização um agente precisaria como registros revisados e versionados, e não como texto recuperável, e quem seria o responsável por eles?

## 📚 Plano de leitura
### Do artigo
- O artigo não cita links externos.

### Para aprofundar
- Especificação Open Knowledge Format (OKF) do Google Cloud, a fonte do formato descrito aqui; confira o original para detalhes que o artigo resume.
- Bancos de dados vetoriais e estratégias de chunking, já que o lado RAG do artigo é apenas um placeholder em seu código e o desenho real da recuperação fica em aberto.
- Governança de dados e catálogos de metadados, para decidir quem aprova os fatos que um agente pode tratar como autoritativos.

## Conceitos
- [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)
- [Embedding](/pt-br/concepts/ai-ml/embedding.md)
- [Busca semântica](/pt-br/concepts/ai-ml/semantic-search.md)
- [Agente de IA](/pt-br/concepts/ai-ml/ai-agent.md)
- [Modelo de linguagem de grande porte e modelo de fundação](/pt-br/concepts/ai-ml/large-language-model.md)
- [Controle de versão e revisão de código](/pt-br/concepts/software-engineering/version-control-code-review.md)
- [Alucinação](/pt-br/concepts/ai-ml/hallucination.md)
