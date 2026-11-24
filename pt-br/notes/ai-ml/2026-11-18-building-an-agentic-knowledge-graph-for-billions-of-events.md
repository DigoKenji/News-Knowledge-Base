---
type: Article Summary
title: Construindo um Grafo de Conhecimento Agêntico para Bilhões de Eventos
description: Observabilidade orientada por ontologia conecta ferramentas de incidente fragmentadas em um único grafo de conhecimento; a reconstrução com dados públicos funciona, mas os detectores continuam sendo o elo mais fraco.
resource: https://medium.com/gitconnected/building-an-agentic-knowledge-graph-for-billions-of-events-9348cf08bc50
tags:
- ai-ml
- ai-agents
- agent-harness
- structured-decision-model
- calibration
- knowledge-graph
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://medium.com/gitconnected/building-an-agentic-knowledge-graph-for-billions-of-events-9348cf08bc50
  title: Building an Agentic Knowledge Graph for Billions of Events
  author: Level Up Coding
status: stable
source_url: https://medium.com/gitconnected/building-an-agentic-knowledge-graph-for-billions-of-events-9348cf08bc50
source: Medium (Level Up Coding)
source_type: medium
published: ''
domain: ai-ml
subdomain: ai-agents
concepts:
- agent-harness
- structured-decision-model
- calibration
- knowledge-graph
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8187afc8d14b535c41c2
lang: pt-br
translation_of: /notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md
---

## 🎯 Tese central
Conectar ferramentas de incidente por meio de um único grafo de conhecimento compartilhado e validado permite que um investigador responda "o que mudou, onde e quando" em um só lugar; a reconstrução com dados públicos feita pelo autor mostra que a abordagem funciona, mas que os detectores continuam sendo o elo mais fraco.

## 🧭 Por que o autor escreveu o texto
Quando Mike Tyson lutou contra Jake Paul em novembro de 2024, a Netflix entregou a luta a cerca de 65 milhões de streams simultâneos, registrando mais de 38 milhões de eventos por segundo. Quando a página inicial foi renderizada 1.5 segundo mais devagar, o alerta disparou, mas, no exemplo da própria Netflix, a correção levou cerca de quatro horas, gastas sobretudo na reconstrução de fatos que as ferramentas já conheciam, porque uma ferramenta dizia "application" e outra dizia "service". Se a tese estiver certa, um único acordo compartilhado sobre nomes, fatos e tempo transforma essa reconstrução em uma consulta.

## Reconstruindo a observabilidade no estilo da Netflix com IA
O autor reconstrói o design da Netflix com dados públicos, sem nenhuma afiliação com a Netflix. A reconstrução tem sete partes: um lago de telemetria com 1,922,290,480 linhas reais em 15.24 GB de Parquet; identidade e uma ontologia em que as formas recusam fatos inválidos; um armazenamento de proveniência com um grafo nomeado por execução; enriquecedores e uma escada de extração que registra a cobertura; uma correlação que segue o grafo de chamadas; agentes de causa raiz com ferramentas tipadas, um verificador de evidências e um reconciliador; e um exame mais um replay de stream. O resultado final é que as cadeias pelo grafo de chamadas colocam o componente quebrado entre os três primeiros em 26.8% das vezes, e um único laptop sustenta 26.1 milhões de eventos por segundo.

O layout do projeto é:

```text
ontology-observability/
├── ontology/
│   ├── ops.ttl        # classes, properties and ranges
│   └── shapes.ttl     # the rules every fact must pass
└── kg/
    ├── config.py      # settings, models and budgets
    ├── data.py        # checked Parquet
    ├── identity.py    # one name per machine
    ├── ontology.py    # vocabulary and shape checks
    ├── store.py       # named graphs and provenance
    ├── enrich.py      # the enricher contract
    ├── llm.py         # the model client
    ├── ladder.py      # the closed-vocabulary rung
    ├── signals.py     # components and topology
    ├── correlate.py   # chains and rankings
    ├── agents.py      # specialists and the verifier
    ├── exam.py        # agreement and exact tests
    └── replay.py      # the stream replay
```

**O que isso faz:**
- Divide o projeto em uma pasta `ontology/` com arquivos Turtle versionados como dados, e uma pasta `kg/` com módulos Python, um por etapa do pipeline.
- Mantém o vocabulário (`ops.ttl`) e suas regras (`shapes.ttl`) separados do código que as aplica.

## Lacuna de contexto do incidente
Em 17 de abril de 2024, às 09:18 UTC, o MediaWiki em Kubernetes no datacenter eqiad da Wikimedia começou a falhar. Uma mudança havia movido o proxy mcrouter para um serviço próprio, e as consultas DNS subiram de cerca de 40,000 por segundo para 110,000. A análise publicada, 1.3 dias depois, atribuiu a culpa a um nome de host sem o ponto final.

Cada ferramenta guardava um pedaço dessa história em seus próprios nomes. Uma máquina de banco aparece como `db1127.eqiad.wmnet:3306` em um alerta, como "o primário do s7" no chat e como `db1127` no log de deploy. Em 89 cronologias escritas, o incidente mediano da Wikimedia foi percebido em 2 minutos e corrigido em 40, mas só foi compreendido após 62 minutos.

Como a telemetria da Netflix é privada, o autor usa registros públicos do mesmo formato: o chat de plantão da Wikimedia, o registro de mudanças, os alertas e os relatórios de incidente, que são as quatro fontes que a Netflix une. OpenRCA e RCAEval acrescentam métricas, logs e traces com gabaritos, e as visualizações da Wikipedia acrescentam volume. Antes de olhar qualquer dado, o autor listou doze afirmações testáveis sobre o design da Netflix, e as seções finais as testam uma a uma.

## Configuração do ambiente
A build precisa de Python 3.12 e cerca de 25 GB de disco. A execução completa usou um laptop de 16 núcleos com 32 GB de memória. O projeto é criado com uv, e todos os downloads ficam dentro dele.

```bash
uv init ontology-observability --python 3.12 && cd ontology-observability && mkdir -p kg ontology
uv add duckdb pandas pyarrow rdflib pyshacl pyoxigraph httpx huggingface-hub scipy psutil
export KG_CONTACT="you@example.org"              # Wikimedia asks every script for a contact
export LLM_BASE_URL="https://your-endpoint/v1"   # any OpenAI-compatible server
export LLM_API_KEY="your key"
```

**O que isso faz:**
- Cria o projeto com Python 3.12 e instala as bibliotecas DuckDB, RDF, SHACL, Oxigraph e HTTP usadas no restante da nota.
- Define a string de contato que a Wikimedia pede, mais um endpoint compatível com OpenAI e a chave lidos do ambiente, para que nenhum segredo seja escrito no código.

Todo módulo lê `config.py`, que contém os caminhos, os limites do DuckDB, um modelo por papel e um orçamento de créditos por fase.

```python
"""config.py: the settings every other file reads."""
import os
from pathlib import Path

PROFILE = os.environ.get("KG_PROFILE", "FULL")             # SMOKE takes a thin slice of every source
DATA = Path(os.environ.get("KG_DATA", "data"))             # about 25 GB once everything is in
CONTACT = os.environ.get("KG_CONTACT", "")                 # Wikimedia asks every script for a contact
THREADS, MEMORY, SEED = 16, "18GB", 42                     # a 16-core laptop with 31 GiB of memory
BASE_URL = os.environ.get("LLM_BASE_URL", "http://127.0.0.1:8000/v1")    # any OpenAI-compatible server
API_KEY = os.environ.get("LLM_API_KEY", "")                # read once, never printed
MODELS = {"flash": "Qwen/Qwen3.8-Flash-Next", "max": "Qwen/Qwen3.8-Max", "reader": "Qwen/Qwen3.7-Max"}
PRICE = {"Qwen/Qwen3.8-Flash-Next": 0.2, "Qwen/Qwen3.8-Max": 0.5, "Qwen/Qwen3.7-Max": 0.5}    # credits a call
BUDGET = {"label extraction": 200, "chat classification": 60, "AutoRCA development": 140, "Wikimedia test": 330,
          "OpenRCA test": 250, "experiments": 185, "reserve": 35}                             # 1,200 in all
```

**O que isso faz:**
- Lê o perfil, os caminhos, o contato e o endpoint de variáveis de ambiente com valores padrão seguros, para que um único arquivo sirva tanto para um teste de fumaça quanto para a execução completa.
- Nomeia três modelos por papel (flash, max, reader), com um preço em créditos por chamada usado na verificação do orçamento.
- Limita o gasto por fase a 1,200 créditos no total, com uma reserva de 35 créditos.

A build depende destas ferramentas, e as importações e os detalhes da máquina são verificados com um pequeno script:

```python
import platform

import duckdb        # SQL over Parquet, without loading it into memory
import pyoxigraph    # the triple store, written in Rust
import pyshacl       # SHACL shapes, checked in Python
import rdflib        # RDF graphs and Turtle files
import psutil        # cores and memory of this machine

print(f"  python {platform.python_version()}, duckdb {duckdb.__version__}, rdflib {rdflib.__version__}")
print(f"  pyshacl {pyshacl.__version__}, pyoxigraph {pyoxigraph.__version__}, {psutil.cpu_count()} cores")

#### OUTPUT ####
  python 3.12.14, duckdb 2.0.0, rdflib 7.6.0
  pyshacl 0.40.2, pyoxigraph 0.5.13, 16 cores
```

**O que isso faz:**
- Importa o DuckDB para SQL sobre Parquet, o Oxigraph para o armazenamento de triplas, o pyshacl para a validação SHACL, o rdflib para grafos RDF e o psutil para o relatório de hardware.
- Imprime as versões e a contagem de núcleos, para que cada tempo da nota possa ser rastreado a esta máquina e a este conjunto exato de software.

## Lago de telemetria
Uma coleta cuidadosa, com pseudônimos para cada pessoa, retornou 1,730,378 linhas de chat, 538,717 mudanças registradas e 414 relatórios de incidente. O espelho do OpenRCA contém 162 arquivos CSV totalizando 68.65 GB, vindos de um banco, uma operadora de telecomunicações e uma loja, e o autor os converte um por um.

A conexão DuckDB usa todos os núcleos e transborda para o disco:

```python
"""data.py: one DuckDB connection, and text turned into checked Parquet."""
from pathlib import Path

import duckdb

from kg.config import DATA, MEMORY, THREADS


def duck() -> duckdb.DuckDBPyConnection:
    # every core, a memory cap, and a spill folder on disk
    return duckdb.connect(config={"threads": THREADS, "memory_limit": MEMORY, "temp_directory": str(DATA / "spill"),
                                  "preserve_insertion_order": False, "TimeZone": "UTC"})
```

**O que isso faz:**
- Abre uma conexão DuckDB com todas as 16 threads, um limite de memória e uma pasta de transbordamento em disco, para que dados maiores que a memória ainda sejam processados.
- Define o fuso horário da sessão como UTC, para que os timestamps de fontes diferentes se alinhem.

Três sistemas usam três layouts de span e três relógios diferentes, então cada um recebe seu próprio select. O CSV só é apagado depois que a contagem de linhas bate:

```python
# kg/data.py
SPANS = {   # one trace layout, three systems, three clocks
    "bank": 'to_timestamp(TRY_CAST("timestamp" AS BIGINT) / 1000) AS ts, cmdb_id AS entity, trace_id, parent_id, '
            "TRY_CAST(duration AS DOUBLE) AS duration_ms",
    "telecom": "to_timestamp(TRY_CAST(startTime AS BIGINT) / 1000) AS ts, cmdb_id AS entity, traceId AS trace_id, "
               "pid AS parent_id, TRY_CAST(elapsedTime AS DOUBLE) AS duration_ms",
    "market": 'to_timestamp(TRY_CAST("timestamp" AS BIGINT) / 1000) AS ts, cmdb_id AS entity, trace_id, '
              "parent_span AS parent_id, TRY_CAST(duration AS DOUBLE) / 1000 AS duration_ms",    # microseconds
}


def to_parquet(csv: Path, select: str, out: Path, con) -> int:
    """CSV in, zstd Parquet out, rows counted, then the CSV deleted."""
    tmp = out.with_suffix(".tmp")
    out.parent.mkdir(parents=True, exist_ok=True)
    con.execute(f"CREATE OR REPLACE TEMP VIEW src AS SELECT * FROM read_csv('{csv.as_posix()}', all_varchar = true)")
    rows = con.execute(f"COPY (SELECT {select} FROM src) TO '{tmp.as_posix()}' "
                       "(FORMAT parquet, COMPRESSION zstd, ROW_GROUP_SIZE 1000000)").fetchone()[0]
    assert rows == con.execute("SELECT count(*) FROM src").fetchone()[0], f"rows lost in {csv.name}"
    tmp.replace(out)                                       # only a checked file gets its real name
    csv.unlink()                                           # so 68 GB of CSV never sits on disk at once
    return rows
```

**O que isso faz:**
- Mapeia os nomes de colunas de cada sistema para um layout de trace comum, incluindo as unidades de timestamp próprias; o sistema de mercado informa microssegundos, então o valor é dividido por 1000.
- Grava em um arquivo temporário, verifica se a contagem de linhas é igual à da fonte e só então renomeia o arquivo, para que um Parquet pela metade nunca receba um nome definitivo.
- Apaga o CSV somente depois que a verificação passa, o que evita que cerca de 68 GB de CSV fiquem no disco ao mesmo tempo.

Um dia do exemplo em execução é convertido de ponta a ponta:

```python
from pathlib import Path
from huggingface_hub import hf_hub_download
from kg.data import SPANS, duck, to_parquet

path = "Market/cloudbed-1/telemetry/2022_03_21/trace/trace_span.csv"      # one of the 162 files
csv = Path(hf_hub_download("tracer-cloud/opensre", path, repo_type="dataset", local_dir="data/raw/openrca"))
out = Path("data/parquet/openrca/market_cb1/spans/2022_03_21__trace_span.parquet")
print(f"  {to_parquet(csv, SPANS['market'], out, duck()):,} spans, CSV deleted: {not csv.exists()}")

#### OUTPUT ####
  9,027,465 spans, CSV deleted: True
```

**O que isso faz:**
- Baixa um arquivo de trace do dataset, converte-o para Parquet com o layout de mercado e imprime a contagem de spans.
- A verificação impressa confirma que o CSV foi apagado depois que as linhas foram contadas.

Ao longo dos 162 arquivos, 68.65 GB foram reduzidos para 7.11 GB sem perda de linhas. Os pageviews da Wikimedia e o RCAEval completam o lago:

```text
#### OUTPUT ####
  Wikimedia  operations record              4,609,863    0.16 GB
  Wikimedia  pageviews, page x hour     1,142,762,478    4.64 GB
  OpenRCA    metrics, logs, spans         524,547,719    7.11 GB
  RCAEval    metrics, logs, spans         250,370,420    3.32 GB
  total                                 1,922,290,480   15.24 GB
```

**O que isso faz:**
- Lista as quatro partes do lago por fonte, com contagens de linhas e tamanhos; o total é 1,922,290,480 linhas em 15.24 GB.
- Os pageviews da Wikipedia sozinhos somam 1,142,762,478 linhas, a maior parte do volume.

A falha que se segue a partir daqui é o cartservice às 07:45 UTC, que aparece em métricas, traces e logs. O DuckDB leva menos de seis segundos para varrer todos os 397 milhões de spans.

## Resolvedor de identidade
Antes que dois fatos possam ser unidos, uma máquina precisa de um único nome. Na Wikimedia, o primeiro dígito do número de um host indica o datacenter, então o código de identidade usa esse dígito para derivar a localização.

```python
"""identity.py: one name for every machine, however a tool happens to write it."""
import re

DC_BY_DIGIT = {"1": "eqiad", "2": "codfw", "3": "esams", "4": "ulsfo", "5": "eqsin", "6": "drmrs", "7": "magru"}
HOSTNAME = re.compile(r"(?<![\w.-])([a-z][a-z-]*\d{4}(?:-dev)?)(?:\.(?:mgmt\.)?(?:eqiad|codfw|esams|ulsfo|eqsin|drmrs"
                      r"|magru)\.wmnet)?(?::\d+)?(?![\w-])")


def canonical_host(token: str) -> tuple[str, str] | None:
    """(name, datacentre) for any spelling of a production host, or None for anything else."""
    m = HOSTNAME.search((token or "").strip().lower())
    if not m:
        return None                                        # 'T316186' is a task, 'php8.3' a version
    dc = DC_BY_DIGIT.get(re.search(r"(\d)\d{3}(?:-dev)?$", m.group(1)).group(1))   # first digit = datacentre
    return (m.group(1), dc) if dc else None
```

**O que isso faz:**
- Mapeia o primeiro dígito do número do host para um datacenter, como 1 para eqiad, o que dá a cada host uma localização sem consulta.
- Usa uma expressão regular para remover os sufixos, as portas e os prefixos de gerenciamento que ferramentas diferentes acrescentam, e então devolve um nome canônico.
- Não retorna nada para strings que apenas parecem hosts, como um id de tarefa ou um número de versão, para que nunca entrem no grafo.

`DB1127.eqiad.wmnet:3306` e `elastic1124.mgmt.eqiad.wmnet` resolvem para hosts em eqiad, enquanto o id de tarefa `T316186` não resolve para nada. O catálogo então atribui a cada host um papel, uma equipe e um cluster, e o tempo é o elo fraco. Dos 5,593 hosts em cinco anos de logs, apenas 44% batem com o catálogo hoje. Ele conhece 81% dos hosts de 2026, mas apenas 18% dos de 2021, porque as máquinas são desativadas.

## Ontologia como código
Cada fato que o armazenamento contém é uma **tripla**, como "este deploy aconteceu em db1127", e um único arquivo decide quais triplas podem existir. A Netflix divide esse vocabulário em doze namespaces, e o autor acrescenta dois. Nada é apenas um `ops:Component`, e todo tipo de sinal fica abaixo de `corr:Signal`. O vocabulário é um arquivo Turtle no git, onde, por exemplo, `ops:Host` é um tipo de `ops:Component`. O vocabulário completo tem 272 triplas, 43 classes e 77 propriedades.

O módulo que carrega o vocabulário e as formas é:

```python
"""ontology.py: the vocabulary and its rules, kept in files and tested like code."""
import hashlib
import re
from pathlib import Path

import pandas as pd
import rdflib
from pyshacl import validate
from rdflib import RDF, Namespace, URIRef
from rdflib.collection import Collection
from rdflib.namespace import SH

FILES = Path(__file__).resolve().parent.parent / "ontology"          # ops.ttl and shapes.ttl
BASE = "https://kg.example.org/"
NS = {"ops": "ops#", "slack": "slack#", "alert": "alert#", "metric": "metric#", "log": "log#", "tl": "timeline#",
      "incident": "incident#", "rca": "rca#", "exp": "experiment#", "e2e": "e2e#", "corr": "correlation#",
      "harvest": "harvest#", "code": "code#", "users": "users#"}        # twelve from Netflix's post, two ours
OPS, CORR, TL, ALERT, HARVEST, SLACK = (Namespace(BASE + NS[p]) for p in ("ops", "corr", "tl", "alert", "harvest", "slack"))
PREFIXES = "".join(f"PREFIX {p}: <{BASE}{s}>\n" for p, s in NS.items()) + \
    "PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>\nPREFIX xsd: <http://www.w3.org/2001/XMLSchema#>\n"
TBOX = rdflib.Graph().parse(FILES / "ops.ttl")               # classes, properties, ranges
SHAPES = rdflib.Graph().parse(FILES / "shapes.ttl")          # what data must pass to be written
```

**O que isso faz:**
- Carrega o arquivo de classes e propriedades (`ops.ttl`) e o arquivo de regras (`shapes.ttl`) uma única vez, na importação, para que todos os módulos compartilhem uma só definição.
- Define um namespace por fonte (alertas, métricas, logs, timelines, incidentes, correlação e assim por diante), correspondendo aos doze da Netflix mais os dois do autor.

Uma única função cria cada URI, e uma segunda mapeia um momento para o seu nó de hora:

```python
# kg/ontology.py
def slug(text: str) -> str:
    s = re.sub(r"[^a-z0-9._~-]+", "-", str(text).strip().lower()).strip("-")
    return s or hashlib.sha256(str(text).encode()).hexdigest()[:12]      # a name made only of symbols


def mint(tenant: str, kind: str, key: str) -> URIRef:
    # one function names every thing, so the same thing always gets the same URI
    return URIRef(f"{BASE}id/{slug(tenant)}/{slug(kind)}/{slug(key)}")


def hour_node(ts) -> URIRef:
    # the hour as a node of its own turns a time window into a lookup, not a scan
    return URIRef(f"{BASE}id/hour/{pd.Timestamp(ts).tz_convert('UTC'):%Y-%m-%dT%H}")
```

**O que isso faz:**
- `slug` deixa minúsculo e limpa qualquer texto, então `DB1127` com um espaço perdido e `db1127` produzem a mesma URI.
- `mint` constrói cada URI a partir de tenant, tipo e chave, então a mesma coisa sempre recebe o mesmo identificador.
- `hour_node` transforma cada hora em um nó próprio, o que faz de "o que mudou nesta janela?" uma busca, e não uma varredura.

## Camada de validação de formas
Uma ontologia diz o que pode existir, mas não consegue impedir dados ruins. As **formas SHACL** conseguem, atuando como regras de mundo fechado que um fato precisa passar antes de ser armazenado. O exemplo do autor é uma confiança armazenada como o texto "0.92". Isso quebra um filtro em dois motores: o rdflib retorna todas as três linhas, até a da palavra "high", enquanto o Oxigraph não retorna nenhuma, e nenhum dos dois reporta erro. A correção pertence ao momento da escrita. O arquivo de regras tem dezesseis regras, e a forma de cadeia exige que todo `corr:confidence` seja um `xsd:decimal` entre 0 e 1.

> ❌ **Anti-padrão — inferência RDFS no validador:** o primeiro validador rodou com a inferência RDFS habilitada, então concluiu que um host com nome errado devia ser um componente. **Por que as pessoas fazem isso:** a inferência parece completude gratuita. **Consequência:** nomes ruins viram fatos tipados que parecem válidos. **Correção:** validar com `inference="none"` e julgar apenas o que os dados realmente dizem.

O verificador julga apenas os nós que um novo delta escreve, com o contexto armazenado deles, e não infere nada:

```python
# kg/ontology.py
def check(delta: rdflib.Graph, context: rdflib.Graph | None = None) -> tuple[bool, list[tuple]]:
    """Judge the nodes a delta writes, with their stored context. Subclasses count, nothing is inferred."""
    focus = sorted({s for s in delta.subjects() if isinstance(s, URIRef)})
    ok, report, _ = validate(delta + (context or rdflib.Graph()), shacl_graph=SHAPES, ont_graph=TBOX,
                             inference="none", focus_nodes=focus or None)
    rows = []
    for r in report.subjects(RDF.type, SH.ValidationResult):
        rule = str(report.value(r, SH.sourceConstraintComponent)).rsplit("#", 1)[-1].removesuffix("ConstraintComponent")
        path = report.value(r, SH.resultPath)
        rows.append((str(report.value(r, SH.focusNode)).rsplit("/", 1)[-1], path.n3(TBOX.namespace_manager)
                     if path is not None else "", rule, str(report.value(r, SH.resultMessage) or "")))
    return ok, sorted(rows)
```

**O que isso faz:**
- Executa o pyshacl com a inferência desligada, para que as formas julguem exatamente os fatos que foram escritos.
- Limita a verificação aos sujeitos do novo delta e acrescenta os fatos já armazenados sobre eles como contexto.
- Transforma cada violação em uma linha com o nó, a propriedade, a regra que falhou e a mensagem, ordenadas para um relatório estável.

O autor plantou seis nós defeituosos em um arquivo pequeno e executou a verificação:

```python
import rdflib
from kg.ontology import check

ok, rows = check(rdflib.Graph().parse("planted.ttl"))     # six broken nodes, two good ones
for node, path, rule, why in rows[:4]:
    print(f"  {node:<7}{path:<17}{rule:<13}{why}")

#### OUTPUT ####
  chain1 corr:confidence  Datatype     a confidence is a number between 0 and 1, never text
  chain1 corr:confidence  MaxInclusive a confidence is a number between 0 and 1, never text
  chain1 corr:confidence  MinInclusive a confidence is a number between 0 and 1, never text
  cov1   harvest:status   In           a harvest is ok, partial or missing, nothing else
```

**O que isso faz:**
- Carrega um arquivo com seis nós deliberadamente defeituosos e dois corretos, e depois imprime as quatro primeiras violações.
- Cada linha indica o nó com defeito, a propriedade, a regra (Datatype, MaxInclusive, In e assim por diante) e a mensagem da forma.

Todas as oito violações são detectadas.

## Armazenamento de proveniência
De onde veio um fato, e quando ele se tornou conhecido? Cada fato fica no **grafo nomeado** da execução que o escreveu, e um registro PROV-O do W3C descreve cada execução. O armazenamento é o Oxigraph, com a árvore de classes em um grafo próprio, para que as consultas de subclasse possam seguir a hierarquia.

```python
"""store.py: Oxigraph quads, one named graph per run, every run described in PROV-O."""
import pandas as pd
import pyoxigraph as ox
import rdflib
from rdflib import RDF, XSD, Literal, Namespace, URIRef

from kg.ontology import BASE, HARVEST, OPS, PREFIXES, TBOX, mint, slug

PROV = Namespace("http://www.w3.org/ns/prov#")
META, TBOX_GRAPH = URIRef(f"{BASE}graph/meta"), URIRef(f"{BASE}graph/tbox")   # runs described, classes


def dt_literal(ts) -> Literal:
    return Literal(pd.Timestamp(ts).tz_convert("UTC").strftime("%Y-%m-%dT%H:%M:%SZ"), datatype=XSD.dateTime)


class GraphStore:
    """Facts live in the named graph of the run that wrote them."""

    def __init__(self, path: str | None = None):
        self.store = ox.Store(path) if path else ox.Store()        # on disk for real, in memory for tests
        self._load(TBOX, TBOX_GRAPH)                               # the class tree, so subClassOf* can be followed

    def _load(self, g: rdflib.Graph, into: URIRef) -> None:
        self.store.load(g.serialize(format="nt").encode(), format=ox.RdfFormat.N_TRIPLES, to_graph=ox.NamedNode(into))
```

**O que isso faz:**
- Usa o Oxigraph como armazenamento de quads em disco ou em memória, para que cada fato seja guardado junto com o grafo de onde veio.
- Carrega a árvore de classes em seu próprio grafo nomeado na inicialização, para que consultas como "subclasse de Signal" funcionem.
- Grava datetimes em um único formato e mantém o namespace PROV em um só lugar.

Gravar uma execução novamente a substitui, então as reconstruções nunca duplicam:

```python
# kg/store.py
    def write(self, facts: rdflib.Graph, tenant: str, enricher: str, run: str, used: list[str], started) -> URIRef:
        g = URIRef(f"{BASE}graph/{slug(tenant)}/{slug(enricher)}/{slug(run)}")
        if self.store.contains_named_graph(ox.NamedNode(g)):
            self.drop(g)                                   # a rerun replaces, never duplicates
        self._load(facts, g)
        rid, meta = URIRef(g.replace("/graph/", "/id/run/")), rdflib.Graph()
        for p, o in ((RDF.type, HARVEST.Run), (HARVEST.enricher, Literal(enricher)), (PROV.startedAtTime,
                     dt_literal(started)), (PROV.endedAtTime, dt_literal(pd.Timestamp.now(tz="UTC"))),
                     (HARVEST.triplesAdded, Literal(len(facts), datatype=XSD.integer)),
                     *((PROV.used, URIRef(u)) for u in used)):           # the sources this run read
            meta.add((rid, p, o))
        meta.add((g, PROV.wasGeneratedBy, rid))
        meta.add((g, OPS.tenant, mint(tenant, "organisation", tenant)))
        self._load(meta, META)
        return g
```

**O que isso faz:**
- Dá a cada execução um grafo nomeado, identificado por tenant, enriquecedor e id da execução, para que uma reexecução encontre e substitua o próprio grafo.
- Grava um registro PROV-O com os horários de início e fim, o número de triplas adicionadas e as fontes que a execução leu.
- Liga o grafo ao seu registro de execução, para que a proveniência sobreviva no armazenamento ao lado dos fatos.

Remover uma execução ruim apaga seus fatos e seu registro juntos. `context` reúne os fatos armazenados ao redor de um delta:

```python
# kg/store.py
    def drop(self, g: URIRef) -> None:
        # its facts and its PROV-O record, nothing else
        self.store.remove_graph(ox.NamedNode(g))
        self.store.update(f"DELETE WHERE {{ GRAPH <{META}> {{ <{g}> <{PROV.wasGeneratedBy}> ?run . ?run ?p ?o }} }}")
        self.store.update(f"DELETE WHERE {{ GRAPH <{META}> {{ <{g}> ?p ?o }} }}")

    def ask(self, sparql: str, graphs: list | None = None) -> list:
        # a caller may name the only graphs it is allowed to read
        kw = {"default_graph": [ox.NamedNode(g) for g in graphs]} if graphs else {"use_default_graph_as_union": True}
        return list(self.store.query(PREFIXES + f"PREFIX prov: <{PROV}>\n" + sparql, **kw))

    def context(self, delta: rdflib.Graph) -> rdflib.Graph:
        # what the store already says about a delta's nodes
        nodes = " ".join(sorted({f"<{n}>" for s, _, o in delta for n in (s, o) if str(n).startswith(BASE + "id/")}))
        q = f"CONSTRUCT {{ ?s ?p ?o . ?o a ?c }} WHERE {{ VALUES ?s {{ {nodes} }} ?s ?p ?o OPTIONAL {{ ?o a ?c }} }}"
        nt = self.store.query(q, use_default_graph_as_union=True).serialize(format=ox.RdfFormat.N_TRIPLES) if nodes else b""
        return rdflib.Graph().parse(data=nt, format="nt")
```

**O que isso faz:**
- `drop` remove os fatos da execução e o seu registro PROV-O, sem deixar nada para trás.
- `ask` permite que quem chama nomeie os únicos grafos que pode ler, então um agente pode ser restrito a certas execuções.
- `context` reúne o que o armazenamento já diz sobre os nós de um novo delta, o que a verificação de formas precisa.

Cada sinal registra quando aconteceu e quando o sistema poderia tê-lo conhecido. Um agente às 09:33 não pode ver um relatório escrito no dia seguinte:

```python
# kg/store.py
AS_OF = """SELECT ?s WHERE {{ ?s a/rdfs:subClassOf* corr:Signal . ?s corr:at ?t . ?s corr:detectedAt ?d .
  FILTER(?d <= {asof} && ?t >= {since}) }}"""


def visible(store: GraphStore, asof, since, graphs: list | None = None) -> set[str]:
    # a fact written after `asof` does not exist yet
    return {r["s"].value for r in store.ask(AS_OF.format(asof=dt_literal(asof).n3(), since=dt_literal(since).n3()), graphs)}
```

**O que isso faz:**
- Exige que cada sinal tenha um momento de ocorrência (`corr:at`) e um momento de detecção (`corr:detectedAt`).
- Mantém apenas os sinais detectados antes do momento da consulta, então as consultas "as of" nunca veem o futuro.

Na execução completa, o relatório do incidente de abril trouxe um **canário**, um fato que nunca deve chegar a um agente. Os resultados foram:

```text
#### OUTPUT ####
  asked as of                all graphs  canary?  agent graphs  canary?
  2024-04-17 09:33 UTC              105       no           105       no
  2024-04-18 16:00 UTC              177      yes           175       no
  rollback of a bad run: 2,067 quads before, 1,322 after
```

**O que isso faz:**
- Mostra o canário ausente às 09:33 UTC de 17 de abril e presente às 16:00 UTC de 18 de abril no armazenamento completo.
- Mostra que os grafos do agente nunca contêm o canário, e que reverter uma execução ruim reduziu o armazenamento de 2,067 quads para 1,322.

O canário aparece somente depois que o relatório existe, e nunca nos grafos de um agente. Pelo nó de hora, "o que mudou na hora anterior?" levou 0.50 ms em vez de 85.55 ms.

## Contrato do enriquecedor
Os fatos chegam por meio de **enriquecedores**, pequenos programas que lêem cada um uma fonte. Cada execução registra sua **cobertura**, porque "nada disparou" e "ninguém olhou" são respostas diferentes.

```python
"""enrich.py: the contract every enricher keeps, their order, and the checks on their work."""
import re
from collections import Counter
from dataclasses import dataclass
from graphlib import TopologicalSorter

import pandas as pd
import rdflib
from rdflib import RDF, Literal, URIRef

from kg.ontology import HARVEST, check, mint
from kg.store import GraphStore, dt_literal


@dataclass
class Coverage:
    source: str
    start: pd.Timestamp
    end: pd.Timestamp
    status: str                    # ok, partial or missing: 'nothing fired' is not 'nobody looked'
    detail: str = ""
```

**O que isso faz:**
- Define um registro `Coverage` com uma fonte, uma janela de tempo, um status `ok`, `partial` ou `missing` e uma string de detalhe.
- Mantém o status em três valores, para que uma fonte silenciosa não seja confundida com uma fonte tranquila.

Uma fonte morta custa cobertura, nunca a colheita:

```python
# kg/enrich.py
class Enricher:
    """fetch() may fail. analyse() is pure and returns facts."""
    name, requires, produces, source = "enricher", (), (), ""

    def run(self, start, end) -> tuple[rdflib.Graph, Coverage]:
        try:
            raw, why = self.fetch(start, end), ""
        except Exception as e:                             # a dead source costs coverage, not the harvest
            raw, why = None, f"{type(e).__name__}: {e}"
        facts = self.analyse(raw, start, end) if raw is not None else rdflib.Graph()
        return facts, Coverage(self.source, start, end, "ok" if raw is not None else "missing", why)
```

**O que isso faz:**
- Chama `fetch` dentro de um bloco try, então uma exceção apenas marca a fonte como `missing` e registra o texto do erro.
- Executa a etapa pura `analyse` somente quando algum dado foi obtido, o que mantém a busca e o raciocínio separados.

Cada enriquecedor declara o que precisa e o que produz, e a ordenação topológica da biblioteca padrão deriva a ordem de execução:

```python
# kg/enrich.py
def plan_order(enrichers: list, available: frozenset = frozenset()) -> list:
    """The run order, from what each enricher needs and makes."""
    makers = {cls: e.name for e in enrichers for cls in e.produces}
    for e in enrichers:                                    # an unmet need stops the plan before any run
        if missing := [c for c in e.requires if c not in makers and c not in available]:
            raise ValueError(f"{e.name} requires {missing[0].rsplit('#', 1)[-1]}, which no enricher produces")
    by_name = {e.name: e for e in sorted(enrichers, key=lambda e: e.name)}     # sorted, so the plan never wobbles
    deps = {n: sorted({makers[c] for c in e.requires if c in makers} - {n}) for n, e in by_name.items()}
    return [by_name[n] for n in TopologicalSorter(deps).static_order()]         # CycleError on a loop
```

**O que isso faz:**
- Constrói um mapa de cada classe para o enriquecedor que a produz, e então recusa iniciar se alguma classe exigida não tiver produtor.
- Ordena os enriquecedores por nome antes, para que o plano nunca mude entre execuções, e depois aplica o `TopologicalSorter`.
- Lança um erro em caso de laço de dependência, porque o `TopologicalSorter` reporta um ciclo.

Em qualquer ordem em que sejam listados, o enriquecedor de host ainda roda primeiro, e um enriquecedor cuja entrada ninguém produz é recusado antes de qualquer execução. O pipeline verifica cada delta com o seu contexto armazenado, e fatos em quarentena ainda deixam um registro de cobertura:

```python
# kg/enrich.py
def run_pipeline(enrichers: list, start, end, store: GraphStore, tenant: str, run: str) -> list[Coverage]:
    """Each enricher in order. A delta that breaks a shape is quarantined."""
    have = frozenset(URIRef(r["c"].value) for r in store.ask("SELECT DISTINCT ?c WHERE { ?s a ?c }"))
    report = []
    for e in plan_order(enrichers, have):
        started, (facts, cov) = pd.Timestamp.now(tz="UTC"), e.run(start, end)
        ok, rows = check(facts, store.context(facts)) if len(facts) else (True, [])
        if not ok:
            cov.status, cov.detail = "partial", f"{len(rows)} shape violations, first: {rows[0][2]} on {rows[0][1]}"
            facts = rdflib.Graph()
        node = mint("harvest", "coverage", f"{e.name}-{cov.source}-{start:%Y%m%dT%H%M}")
        for p, o in ((RDF.type, HARVEST.Coverage), (HARVEST.source, Literal(cov.source)), (HARVEST.status,
                     Literal(cov.status)), (HARVEST.windowStart, dt_literal(start)), (HARVEST.windowEnd, dt_literal(end))):
            facts.add((node, p, o))                        # what was read, and how well, is a fact too
        store.write(facts, tenant, e.name, run, [], started)
        report.append(cov)
    return report
```

**O que isso faz:**
- Executa cada enriquecedor na ordem planejada e verifica seus fatos contra as formas, usando o contexto armazenado.
- Se um delta quebra uma forma, marca a cobertura como `partial`, descarta os fatos e ainda grava o registro de cobertura.
- Grava a saída de cada enriquecedor como seu próprio grafo nomeado, para que os resultados de cada fonte possam ser rastreados e descartados separadamente.

A cobertura dá a uma pergunta três respostas em vez de duas:

```python
# kg/enrich.py
def alert_answer(store: GraphStore, host: URIRef, t: pd.Timestamp, source: str) -> str:
    # did anything alert on this host in the half hour before t? three answers, not two
    a, b = dt_literal(t - pd.Timedelta(minutes=30)).n3(), dt_literal(t).n3()
    if store.ask(f"SELECT ?f WHERE {{ ?f a alert:Firing . ?f corr:on <{host}> . ?f corr:at ?x . "
                 f"FILTER(?x >= {a} && ?x <= {b}) }} LIMIT 1"):
        return "yes, an alert fired"
    seen = store.ask(f'SELECT ?st WHERE {{ ?c harvest:source "{source}" . ?c harvest:status ?st . '
                     f"?c harvest:windowStart ?w0 . ?c harvest:windowEnd ?w1 . FILTER(?w0 <= {b} && ?w1 >= {b}) }}")
    return "no, the source was read and nothing fired" if seen and seen[0]["st"].value == "ok" else \
        "unknown, the alert source was not covered"
```

**O que isso faz:**
- Retorna "sim" se um alerta disparou no host na meia hora antes do horário perguntado.
- Retorna "não" somente se a fonte de alertas foi lida naquela janela e nada disparou.
- Caso contrário, retorna "desconhecido", porque a fonte nunca foi coberta.

Com o Loki desligado, um "não" de um host silencioso virou "desconhecido". A última proteção é o portão do catálogo:

```python
# kg/enrich.py
SERVICE_LIKE = re.compile(r"(?<![\w.-])([a-z][a-z0-9]+(?:-[a-z0-9]+)+)(?![\w-])")


def gate(texts, is_host, in_catalogue) -> Counter:
    # service-looking tokens the catalogue refuses, with counts
    seen = Counter(tok for t in texts for tok in SERVICE_LIKE.findall(t.lower()) if not is_host(tok))
    return Counter({tok: n for tok, n in seen.items() if not in_catalogue(tok)})
```

**O que isso faz:**
- Encontra tokens com cara de serviço no texto dos alertas, como `mw-web`, que não são hosts conhecidos.
- Conta esses tokens e mantém apenas os que o catálogo não reconhece, para que os resultados mostrem o que o portão recusaria.

Das 3,841 strings desse tipo nos alertas do Loki, o catálogo conhecia 107 e recusou 3,734. Sem o portão, cada nome recusado se tornaria uma aplicação sem dono no grafo.

> ❌ **Anti-padrão — nomes sem portão como entidades:** uma string com cara de serviço é escrita diretamente no grafo. **Por que as pessoas fazem isso:** é mais rápido do que consultar um catálogo. **Consequência:** o grafo se enche de aplicações que ninguém possui e que não podem ser roteadas nem corrigidas. **Correção:** verificar cada nome contra o catálogo e manter as recusas visíveis.

## Escada de extração
O chat de incidente é texto livre. A análise da Netflix sobe cinco degraus, os mais baratos primeiro, com um modelo no quarto. Em 13,331 linhas de chat humano de cerca de 128 incidentes, os dois primeiros degraus chegam até aqui:

```text
#### OUTPUT ####
  after layer                      text typed   lines linked
  1 regular expressions                  2.0%           8.8%
  2 catalogue validation                 2.7%           7.1%
```

**O que isso faz:**
- Mostra a parcela de linhas com texto tipado e a parcela ligada a uma entidade conhecida, depois dos degraus 1 (expressões regulares) e 2 (validação pelo catálogo).
- Mostra que 93% das linhas ainda não se ligam a nada, por isso um modelo é necessário.

Cada chamada de modelo passa por um único cliente com cache, livro-razão e no máximo seis chamadas em andamento:

```python
"""llm.py: one door for every model call, with a cache, a ledger and a budget checked before each call."""
import hashlib
import json
import re
import sqlite3
import threading
import time
from pathlib import Path

import httpx

from kg.config import API_KEY, BASE_URL, BUDGET, PRICE

Path("artifacts").mkdir(exist_ok=True)
CACHE = sqlite3.connect("artifacts/llm_cache.sqlite", check_same_thread=False)
CACHE.execute("CREATE TABLE IF NOT EXISTS calls (key TEXT PRIMARY KEY, reply TEXT, seconds REAL)")
LEDGER = Path("artifacts/llm_ledger.jsonl")                # every paid call, so the budget survives restarts
HTTP = httpx.Client(base_url=BASE_URL, timeout=180, headers={"Authorization": f"Bearer {API_KEY}"})
GATE, LOCK, PHASE = threading.BoundedSemaphore(6), threading.Lock(), ["experiments"]


def spent(phase: str) -> float:
    rows = [json.loads(x) for x in LEDGER.read_text().splitlines()] if LEDGER.exists() else []
    return sum(r["credits"] for r in rows if r["phase"] == phase)
```

**O que isso faz:**
- Abre um cache SQLite e um livro-razão JSONL, para que as respostas sejam reutilizadas e cada chamada paga seja registrada entre reinicializações.
- Cria um cliente HTTP com uma chave bearer e define um semáforo para seis chamadas simultâneas.
- Prepara a tabela do cache e o lock usado nas escritas.

O orçamento é verificado antes de a chamada ser enviada, e uma pergunta já em cache é gratuita e repete a sua resposta:

```python
# kg/llm.py
def chat(model: str, messages: list, json_mode: bool = True, tools: list | None = None, max_tokens: int = 800) -> dict:
    body = {"model": model, "messages": messages, "max_tokens": max_tokens, "temperature": 0.0}
    body |= ({"response_format": {"type": "json_object"}} if json_mode else {}) | ({"tools": tools} if tools else {})
    key = hashlib.sha256(json.dumps(body, sort_keys=True).encode()).hexdigest()   # same question, same key
    if hit := CACHE.execute("SELECT reply, seconds FROM calls WHERE key = ?", (key,)).fetchone():
        return {**json.loads(hit[0]), "seconds": hit[1], "cached": True}          # free, and identical
    if spent(PHASE[0]) + PRICE[model] > BUDGET[PHASE[0]]:                         # refused before it is made
        raise RuntimeError(f"'{PHASE[0]}' would pass its cap of {BUDGET[PHASE[0]]} credits")
    t0 = time.perf_counter()
    with GATE:                                             # at most six calls in flight
        for attempt in range(1, 7):
            r = HTTP.post("/chat/completions", json=body)
            if r.status_code != 429 and r.status_code < 500:
                break
            time.sleep(min(30, 1.5 * 2 ** attempt))
    r.raise_for_status()
    msg, seconds = r.json()["choices"][0]["message"], time.perf_counter() - t0
    reply = {"text": msg.get("content") or "", "tool_calls": msg.get("tool_calls")}
    with LOCK:                                             # every answered call is billed, so all are logged
        LEDGER.open("a").write(json.dumps({"phase": PHASE[0], "model": model, "credits": PRICE[model]}) + "\n")
        CACHE.execute("INSERT OR REPLACE INTO calls VALUES (?, ?, ?)", (key, json.dumps(reply), seconds))
        CACHE.commit()
    return {**reply, "seconds": seconds, "cached": False}
```

**O que isso faz:**
- Calcula o hash de todo o corpo da requisição, então a mesma pergunta sempre gera a mesma chave de cache.
- Retorna uma resposta em cache gratuitamente, e recusa uma nova chamada que levaria uma fase além do teto de créditos.
- Tenta de novo as respostas com limite de taxa ou erro de servidor, com uma espera crescente, e registra cada chamada respondida.

Um auxiliar extrai JSON de uma resposta, mesmo quando ele está dentro de um bloco de código:

```python
# kg/llm.py
def parse_json(text: str):
    """The first JSON object in a reply, even inside a code fence."""
    m = re.search(r"\{.*\}", text or "", flags=re.S)
    try:
        return json.loads(m.group(0)) if m else None
    except json.JSONDecodeError:
        return None
```

**O que isso faz:**
- Encontra o primeiro objeto entre chaves na resposta e o interpreta.
- Retorna `None` em vez de lançar erro quando a resposta não é JSON válido, para que quem chamou decida o que fazer.

O modelo não pode inventar categorias. Seu vocabulário é lido do arquivo de formas:

```python
# kg/ontology.py
def closed_list(shape: str, path: URIRef) -> list[str]:
    # the sh:in list of one property
    prop = next(p for p in SHAPES.objects(URIRef(f"{BASE}shapes#{shape}"), SH.property) if SHAPES.value(p, SH.path) == path)
    return [str(v) for v in Collection(SHAPES, SHAPES.value(prop, SH["in"]))]
```

**O que isso faz:**
- Lê os valores permitidos de uma propriedade a partir da lista `sh:in` no arquivo de formas.
- Faz com que as listas de categoria e de sentimento existam em um só lugar, e que o prompt não diverja do validador.

A resposta é verificada em código antes de qualquer escrita:

```python
"""ladder.py: the model's rung of the extraction ladder, held to the shapes' closed lists."""
from kg.ontology import SLACK, closed_list

CATEGORIES = closed_list("MessageShape", SLACK.category)    # the vocabulary comes from shapes.ttl
SENTIMENTS = closed_list("MessageShape", SLACK.sentiment)
L4_SYSTEM = ("You label on-call chat conversations. Answer with JSON only, with the keys category, sentiment, "
             f"incident_related and mentions. category is one of {', '.join(CATEGORIES)}. sentiment is one of "
             f"{', '.join(SENTIMENTS)}. incident_related is true or false. mentions lists candidate ids only.")


def l4_valid(data, candidates: list[str]) -> bool:
    # checked in code before anything is written
    return (isinstance(data, dict) and data.get("category") in CATEGORIES and data.get("sentiment") in SENTIMENTS
            and isinstance(data.get("incident_related"), bool) and isinstance(data.get("mentions"), list)
            and all(m in candidates for m in data["mentions"]))
```

**O que isso faz:**
- Monta o prompt do sistema a partir das listas fechadas, para que o modelo saiba que suas únicas categorias e sentimentos permitidos são estes.
- O validador aceita uma resposta somente se categoria e sentimento estiverem na lista, se `incident_related` for booleano e se cada menção for um id candidato.

Aqui o modelo rotula as dez conversas em torno do incidente de abril:

```python
from kg.config import MODELS
from kg.ladder import L4_SYSTEM, l4_valid
from kg.llm import PHASE, chat, parse_json

PHASE[0] = "chat classification"
for k, (text, ids) in enumerate(conversations):           # scrubbed text, and the ids rungs 1 to 3 found
    r = parse_json(chat(MODELS["flash"], [{"role": "system", "content": L4_SYSTEM},
                                          {"role": "user", "content": f"candidates: {ids}\n\n{text}"}])["text"])
    if l4_valid(r, ids) and r["incident_related"]:
        print(f"  {k:<3}{r['category']:<11}{r['sentiment']:<9}{r['mentions']}")

#### OUTPUT ####
  1  incident   urgent   ['service:mw-web']
  6  incident   urgent   ['host:ncredir2001', 'service:mw-api-int', 'service:mw-web']
  7  discussion positive []
```

**O que isso faz:**
- Envia cada conversa com seus ids candidatos ao modelo flash na fase de classificação.
- Imprime somente as respostas que passam na validação e são relacionadas a incidentes, para que saídas inválidas nunca cheguem ao grafo.

As linhas impressas mostram três conversas relacionadas a incidentes, duas delas urgentes. As respostas do modelo vêm da execução gravada pelo autor em 31 de outubro e 1 de novembro de 2026, com prompts encurtados. Perguntado três vezes com temperatura 0.7, os rótulos se repetiram em 90% das vezes com o vocabulário fechado. Com JSON livre, eles nunca se repetiram, e o Claude com saídas estruturadas chegou a 70%.

> ❌ **Anti-padrão — rótulos livres do modelo:** o modelo responde com qualquer categoria que preferir. **Por que as pessoas fazem isso:** o modo JSON parece estrutura suficiente. **Consequência:** a mesma conversa recebe rótulos diferentes a cada execução, e os rótulos não podem ser contados. **Correção:** dar ao modelo uma lista fechada lida do esquema e validar cada resposta em código.

Isto é o padrão de [modelo de decisão estruturado](/pt-br/concepts/ai-ml/structured-decision-model.md) aplicado ao chat: saídas tipadas com escolhas fixas, em vez de texto livre.

## Motor de correlação
Toda revisão de incidente pergunta o que mudou logo antes. A Wikimedia registrou 283,743 mudanças, uma a cada 10.1 minutos, e o código de correlação começa contando quantas mudanças ficam perto de um determinado momento.

```python
"""correlate.py: how often a change is nearby by chance, and chains from a cause to a symptom."""
import numpy as np


def changes_before(moments: np.ndarray, minutes: float, times: np.ndarray) -> np.ndarray:
    """Changes in (t - minutes, t] for every moment t, by binary search."""
    return np.searchsorted(times, moments, side="right") - np.searchsorted(times, moments - minutes * 60, side="right")
```

**O que isso faz:**
- Usa busca binária sobre os horários ordenados das mudanças, então contar as mudanças em cada janela é rápido, mesmo com centenas de milhares delas.
- Retorna, para cada momento, o número de mudanças na janela que o antecede.

Lançado em 20,000 momentos aleatórios, encontra uma mudança na hora anterior em 58.9% deles. Um deploy precede 40.4% dos incidentes em até 30 minutos, contra 31.4% das mesmas horas em outras semanas.

> ❌ **Anti-padrão — correlação apenas por tempo:** uma mudança é culpada porque veio primeiro. **Por que as pessoas fazem isso:** o tempo é barato e intuitivo. **Consequência:** a produção está sempre ocupada, então o tempo mede sobretudo atividade, não causas. **Correção:** exigir um caminho plausível no grafo de chamadas antes de criar a ligação.

Os detectores comparam cada janela do OpenRCA com os sessenta minutos anteriores e sinalizaram 863,062 minutos. Cada sinal precisa dos nomes de componente do gabarito:

```python
"""signals.py: signal minutes that know their component, and the call graph between components."""
import re

import numpy as np
import pandas as pd


def components_of(system: str, entity: str) -> list[str]:
    """The answer key's names for one measurement."""
    e = str(entity)
    if not system.startswith("market"):
        return [e]                                         # bank and telecom use the key's names already
    if e.endswith("-grpc") or ".ts:" in e:
        return [re.split(r"-grpc$|\.ts:", e)[0].rstrip("2")]
    if "." in e:
        node, pod = e.split(".", 1)                        # 'node-6.cartservice-0'
        return [pod, re.sub(r"2?-\d+$", "", pod), node]
    return [e] if re.fullmatch(r"node-\d+", e) else [e, re.sub(r"2?-\d+$", "", e)]
```

**O que isso faz:**
- Mapeia cada medição para nomes de componente, com tratamento especial para a nomenclatura de nó e de pod do sistema de mercado.
- Retorna a entidade bruta para banco e telecom, que já usam os nomes do gabarito.

Um sinal pode explicar outro somente se o segundo componente chama o primeiro, diretamente ou um passo acima, ou roda sobre ele:

```python
# kg/signals.py
def related_in(callers: dict, node_of: dict):
    # b may be an effect of a when b calls a, directly or one step up, or a is the node b runs on
    def related(a: str, b: str) -> bool:
        if re.sub(r"2?-\d+$", "", a) == re.sub(r"2?-\d+$", "", b) or node_of.get(b) == a:
            return True
        up = callers.get(a, set())
        return b in up or any(b in callers.get(u, set()) for u in up)
    return related


def chain_inputs(sig: pd.DataFrame, system: str, top: int = 3) -> list[dict]:
    """The strongest few signals per component and kind, severity in 0 to 1."""
    if sig.empty:
        return []                                          # six windows produced no signal at all
    d = pd.concat([g.assign(comp=c) for e, g in sig.groupby("entity") for c in components_of(system, e)])
    d["severity"] = np.clip(np.log1p(d.strength) / np.log1p(50), 0, 1)       # z of 50 or more is fully severe
    d = d.sort_values("severity", ascending=False).groupby(["comp", "kind"]).head(top)
    return [{"sid": f"{r.comp}|{r.kind}|{r.minute:%H%M}|{i}", "type": r.kind, "comp": r.comp, "t": r.minute,
             "severity": float(r.severity)} for i, r in enumerate(d.itertuples())]
```

**O que isso faz:**
- `related` aceita uma ligação quando os dois componentes são da mesma família de serviço, quando um roda no nó do outro, ou quando o grafo de chamadas os liga diretamente ou por um chamador intermediário.
- `chain_inputs` dá a cada sinal uma severidade entre 0 e 1, usando um logaritmo para que um z-score de 50 ou mais seja totalmente severo, e mantém os mais fortes por componente e tipo.

Um salto é pontuado pelas regras do texto original. Uma causa e um efeito reconhecidos recebem 0.9, e qualquer outro salto recebe uma pontuação construída a partir da proximidade no tempo e da severidade, dentro de uma janela de 10 minutos. O autor acrescenta uma mudança: a linhagem pode ampliar essa janela.

```python
# kg/correlate.py
KNOWN_PAIRS = {("DEPLOYMENT", "LOG_ERROR"), ("CONFIG_CHANGE", "METRIC_ANOMALY"), ("ALERT", "INCIDENT"),
               ("TRACE_FAILED", "INCIDENT")}


def hop_score(a: str, b: str, minutes: float, severity: float, window: float = 10.0) -> float:
    """A known causal pair scores 0.9, any other hop 0.6 x closeness + 0.4 x severity."""
    return 0.9 if (a, b) in KNOWN_PAIRS else 0.6 * max(0.0, 1 - minutes / window) + 0.4 * severity


def may_link(a: dict, b: dict, mode: str, related, window: float = 10.0, lineage: set = frozenset()) -> float | None:
    """Minutes from a to b if the hop is allowed, else None."""
    gap = (b["t"] - a["t"]).total_seconds() / 60
    if gap < 0:
        return None
    if (a["sid"], b["sid"]) in lineage:
        return gap                                         # the deploy ships the commit, whatever the clock says
    if gap > window:
        return None
    return gap if mode == "timing" or a["comp"] == b["comp"] or related(a["comp"], b["comp"]) else None
```

**O que isso faz:**
- Dá a um par causal conhecido, como deploy para erro de log, uma pontuação fixa de 0.9.
- Para qualquer outro salto, combina proximidade temporal (60%) e severidade (40%), que é a regra do texto original sem o piso de 0.6.
- `may_link` recusa ligações para trás e ligações fora da janela, a menos que a linhagem diga que o deploy trouxe o commit.

A busca em profundidade segue os cinco melhores próximos saltos em direção aos sintomas:

```python
# kg/correlate.py
def best_chains(signals: list, sinks: set, mode: str, related, max_hops: int = 4, lineage: set = frozenset()) -> dict:
    """Depth-first from every signal to the symptoms. Each keeps its best chain."""
    by_time, best = sorted(signals, key=lambda s: s["t"]), {}
    nxt = {s["sid"]: [] for s in by_time}
    for i, a in enumerate(by_time):
        for b in by_time[i + 1:]:
            if (b["t"] - a["t"]).total_seconds() > 600 and not lineage:
                break                                      # sorted by time, so nothing later can link
            if (gap := may_link(a, b, mode, related, lineage=lineage)) is not None:
                score = None if (a["sid"], b["sid"]) in lineage else hop_score(a["type"], b["type"], gap, b["severity"])
                nxt[a["sid"]].append((b, score))
        nxt[a["sid"]] = sorted(nxt[a["sid"]], key=lambda x: -(x[1] if x[1] is not None else 1))[:5]   # 5 best hops

    def walk(root, node, scores, path):
        if node["sid"] in sinks and scores and np.mean(scores) > best.get(root["sid"], (0, None))[0]:
            best[root["sid"]] = (float(np.mean(scores)), list(path))
        for b, sc in (nxt[node["sid"]] if len(path) <= max_hops else []):
            walk(root, b, scores + ([sc] if sc is not None else []), path + [b["sid"]])

    for s in by_time:
        if s["sid"] not in sinks:
            walk(s, s, [], [s["sid"]])
    return best
```

**O que isso faz:**
- Ordena os sinais por tempo e mantém somente os cinco melhores próximos saltos de cada sinal.
- Percorre a partir de cada sinal em direção aos sintomas, mantendo a cadeia com a maior média de pontuação, até quatro saltos.

Os componentes são ranqueados pela sua melhor cadeia, e os componentes não alcançados vêm depois, do mais barulhento para o menos:

```python
# kg/correlate.py
def rank_loudest(signals: list) -> list[str]:
    """The baseline: components by their loudest signal, ties by name."""
    loud = {}
    for s in signals:
        loud[s["comp"]] = max(loud.get(s["comp"], 0), s["severity"])
    return [c for c, _ in sorted(loud.items(), key=lambda x: (-x[1], x[0]))]


def rank_components(signals: list, mode: str, related) -> list[str]:
    """Components by best chain, then the unreached ones, loudest first."""
    sinks = {s["sid"] for s in signals if s["type"] in ("TRACE_SLOW", "TRACE_FAILED")} or \
        {max(signals, key=lambda s: s["t"])["sid"]}         # the symptoms are what callers felt
    comp, score = {s["sid"]: s["comp"] for s in signals}, {}
    for sid, (s, _) in best_chains(signals, sinks, mode, related).items():
        score[comp[sid]] = max(score.get(comp[sid], 0), s)
    chained = [c for c, _ in sorted(score.items(), key=lambda x: (-x[1], x[0]))]
    return chained + [c for c in rank_loudest(signals) if c not in score]
```

**O que isso faz:**
- `rank_loudest` é a linha de base: componentes ordenados pelo seu sinal mais severo.
- `rank_components` ranqueia os componentes pela melhor cadeia que chega a um sintoma, e coloca depois deles os que nenhuma cadeia alcança, na ordem de intensidade.

O motor precisa primeiro reproduzir a cadeia de exemplo do texto original:

```python
import pandas as pd
from kg.correlate import best_chains

t0, same = pd.Timestamp("2026-01-01 12:00", tz="UTC"), lambda a, b: a == b
chain = [{"sid": k, "type": k, "comp": "api-gateway", "t": t0 + pd.Timedelta(minutes=m), "severity": 0.825}
         for k, m in (("COMMIT", 0), ("DEPLOYMENT", 14), ("LOG_ERROR", 17), ("ALERT", 19), ("INCIDENT", 24))]
score, path = best_chains(chain, {"INCIDENT"}, "topology", same, lineage={("COMMIT", "DEPLOYMENT")})["COMMIT"]
print(f"  {score:.3f} via {path}, without lineage: {'COMMIT' in best_chains(chain, {'INCIDENT'}, 'topology', same)}")

#### OUTPUT ####
  0.870 via ['COMMIT', 'DEPLOYMENT', 'LOG_ERROR', 'ALERT', 'INCIDENT'], without lineage: False
```

**O que isso faz:**
- Monta a cadeia de exemplo do texto (commit, deploy, erro de log, alerta, incidente) com severidade 0.825.
- Verifica se o motor retorna 0.870 para o caminho completo e se, sem linhagem, o salto de deploy de 14 minutos é recusado.

Ele reproduz 0.87, e sem linhagem o salto de 14 minutos é recusado.

Nas 41 falhas de desenvolvimento com um componente nomeado, a topologia melhora a precisão de forma substancial:

```text
#### OUTPUT ####
  ranking                  AC@1     AC@3
  loudest signal           4.9%     4.9%
  timing-only chains       4.9%     9.8%
  topology chains          9.8%    26.8%
```

**O que isso faz:**
- Compara três rankings: o sinal mais barulhento, cadeias apenas por tempo e cadeias por topologia.
- Mostra que as cadeias por topologia elevam a precisão no top três (AC@3) de 4.9% para 26.8%.

Apenas 6% das cadeias com pontuação acima de 0.9 apontaram para o componente correto, com um escore de Brier de 0.871. A pontuação ordena causas e não é uma probabilidade. A [calibração](/pt-br/concepts/ai-ml/calibration.md) do escore de cadeia é fraca, então ele deve ser lido como um ranking.

> 🔎 Contexto adicional: o escore de Brier mede a diferença quadrática média entre probabilidades previstas e resultados reais, em que 0 é perfeito e valores maiores são piores. Um escore de 0.871 mostra, portanto, que o escore de cadeia não é uma probabilidade utilizável.

## Volante de conhecimento
Cada falha resolvida vem com a sua resposta, e cada resposta pode ensinar à próxima investigação onde olhar. A execução completa aprendeu o quanto cada tipo de sinal aparece com mais frequência em uma causa raiz do que em um espectador. Avaliado pelos ganhos aprendidos nas outras falhas, colocou o componente certo em primeiro lugar em 29.3% delas, contra 4.9% da intensidade. Em um cluster que nunca tinha visto, superou regras escritas à mão em 7 falhas e perdeu em 8.

## Especialistas em causa raiz
Ler as evidências é o trabalho de agentes especialistas. Cada um é um [harness de agente](/pt-br/concepts/ai-ml/agent-harness.md): um modelo fixo envolto em ferramentas tipadas, com um limite de turnos e um formato de saída obrigatório. Um especialista nunca lê dados brutos. Suas ferramentas tipadas devolvem linhas com ids que ele pode citar, e lembram de cada id que entregaram.

```python
"""agents.py: specialists with tools, a verifier for their evidence, a reconciler that may ask back."""
import inspect
import json
from collections import Counter
from concurrent.futures import ThreadPoolExecutor

import pandas as pd

from kg.config import MODELS
from kg.llm import chat, parse_json


def utc8(ts) -> str:
    return pd.Timestamp(ts).tz_convert("Asia/Shanghai").strftime("%Y-%m-%d %H:%M:%S")   # the answer key's clock


class LabTools:
    """What a specialist may ask about one window. Only ids handed out may be cited."""

    def __init__(self, sig: pd.DataFrame, callers: dict):
        self.sig = sig.assign(id=[f"s{i}" for i in range(len(sig))])     # one id per signal minute
        self.callers, self.given = callers, set()                         # given: every id handed out

    def _rows(self, d: pd.DataFrame) -> list[dict]:
        d = d.sort_values("strength", ascending=False).head(12)
        self.given |= set(d.id)
        return [{"id": r.id, "entity": r.entity, "kind": r.kind, "family": r.family, "minute": utc8(r.minute),
                 "strength": round(float(r.strength), 1)} for r in d.itertuples()]
```

**O que isso faz:**
- Mantém a tabela de sinais com um id único por linha, e um registro (`given`) de cada id que qualquer ferramenta entregou.
- Retorna no máximo doze linhas por chamada, ordenadas por intensidade, para que uma ferramenta não inunde o contexto.
- Formata os horários no relógio UTC+8 do gabarito, para que o agente e o gabarito concordem.

Cada ferramenta é um pequeno método cuja docstring o modelo lê:

```python
# kg/agents.py
    def top_anomalies(self, kind: str = "any") -> list[dict]:
        """The strongest anomalies in the window. kind: any, METRIC_ANOMALY, TRACE_SLOW, TRACE_FAILED or LOG_ERROR."""
        d = self.sig if kind == "any" else self.sig[self.sig.kind == kind]
        return self._rows(d.sort_values("strength", ascending=False).drop_duplicates(["entity", "family"]))

    def component_signals(self, component: str) -> list[dict]:
        """Every anomaly on one component, strongest first."""
        return self._rows(self.sig[self.sig.comps.map(lambda cs: component in cs)])

    def first_anomaly(self, component: str) -> dict:
        """When the first anomaly on a component appeared."""
        d = self.sig[self.sig.comps.map(lambda cs: component in cs)].sort_values("minute")
        if d.empty:
            return {"component": component, "first": None}
        self.given.add(d.id.iloc[0])
        return {"component": component, "first": utc8(d.minute.iloc[0]), "id": d.id.iloc[0], "family": d.family.iloc[0]}

    def neighbours(self, component: str) -> dict:
        """Which components call this one and which it calls, from the traces."""
        return {"component": component, "called_by": sorted(self.callers.get(component, set()))[:15],
                "calls": sorted(c for c, up in self.callers.items() if component in up)[:15]}
```

**O que isso faz:**
- `top_anomalies` lista as anomalias mais fortes, opcionalmente filtradas por tipo.
- `component_signals` lista cada anomalia de um componente.
- `first_anomaly` retorna a primeira anomalia de um componente e registra o seu id como entregue.
- `neighbours` retorna os chamadores e os chamados a partir dos traces.

A especificação das ferramentas é gerada a partir do código, e um argumento inválido devolve um erro que o agente consegue ler:

```python
# kg/agents.py
    def schema(self, names: list[str]) -> list[dict]:
        # docstrings become descriptions, so the code is the spec
        return [{"type": "function", "function": {"name": n, "description": getattr(self, n).__doc__, "parameters": {
                 "type": "object", "properties": {a: {"type": "string"} for a in inspect.signature(getattr(self, n)).parameters}}}}
                for n in names]

    def call(self, name: str, args: dict):
        try:
            return getattr(self, name)(**args)
        except Exception as e:                             # a bad argument is an answer the agent can read
            return {"error": f"{type(e).__name__}: {e}"}
```

**O que isso faz:**
- Constrói o esquema de cada ferramenta a partir do nome do método, da sua docstring e dos nomes dos parâmetros, então o código é a própria especificação.
- Captura qualquer exceção levantada por uma ferramenta e a devolve como dado, para que o agente possa se corrigir.

Os três especialistas recebem as suas ferramentas e um briefing cada. Os papéis dos especialistas são mostrados abaixo:

```python
# kg/agents.py
SPECIALISTS = {   # role: (its tools, its brief)
    "metrics": (["top_anomalies", "component_signals", "first_anomaly"], "You are the metrics specialist."),
    "traces": (["top_anomalies", "neighbours", "first_anomaly"], "You are the traces specialist. A slow caller is often "
               "the victim of a slow callee."),
    "logs": (["top_anomalies", "component_signals"], "You are the logs specialist. Look at bursts of error lines."),
}
FINDING = ('Finish with JSON only: {"hypotheses": [{"component": ..., "reason": ..., "datetime": "YYYY-MM-DD HH:MM:SS" '
           'in UTC+8, "evidence": [ids your tools returned], "confidence": 0 to 1}]}, best first, at most 3.')
```

**O que isso faz:**
- Dá aos especialistas de métricas, traces e logs conjuntos de ferramentas e briefings diferentes; os de traces, por exemplo, são informados de que um chamador lento costuma ser vítima de um chamado lento.
- Exige a mesma saída em JSON dos três: uma hipótese com componente, motivo, horário em UTC+8, ids de evidência e confiança, no máximo três.
- Executa cada especialista por no máximo três rodadas de ferramentas e depois um turno sem ferramentas, para que ele seja obrigado a responder.

O laço que executa cada especialista:

```python
# kg/agents.py
def run_specialist(role: str, tools: LabTools, question: str, vocab: dict, rounds: int = 3) -> dict:
    """Three rounds of tools at most, then a last turn without them."""
    names, brief = SPECIALISTS[role]
    msgs = [{"role": "system", "content": f"{brief} Allowed components: {vocab['components']}. Allowed reasons: "
             f"{vocab['reasons']}. Use the tools and cite only ids they return. {FINDING}"}, {"role": "user", "content": question}]
    trail, turns, seconds = [], 0, 0.0
    for step in range(rounds + 1):
        last = step == rounds                              # no tools on the last turn, so it must answer
        r = chat(MODELS["flash"], msgs + ([{"role": "user", "content": "No more tools. " + FINDING}] if last else []),
                 json_mode=False, tools=None if last else tools.schema(names), max_tokens=700)
        turns, seconds = turns + 1, seconds + r["seconds"]
        if not r["tool_calls"] or last:
            hyps = (parse_json(r["text"]) or {}).get("hypotheses") or []
            return {"hypotheses": hyps[:3], "tools": trail, "turns": turns, "seconds": seconds}
        msgs.append({"role": "assistant", "content": r["text"], "tool_calls": r["tool_calls"]})
        for tc in r["tool_calls"]:                          # one turn may ask for several tools
            name, args = tc["function"]["name"], parse_json(tc["function"].get("arguments")) or {}
            trail.append(name)
            result = tools.call(name, args) if name in names else {"error": f"no tool {name}"}
            msgs.append({"role": "tool", "tool_call_id": tc.get("id", name), "content": json.dumps(result, default=str)[:6000]})
```

**O que isso faz:**
- Envia a cada especialista as suas ferramentas como esquema, executa cada chamada de ferramenta que ele faz e devolve cada resultado como uma mensagem de ferramenta limitada a 6,000 caracteres.
- Retira as ferramentas no turno final, para que o especialista precise responder, e retorna suas três melhores hipóteses com o rastro dos nomes das ferramentas, turnos e segundos.

Os três especialistas rodaram no modelo flash, com estes resultados:

```text
#### OUTPUT ####
  metrics  4 turns, 7 tool calls, 11.3 s   -> node-6, node memory consumption, 15:48
  traces   4 turns, 5 tool calls,  9.1 s   -> cartservice-0, container read I/O load, 15:48
  logs     4 turns, 6 tool calls, 11.0 s   -> cartservice-0, container read I/O load, 15:46
  answer key                               -> cartservice, container packet loss, 15:45:31
```

**O que isso faz:**
- Mostra os turnos, as chamadas de ferramenta e o tempo de cada especialista, e o componente que cada um apontou.
- Compara com o gabarito: os especialistas de métricas e de traces chegaram ao serviço com falha, e o gabarito aponta perda de pacotes do contêiner.

Dois especialistas chegaram ao serviço com falha. O comentário do autor é que ambos culpam leituras de disco, enquanto o gabarito aponta pacotes perdidos.

> ❌ **Anti-padrão — confiar em uma resposta plausível:** a primeira hipótese do agente é tomada como causa. **Por que as pessoas fazem isso:** a resposta é fluente e confiante. **Consequência:** uma causa errada, como leituras de disco em vez de perda de pacotes, chega ao relatório. **Correção:** verificar cada citação e deixar que um modelo mais forte reconcilie vários achados independentes.

## Verificador de evidências e reconciliador
Agentes às vezes citam ids que nenhuma ferramenta lhes deu, e um id falso parece real. Pegá-lo não exige modelo algum:

```python
# kg/agents.py
def verify(hypotheses: list, tools: LabTools, vocab: dict, ws, we) -> tuple[list, Counter]:
    """Mechanical and cheap. Nothing here asks a model whether an answer seems right."""
    kept, why = [], Counter()
    for h in hypotheses:
        cited = set(map(str, h.get("evidence") or []))
        t = pd.to_datetime(str(h.get("datetime")), errors="coerce")
        if pd.notna(t):
            t = (t if t.tzinfo else t.tz_localize("Asia/Shanghai")).tz_convert("UTC")   # agents answer in UTC+8
        if not cited:
            why["no evidence"] += 1
        elif not cited <= tools.given:                     # an id its own tools never handed out
            why["cites evidence it was not given"] += 1
        elif h.get("component") not in vocab["components"]:
            why["component outside the list"] += 1
        elif h.get("reason") not in vocab["reasons"]:
            why["reason outside the list"] += 1
        elif pd.isna(t) or not ws - pd.Timedelta(minutes=5) <= t <= we:
            why["time outside the window"] += 1
        else:
            kept.append(h)
    return kept, why
```

**O que isso faz:**
- Verifica se cada id citado foi entregue pelas ferramentas, e se o componente e o motivo estão nas listas permitidas.
- Verifica se o horário cai dentro da janela (com uma margem de cinco minutos) e se a hipótese cita pelo menos uma evidência.
- Conta cada motivo de rejeição, para que o relatório mostre por que hipóteses foram descartadas.

Somente as hipóteses verificadas chegam ao **reconciliador**, um modelo mais forte (Qwen/Qwen3.8-Max), que pode fazer uma pergunta de acompanhamento a um especialista:

```python
# kg/agents.py
def reconcile(question: str, verified: dict, vocab: dict, allow_ask: bool = True) -> dict:
    """The strongest model reads verified hypotheses only, and may ask back once."""
    system = (f"You lead a root-cause investigation. Allowed components: {vocab['components']}. Allowed reasons: "
              f"{vocab['reasons']}. Prefer a cause that explains the other findings. Answer with JSON only: "
              '{"answer": [{"datetime": ..., "component": ..., "reason": ...}], "confidence": 0 to 1, "ask": null or '
              '{"specialist": ..., "question": ...}}.' + ("" if allow_ask else ' "ask" is null.'))
    body = "\n".join(f"{role}: {json.dumps(kept, default=str)}" for role, kept in verified.items())
    r = chat(MODELS["max"], [{"role": "system", "content": system}, {"role": "user", "content": f"{question}\n\nFindings:\n{body}"}])
    d = parse_json(r["text"]) or {}
    return {"answer": d.get("answer") or [], "confidence": d.get("confidence"), "ask": d.get("ask") if allow_ask else None}
```

**O que isso faz:**
- Entrega ao reconciliador somente as hipóteses verificadas de cada especialista, junto com as listas permitidas de componentes e motivos.
- Pede que ele prefira uma causa que explique os demais achados, e que responda em JSON com uma única pergunta opcional de volta a um especialista.

Uma investigação executa os três especialistas em paralelo, verifica as afirmações deles e as reconcilia:

```python
# kg/agents.py
def investigate(sig: pd.DataFrame, callers: dict, question: str, vocab: dict, ws, we) -> dict:
    """Specialists side by side, every finding verified, one answer."""
    tools = {role: LabTools(sig, callers) for role in SPECIALISTS}          # each keeps its own record of ids
    with ThreadPoolExecutor(len(SPECIALISTS)) as ex:
        found = dict(zip(SPECIALISTS, ex.map(lambda ro: run_specialist(ro, tools[ro], question, vocab), SPECIALISTS)))
    checked = {role: verify(f["hypotheses"], tools[role], vocab, ws, we) for role, f in found.items()}
    verified, rejected = {r: k for r, (k, _) in checked.items()}, sum((w for _, w in checked.values()), Counter())
    rec, asked = reconcile(question, verified, vocab), None
    if isinstance(rec["ask"], dict) and rec["ask"].get("specialist") in SPECIALISTS:   # one question, never a loop
        asked = rec["ask"]["specialist"]
        found[asked + "+"] = run_specialist(asked, tools[asked], f"{question}\nThe lead asks: {rec['ask'].get('question')}", vocab, 2)
        kept, why = verify(found[asked + "+"]["hypotheses"], tools[asked], vocab, ws, we)
        verified[asked], rejected = kept + verified[asked], rejected + why
        rec = reconcile(question, verified, vocab, allow_ask=False)
    return {**rec, "asked": asked, "found": found, "rejected": sum(rejected.values()),
            "calls": sum(f["turns"] for f in found.values()) + 1 + (asked is not None)}
```

**O que isso faz:**
- Executa os três especialistas lado a lado em um pool de threads, e verifica as hipóteses de cada um contra o seu próprio registro de ferramentas.
- Permite que o reconciliador faça uma pergunta de acompanhamento a um especialista, e nunca entra em laço.
- Conta as chamadas ao modelo, que somam 13 no teste.

Para testar o verificador, o autor acrescentou uma hipótese falsa que cita `s999999`, um id que nenhuma ferramenta produziu:

```text
#### OUTPUT ####
  verifier     planted 1 rejected ('cites evidence it was not given'), 9 real hypotheses pass
  answer       cartservice-0, container read I/O load, 2022-03-21 15:46:00, confidence 0.7
  asked back   None, 13 model calls, 33.2 s one after another, 13.1 s side by side
  score        partial 0.33 against the answer key
```

**O que isso faz:**
- Mostra a citação falsa plantada rejeitada como "cites evidence it was not given", enquanto nove hipóteses reais passam.
- Dá a resposta final, cartservice-0 com carga de leitura de I/O do contêiner às 15:46 de 21 de março de 2022 e confiança 0.7, com crédito parcial de 0.33.
- Mostra que o reconciliador não fez pergunta de acompanhamento, e que a execução sequencial levou 33.2 segundos contra 13.1 segundos em paralelo.

A citação falsa é pega, e a resposta acerta o pod e o minuto, mas erra o motivo. Na execução completa, a resposta vira um cartão de correção que aguarda uma pessoa.

## Adjudicação de rótulos
Os 146 relatórios de incidente da Wikimedia não têm gabarito, então modelos fizeram a rotulagem. Dois modelos de famílias diferentes precisavam concordar. Claude Opus 5.5 e Qwen/Qwen3.7-Max leram cada relatório, e o kappa de Cohen mede a concordância entre eles além do que o acaso daria.

```text
#### OUTPUT ####
  trigger        kappa 0.73
  detected by    kappa 0.92
  agreed on trigger and components: 100 of 146
```

**O que isso faz:**
- Reporta kappa de 0.73 para o gatilho e 0.92 para a detecção, que o autor lê como concordância entre os dois modelos.
- Mostra que os dois modelos concordaram sobre o gatilho e os componentes em 100 dos 146 relatórios.

> 🔎 Contexto adicional: o kappa de Cohen ajusta a concordância bruta pela concordância que dois avaliadores atingiriam por acaso, então 1.0 é perfeito e 0 é o nível do acaso.

Para os outros 46 relatórios, um modelo Claude e o Qwen/Qwen3.8-Max julgaram às cegas e concordaram em 27. Os outros 19 saíram do teste. Antes que qualquer dado de teste fosse aberto, as divisões, pisos e metas foram registrados com sha256, para que o desenho não pudesse ser alterado depois de ver os resultados.

## Grafo versus contexto bruto
Uma única consulta SPARQL casou cinco buscas em cinco fontes, em todos os 217 momentos de alerta com uma mudança. Depois, quatro braços leram os 24 incidentes de teste, como eram conhecidos 15 minutos após o início:

```text
#### OUTPUT ####
  arm                     trigger right   cites unseen   credits
  graph, one prompt                 42%             0%      0.20
  raw, one prompt                   25%             4%      0.20
  graph, specialists                33%             0%      3.38
  raw, specialists                  33%             0%      3.36
```

**O que isso faz:**
- Compara fatos do grafo com texto bruto, cada um em um único prompt e na configuração de especialistas.
- Mostra os fatos do grafo identificando o gatilho em 42% em um único prompt, contra 25% do texto bruto, e só o texto bruto citando evidências não vistas (4%).
- Mostra que os braços de especialistas custam cerca de 3.4 créditos, contra 0.20 de um único prompt.

Fatos do grafo superam o texto bruto em um único prompt, 42% contra 25%, e só o texto bruto citou evidências não vistas. Com 24 incidentes, porém, nenhuma diferença aqui passa de quatro incidentes, então o resultado é sugestivo, e não conclusivo.

## Comparação com benchmark
O RCAEval informa a cada método quando a falha foi injetada. O BARO, sua linha de base publicada mais forte no Train Ticket, pontua cada serviço pelo quanto suas métricas se deslocam depois desse momento. O ranqueador sem modelo do autor faz o mesmo de forma simples:

```python
"""exam.py: the rulers, a model-free ranker and exact tests."""
import pandas as pd
from scipy.stats import binomtest


def rank_rcaeval(metrics: pd.DataFrame, inject_time: int) -> list[str]:
    """Services by their most extreme metric after the fault, in normal spreads."""
    t = pd.to_numeric(metrics.iloc[:, 0], errors="coerce")                     # the first column is the clock
    normal, after, score = metrics[t < inject_time], metrics[t >= inject_time], {}
    for col in metrics.columns[1:]:
        x0 = pd.to_numeric(normal[col], errors="coerce").dropna()
        x1 = pd.to_numeric(after[col], errors="coerce").dropna()
        if "_" not in col or len(x0) < 10 or x1.empty:
            continue                                                            # too little history to know normal
        iqr = x0.quantile(0.75) - x0.quantile(0.25)
        scale = iqr if iqr > 0 else (x0.std() if x0.std() > 0 else 1.0)
        svc = col.rsplit("_", 1)[0]                                             # 'cartservice_cpu' -> cartservice
        score[svc] = max(score.get(svc, 0.0), float((x1 - x0.median()).abs().max() / scale))
    return [s for s, _ in sorted(score.items(), key=lambda x: -x[1])]
```

**O que isso faz:**
- Usa a primeira coluna como relógio e o momento da injeção como divisão entre os períodos normal e defeituoso.
- Pontua cada serviço pela sua métrica mais extrema depois da falha, medida em unidades da dispersão normal (IQR, ou desvio padrão como alternativa).
- Ignora qualquer métrica com menos de dez leituras normais, porque há histórico insuficiente para saber o que é normal.

Em todos os 360 casos do RCAEval, sem modelo e sem treinamento, o serviço injetado está entre os três primeiros palpites em 91% das vezes. No Train Ticket são 80%, contra [82% do BARO](https://arxiv.org/abs/2412.17015). O primeiro palpite acerta em 31% das vezes, contra 69% do BARO.

O OpenRCA oculta o momento da injeção. Congelados e executados uma vez no cluster reservado para teste, os agentes do autor obtiveram 16.9% do crédito parcial e 5.1% na pontuação estrita. Um [estudo de acompanhamento](https://arxiv.org/abs/2607.13548) executou seus próprios braços neste cluster, e todos superaram os agentes do autor, porque os detectores do autor escondem a causa antes que qualquer agente olhe.

## Veredictos das afirmações
Um teste exato decide cada uma das doze afirmações. O **teste de McNemar** conta apenas os casos em que dois métodos discordam, e sete comparações compartilham a **correção de Holm**.

```python
# kg/exam.py
def mcnemar(a: list, b: list) -> tuple[int, int, float]:
    """Exact McNemar on paired outcomes. Only disagreements count."""
    only_a, only_b = sum(bool(x) and not y for x, y in zip(a, b)), sum(bool(y) and not x for x, y in zip(a, b))
    return only_a, only_b, (binomtest(only_a, only_a + only_b).pvalue if only_a + only_b else 1.0)


def holm(p: dict) -> dict:
    """The k-th smallest of m p-values times m - k + 1, never decreasing."""
    adj, running = {}, 0.0
    for k, name in enumerate(sorted(p, key=p.get)):
        running = max(running, min(1.0, (len(p) - k) * p[name]))
        adj[name] = running
    return adj
```

**O que isso faz:**
- `mcnemar` conta os casos em que apenas um método acerta e usa um teste binomial exato sobre essas discordâncias.
- `holm` ajusta os p-valores para comparações múltiplas, para que a família de testes mantenha sua taxa de erro global.

> 🔎 Contexto adicional: o teste de McNemar compara dois métodos nos mesmos casos usando apenas os pares discordantes. A correção de Holm é um ajuste em degraus para vários testes ao mesmo tempo, o que torna cada p-valor individual mais rigoroso.

Uma afirmação precisa de todas as comparações apontando para o mesmo lado, com um p-valor corrigido abaixo de 0.05:

```text
#### OUTPUT ####
  comparison                          n   what we saw                          p   Holm p
  closed vocabulary repeats itself   10   only closed 9, only free 0       0.004    0.027
  topology over timing               41   first: topology 4, timing 2      0.500    1.000
  incidents over placebo            853   33/146 incidents, 103/707       0.018    0.111
  learned over manual, held out      78   better on 7, worse on 8          1.000    1.000
  three more with chat, raw text and coverage, each 1 incident against 0, Holm p 1.000
```

**O que isso faz:**
- Lista as comparações com as contagens, os p-valores brutos e os p-valores ajustados por Holm.
- Mostra que só a afirmação do vocabulário fechado sobrevive à correção, com Holm p de 0.027, enquanto as demais ficam em 0.111 ou acima.

Depois da correção, só o vocabulário fechado sobrevive. Cinco afirmações se sustentam, incluindo uma consulta que substitui três ferramentas. Duas falham, o limite de arestas e o volante, e cinco permanecem abertas ou não podem ser testadas.

## Replay de stream e dimensionamento da frota
O fluxo de logs da Netflix ultrapassa 38 milhões de eventos por segundo. Para medir uma máquina, o autor transmite todos os 525 milhões de eventos do OpenRCA, embaralhados em pequenos lotes, por meio de agregados por minuto.

```python
"""replay.py: the whole OpenRCA corpus as one mixed stream, read by worker processes against the clock."""
import multiprocessing as mp
import random
import time

import duckdb
import pyarrow as pa
import pyarrow.parquet as pq

COLUMNS = {"spans": ["ts", "entity", "duration_ms"], "logs": ["ts", "entity", "message"], "metrics": ["ts", "entity", "kpi", "value"]}
REPLAY_SQL = {   # the per-minute aggregates the detectors start from
    "spans": "SELECT entity, date_trunc('minute', ts) AS m, count(*) AS n, avg(duration_ms) AS ms, max(duration_ms) AS worst "
             "FROM batch GROUP BY ALL",
    "logs": "SELECT entity, date_trunc('minute', ts) AS m, count(*) AS n, count(*) FILTER (WHERE "
            "regexp_matches(message, '(?i)error|exception|fail|refused|timeout')) AS bad FROM batch GROUP BY ALL",
    "metrics": "SELECT entity, kpi, date_trunc('minute', ts) AS m, avg(value) AS v, max(value) AS peak FROM batch GROUP BY ALL",
}


def micro_batches(files: dict, seed: int = 42) -> list[tuple]:
    """Every row group of every file, shuffled like a live stream."""
    out = [(f, rg, kind) for kind, fs in files.items() for f in fs for rg in range(pq.ParquetFile(f).metadata.num_row_groups)]
    return random.Random(seed).sample(out, len(out))
```

**O que isso faz:**
- Define as colunas que cada tipo de dado precisa e uma consulta de agregação por tipo, para que cada lote seja reduzido a totais por minuto.
- `micro_batches` lista cada grupo de linhas de cada arquivo e os embaralha com uma semente fixa, para que o fluxo seja reproduzível e se pareça com tráfego ao vivo.

Cada worker percorre a sua parte dos lotes até o relógio acabar:

```python
# kg/replay.py
def worker(spec: dict) -> dict:
    """One process: its share of the batches, until the clock runs out."""
    pa.set_cpu_count(spec["threads"])
    con = duckdb.connect(config={"threads": spec["threads"], "TimeZone": "UTC"})
    files, done, rows, start = {}, [], [], time.time()
    while time.time() < start + spec["seconds"]:
        path, rg, kind = spec["batches"][len(done) % len(spec["batches"])]
        if path not in files:
            files[path] = pq.ParquetFile(path)
        batch = files[path].read_row_group(rg, columns=COLUMNS[kind])        # one row group is one micro-batch
        con.register("batch", batch)
        con.sql(REPLAY_SQL[kind]).to_arrow_table()
        con.unregister("batch")
        done.append(time.time())
        rows.append(batch.num_rows)
    return {"start": start, "done": done, "rows": rows}
```

**O que isso faz:**
- Cada processo lê um grupo de linhas por vez, que é um micro-lote, e executa a agregação sobre ele.
- Conta as linhas à medida que avança, e para quando o limite de tempo é atingido.

A taxa é medida do primeiro início até o último lote concluído:

```python
# kg/replay.py
def replay(batches: list, processes: int, seconds: float, cores: int = 16) -> dict:
    """Events a second, from the first start to the last finished batch."""
    specs = [{"batches": batches[k::processes], "seconds": seconds, "threads": max(1, cores // processes)}
             for k in range(processes)]
    with mp.get_context("spawn").Pool(processes) as pool:
        res = pool.map(worker, specs)
    t0, t1 = min(r["start"] for r in res), max(r["done"][-1] for r in res)
    events = sum(sum(r["rows"]) for r in res)
    return {"processes": processes, "seconds": round(t1 - t0, 1), "events": events, "events_per_s": events / (t1 - t0)}
```

**O que isso faz:**
- Divide os lotes entre os processos e dá a cada um uma parte igual dos núcleos.
- Divide os eventos pelo tempo de relógio do primeiro início ao último término, então a taxa é honesta.

O autor executou de 1 a 16 workers por 45 segundos cada, e depois 16 por dez minutos:

```python
from kg.replay import micro_batches, replay

batches = micro_batches(openrca_files)        # {kind: Parquet files}, under if __name__ == "__main__" in a script
long = replay(batches, 16, 600)
print(f"  ten minutes: {long['events']:,} events, {long['events_per_s'] / 1e6:.1f}M a second")

#### OUTPUT ####
  ten minutes: 15,723,611,150 events, 26.1M a second
```

**O que isso faz:**
- Executa dezesseis workers por dez minutos sobre o fluxo embaralhado.
- Reporta 15,723,611,150 eventos a 26.1 milhões por segundo.

O replay rodou a 8.4 milhões de eventos por segundo com um worker e 26.4 milhões com dezesseis. Ao longo de uma execução de dez minutos, dezesseis workers sustentaram 26.1 milhões. O fluxo da Netflix precisa, portanto, de cerca de 1.5 desses laptops, ou 2.0 se os eventos chegarem como JSON. O grafo vê apenas sinais: um sinal condensa 82,681 eventos, então o fluxo se torna 2,753 triplas por segundo. A execução completa carregou 16,393,203 triplas em um único armazenamento. Com baldes por hora, uma consulta típica ficou abaixo de 1.4 ms, independentemente do tamanho do armazenamento, e as escritas ao vivo, com cada lote validado pelas formas antes do commit, chegaram a 39,871 triplas por segundo, o que basta para o fluxo quatorze vezes.

> ❌ **Anti-padrão — gravar eventos brutos no grafo:** cada evento vira uma tripla. **Por que as pessoas fazem isso:** assim tudo fica guardado. **Consequência:** o armazenamento cresce com o fluxo, e não com os incidentes. **Correção:** condensar os eventos em sinais antes e armazenar apenas os sinais no grafo.

## Limites da arquitetura
Este estudo nunca tocou na telemetria própria da Netflix. O autor lista os outros limites, com os detectores como o elo mais fraco. Três partes ficam no repositório e foram deixadas de fora do texto: o cliente de coleta da Wikimedia e os pseudônimos, o conversor de pageviews e os detectores, que o autor diz que devem ser construídos quando uma nova fonte for adicionada; o compilador de formas e os escritores em massa do armazenamento, que são necessários acima de um milhão de triplas; e o volante, os episódios de alerta, os tipos de mudança, a impressão digital do plano e o cartão de correção, que são necessários quando as pessoas agem com base nas respostas.

Todo o código está no [repositório GitHub](https://github.com/FareedKhan-dev/ontology-driven-observability) do autor.

## 🧠 Modelos mentais
### Nomes compartilhados antes dos joins
**O modelo:** fatos só se unem quando cada máquina tem um nome canônico, e os aliases são a primeira falha em qualquer junção de incidentes.
**Como aplicar:** antes de construir um grafo, defina uma única função de URI e teste se grafias diferentes de uma mesma máquina resolvem para o mesmo nó.

### Formas de mundo fechado no momento da escrita
**O modelo:** os dados só entram no armazenamento quando passam nas regras, então fatos ruins são recusados em vez de limpos depois.
**Como aplicar:** coloque verificações de tipo, de intervalo e de vocabulário nas formas, valide apenas o novo delta e desligue a inferência no validador.

### O silêncio exige cobertura
**O modelo:** "nada disparou" só significa algo se a fonte foi de fato lida.
**Como aplicar:** registre a cobertura por execução como ok, partial ou missing, e responda "desconhecido" quando uma fonte não foi coberta.

### Listas fechadas para modelos, verificações em código
**O modelo:** um modelo escolhe entre rótulos enumerados e cita apenas ids que as ferramentas retornaram, e o código verifica as duas coisas.
**Como aplicar:** leia os valores permitidos a partir do esquema, valide cada resposta e rejeite qualquer citação que as ferramentas nunca entregaram.

### Topologia acima do tempo
**O modelo:** uma causa é crível quando o grafo de chamadas a liga ao sintoma, e não apenas porque aconteceu primeiro.
**Como aplicar:** condicione cada salto ao grafo de chamadas e compare qualquer pontuação com uma linha de base de momentos aleatórios antes de confiar nela.

## 🔑 Principais conclusões
- Unir ferramentas de incidente exige um nome canônico por máquina: o índice de 44% de correspondência com o catálogo hoje mostra o quanto aliases e hosts desativados quebram as junções.
- Formas verificadas no momento da escrita pegaram todas as oito violações plantadas, e um número de texto em nível SPARQL quebrou silenciosamente um filtro em dois motores.
- A topologia do grafo de chamadas elevou a precisão no top três de 4.9% (somente por tempo 9.8%) para 26.8% em 41 falhas de desenvolvimento.
- Um vocabulário fechado fez o modelo repetir seus rótulos em 90% das conversas, contra nenhuma repetição com JSON livre.
- O verificador pegou uma citação plantada a um id que nenhuma ferramenta produziu, sem consultar nenhum modelo.
- No cluster de teste reservado do OpenRCA, os agentes do autor obtiveram 5.1% na pontuação estrita e 16.9% na parcial, e os detectores, não os agentes, eram o gargalo.
- Um único laptop sustentou 26.1 milhões de eventos por segundo, o que, segundo o autor, cobre o fluxo da Netflix com cerca de 1.5 máquinas.

## 🏛️ Para o Kenji
Sua experiência com watsonx.governance se encaixa bem na camada de proveniência: grafos nomeados por execução e consultas "as of" formam uma trilha de auditoria, e os registros de cobertura são a evidência de que um controle realmente olhou. Em revisões de arquitetura, isso é um padrão reutilizável para qualquer sistema que precise explicar suas decisões passadas. A pergunta que ele levanta para você: quais dos seus sistemas atuais falhariam no teste de "nada disparou versus ninguém olhou", e como você saberia? Esta é a sua interpretação, não a do autor.

## 📚 Plano de leitura
### A partir do artigo
- [Blog técnico da Netflix: observabilidade orientada por ontologia](https://netflixtechblog.com/ontology-driven-observability-building-an-end-to-end-knowledge-graph-at-netflix-scale-421392cf5adf) — o design original que esta reconstrução segue.
- [Palestra na QCon London (InfoQ)](https://www.infoq.com/presentations/netflix-observability-aiops-ontology-scale/) — a apresentação da própria Netflix sobre a abordagem.
- [Repositório GitHub](https://github.com/FareedKhan-dev/ontology-driven-observability) — o código completo de cada módulo desta nota.
- [BARO (arXiv 2412.17015)](https://arxiv.org/abs/2412.17015) — a linha de base publicada mais forte no Train Ticket.
- [Estudo de acompanhamento (arXiv 2607.13548)](https://arxiv.org/abs/2607.13548) — os braços com que o autor se compara no cluster do OpenRCA.

### Para ir além
- OpenRCA e RCAEval — os datasets públicos com gabaritos usados nos testes.
- W3C PROV-O e SHACL — o vocabulário de proveniência e a linguagem de formas por trás do armazenamento e do validador.

## Conceitos
[Harness de agente](/pt-br/concepts/ai-ml/agent-harness.md) · [Modelo de decisão estruturado](/pt-br/concepts/ai-ml/structured-decision-model.md) · [Calibração](/pt-br/concepts/ai-ml/calibration.md) · [Grafo de conhecimento](/pt-br/concepts/data/knowledge-graph.md)
