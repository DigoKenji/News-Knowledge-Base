---
type: Article Summary
title: Building an Agentic Knowledge Graph for Billions of Events
description: Ontology-driven observability links fragmented incident tools in one knowledge graph; the public-data rebuild
  works, but detectors remain the weak link.
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
---

## 🎯 Core thesis
Connecting incident tools through one shared, validated knowledge graph lets an investigator answer "what changed, where, and when" in one place, and the author's public-data rebuild shows the approach works while its detectors remain the weakest link.

## 🧭 Why the author wrote it
When Mike Tyson fought Jake Paul in November 2024, Netflix delivered the fight to roughly 65 million concurrent streams while logging over 38 million events a second. When the home screen rendered 1.5 seconds slower, the alert fired, but in Netflix's own example the fix took about four hours, mostly spent rebuilding facts the tools already knew, because one tool said "application" where another said "service". If the thesis is right, a single shared agreement on names, facts and time turns that rebuilding into a query.

## Rebuilding Netflix-inspired observability with AI
The author rebuilds Netflix's design on public data, with no affiliation to Netflix. The rebuild has seven parts: a telemetry lake of 1,922,290,480 real rows in 15.24 GB of Parquet; identity and an ontology in which shapes refuse bad facts; a provenance store with one named graph per run; enrichers and an extraction ladder that records coverage; correlation that follows the call graph; root-cause agents with typed tools, an evidence verifier and a reconciler; and an exam plus a stream replay. The final result is that call-graph chains put the broken component in the top three 26.8% of the time, and one laptop sustains 26.1 million events a second.

The project layout is:

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

**What this does:**
- Splits the project into an `ontology/` folder of Turtle files that are version-controlled as data, and a `kg/` folder of Python modules, one per pipeline stage.
- Keeps the vocabulary (`ops.ttl`) and its rules (`shapes.ttl`) apart from the code that enforces them.

## Incident Context Gap
On 17 April 2024 at 09:18 UTC, MediaWiki on Kubernetes in Wikimedia's eqiad datacentre began failing. A change had moved the mcrouter proxy into its own service, and DNS lookups climbed from roughly 40,000 per second to 110,000. The published write-up, 1.3 days later, blamed a hostname missing its final dot.

Every tool held one piece of that story in its own names. One database machine appears as `db1127.eqiad.wmnet:3306` in an alert, as "the s7 primary" in chat and as `db1127` in the deploy log. Across 89 written timelines, the median Wikimedia incident was noticed in 2 minutes and fixed in 40, but understood only after 62 minutes.

Because Netflix's telemetry is private, the author uses public records of the same shape: Wikimedia's on-call chat, change log, alerts and incident reports, which are the four sources Netflix joins. OpenRCA and RCAEval add metrics, logs and traces with answer keys, and Wikipedia pageviews add volume. Before looking at any data, the author listed twelve testable claims in Netflix's design, and the final sections test them one by one.

## Environment Setup
The build needs Python 3.12 and about 25 GB of disk. The full run used a 16-core laptop with 32 GB of memory. The project is created with uv, and every download stays inside it.

```bash
uv init ontology-observability --python 3.12 && cd ontology-observability && mkdir -p kg ontology
uv add duckdb pandas pyarrow rdflib pyshacl pyoxigraph httpx huggingface-hub scipy psutil
export KG_CONTACT="you@example.org"              # Wikimedia asks every script for a contact
export LLM_BASE_URL="https://your-endpoint/v1"   # any OpenAI-compatible server
export LLM_API_KEY="your key"
```

**What this does:**
- Creates the project with Python 3.12 and installs the DuckDB, RDF, SHACL, Oxigraph and HTTP libraries used in the rest of the note.
- Sets the contact string Wikimedia asks for, plus an OpenAI-compatible endpoint and key read from the environment, so no secret is written into code.

Every module reads `config.py`, which holds paths, DuckDB limits, a model per role and a credit budget per phase.

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

**What this does:**
- Reads the profile, paths, contact and endpoint from environment variables with safe defaults, so one file serves a smoke test and the full run.
- Names three models by role (flash, max, reader), with a price in credits per call used by the budget check.
- Caps spending per phase at 1,200 credits in total, with a 35-credit reserve.

The build relies on these tools, and the imports plus the machine details are checked with a short script:

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

**What this does:**
- Imports DuckDB for SQL over Parquet, Oxigraph for the triple store, pyshacl for SHACL validation, rdflib for RDF graphs, and psutil for the hardware report.
- Prints the versions and core count, so every timing in the note can be traced to this exact machine and software set.

## Telemetry Lake
A polite harvest, with pseudonyms for every person, returned 1,730,378 chat lines, 538,717 logged changes and 414 incident reports. OpenRCA's mirror holds 162 CSV files totalling 68.65 GB, from a bank, a telecom operator and a shop, and the author converts them one at a time.

The DuckDB connection uses every core and spills to disk:

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

**What this does:**
- Opens one DuckDB connection with all 16 threads, a memory cap and a spill folder, so data larger than memory still processes.
- Sets the session time zone to UTC, so timestamps from different sources line up.

Three systems use three different span layouts and clocks, so each gets its own select. The CSV is deleted only after the row counts match:

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

**What this does:**
- Maps each system's column names to one trace layout, including its own timestamp units; the market system reports microseconds, so it is divided by 1000.
- Writes to a temporary file, asserts that the row count matches the source, and only then renames the file, so a half-written Parquet file never gets a real name.
- Deletes the CSV only after the check passes, which keeps about 68 GB of CSV from sitting on disk at once.

One day of the running example is converted end to end:

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

**What this does:**
- Downloads one trace file from the dataset, converts it to Parquet with the market layout, and prints the span count.
- The printed check confirms that the CSV was deleted after the rows were counted.

Over all 162 files, 68.65 GB shrank to 7.11 GB with no row lost. Pageviews and RCAEval complete the lake:

```text
#### OUTPUT ####
  Wikimedia  operations record              4,609,863    0.16 GB
  Wikimedia  pageviews, page x hour     1,142,762,478    4.64 GB
  OpenRCA    metrics, logs, spans         524,547,719    7.11 GB
  RCAEval    metrics, logs, spans         250,370,420    3.32 GB
  total                                 1,922,290,480   15.24 GB
```

**What this does:**
- Lists the four parts of the lake by source, with row counts and sizes; the total is 1,922,290,480 rows in 15.24 GB.
- The Wikipedia pageviews alone account for 1,142,762,478 rows, which is most of the volume.

The failure followed from here on is cartservice at 07:45 UTC, which appears in metrics, traces and logs. DuckDB needs less than six seconds to scan all 397 million spans.

## Identity Resolver
Before two facts can be joined, one machine needs one name. At Wikimedia the first digit of a host's number names its datacentre, so the identity code uses that digit to derive the location.

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

**What this does:**
- Maps the first digit of a host number to a datacentre, such as 1 to eqiad, which gives each host a location without a lookup.
- Uses one regular expression to strip the suffixes, ports and management prefixes that different tools add, then returns one canonical name.
- Returns nothing for strings that only look like hosts, such as a task id or a version number, so they never join the graph.

`DB1127.eqiad.wmnet:3306` and `elastic1124.mgmt.eqiad.wmnet` resolve to hosts in eqiad, while the task id `T316186` resolves to nothing. The catalogue then gives each host a role, a team and a cluster, and time is the weak link. Of 5,593 hosts in five years of logs, only 44% match the catalogue today. It knows 81% of 2026's hosts but only 18% of 2021's, because machines retire.

## Ontology as Code
Every fact the store holds is a **triple**, such as "this deploy happened on db1127", and one file decides which triples may exist. Netflix splits that vocabulary into twelve namespaces, and the author adds two. Nothing is ever just an `ops:Component`, and every signal type sits below `corr:Signal`. The vocabulary is a Turtle file in git, where for example `ops:Host` is a kind of `ops:Component`. The full vocabulary is 272 triples, 43 classes and 77 properties.

The module that loads the vocabulary and the shapes is:

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

**What this does:**
- Loads the class and property file (`ops.ttl`) and the rules file (`shapes.ttl`) once at import time, so every module shares one definition.
- Defines one namespace per source (alerts, metrics, logs, timelines, incidents, correlation and so on), matching Netflix's twelve plus the author's two.

A single function mints each URI, and a second maps a moment to its hour node:

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

**What this does:**
- `slug` lowercases and cleans any text, so `DB1127` with a stray space and `db1127` produce the same URI.
- `mint` builds every URI from tenant, kind and key, so the same thing always gets the same identifier.
- `hour_node` makes each hour a node of its own, which turns "what changed in this window" into a lookup instead of a scan.

## Shape Validation Layer
An ontology says what may exist, but it cannot stop bad data. **SHACL shapes** can, acting as closed-world rules that a fact must pass before it is stored. The author's example is a confidence stored as the text "0.92". It breaks a filter in two engines: rdflib returns all three rows, even the word "high", while Oxigraph returns none, and neither reports an error. The fix belongs at write time. The rules file holds sixteen rules, and its chain shape requires every `corr:confidence` to be an `xsd:decimal` between 0 and 1.

> ❌ **Anti-pattern — RDFS inference on the validator:** the first validator ran with RDFS inference enabled, so it concluded that a misspelt host must be a component. **Why people do it:** inference looks like free completeness. **Consequence:** bad names become typed facts that look valid. **Fix:** validate with `inference="none"` and judge only what the data actually says.

The checker judges only the nodes a new delta writes, with their stored context, and infers nothing:

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

**What this does:**
- Runs pyshacl with inference turned off, so the shapes judge exactly the facts that were written.
- Limits the check to the subjects of the new delta, and adds the store's existing facts about them as context.
- Turns each violation into a row with the node, the property, the rule that failed and the message, sorted for a stable report.

The author planted six broken nodes in a small file and ran the check:

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

**What this does:**
- Loads a file with six deliberately broken nodes and two good ones, then prints the first four violations.
- Each row names the failing node, the property, the rule (Datatype, MaxInclusive, In and so on) and the message from the shape.

All eight violations are caught.

## Provenance Store
Where did a fact come from, and when was it known? Each fact lives in the **named graph** of the run that wrote it, and a W3C PROV-O record describes every run. The store is Oxigraph, with the class tree in a graph of its own, so subclass queries can follow the hierarchy.

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

**What this does:**
- Uses Oxigraph as an on-disk or in-memory quad store, so each fact is stored with the graph it came from.
- Loads the class tree into its own named graph at start-up, so queries such as "subclass of Signal" work.
- Writes datetimes in one format and keeps the PROV namespace in one place.

Writing a run again replaces it, so rebuilds never duplicate:

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

**What this does:**
- Gives each run one named graph, keyed by tenant, enricher and run id, so a rerun finds and replaces its own graph.
- Writes a PROV-O record with the start and end times, the number of triples added and the sources the run read.
- Links the graph to its run record, so the provenance survives in the store next to the facts.

Dropping a bad run removes its facts and its record together. `context` collects the stored facts around a delta:

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

**What this does:**
- `drop` removes the run's facts and its PROV-O record, leaving nothing behind.
- `ask` lets a caller name the only graphs it may read, so an agent can be restricted to certain runs.
- `context` gathers what the store already says about the nodes in a new delta, which the shape check needs.

Each signal records when it happened and when the system could have known it. An agent at 09:33 must not see a report written the next day:

```python
# kg/store.py
AS_OF = """SELECT ?s WHERE {{ ?s a/rdfs:subClassOf* corr:Signal . ?s corr:at ?t . ?s corr:detectedAt ?d .
  FILTER(?d <= {asof} && ?t >= {since}) }}"""


def visible(store: GraphStore, asof, since, graphs: list | None = None) -> set[str]:
    # a fact written after `asof` does not exist yet
    return {r["s"].value for r in store.ask(AS_OF.format(asof=dt_literal(asof).n3(), since=dt_literal(since).n3()), graphs)}
```

**What this does:**
- Requires each signal to have a time of occurrence (`corr:at`) and a detection time (`corr:detectedAt`).
- Keeps only signals detected before the query time, so "as of" queries never see the future.

In the full run, the April incident's report carried a **canary**, a fact that must never reach an agent. The results were:

```text
#### OUTPUT ####
  asked as of                all graphs  canary?  agent graphs  canary?
  2024-04-17 09:33 UTC              105       no           105       no
  2024-04-18 16:00 UTC              177      yes           175       no
  rollback of a bad run: 2,067 quads before, 1,322 after
```

**What this does:**
- Shows the canary absent at 09:33 UTC on 17 April and present by 16:00 UTC on 18 April in the full store.
- Shows that the agent's graphs never contain the canary, and that rolling back a bad run cut the store from 2,067 quads to 1,322.

The canary appears only after the report exists, and never in an agent's graphs. Through the hour node, "what changed in the hour before?" took 0.50 ms instead of 85.55 ms.

## Enricher Contract
Facts arrive through **enrichers**, small programs that each read one source. Every run records its **coverage**, because "nothing fired" and "nobody looked" are different answers.

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

**What this does:**
- Defines a `Coverage` record with a source, a time window, a status of `ok`, `partial` or `missing`, and a detail string.
- Keeps the three-way status so that a silent source cannot be mistaken for a quiet one.

A dead source costs coverage, never the harvest:

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

**What this does:**
- Calls `fetch` inside a try block, so an exception only marks the source as `missing` and records the error text.
- Runs the pure `analyse` step only when data was fetched, which keeps the fetch and the reasoning separate.

Each enricher declares what it needs and what it makes, and the standard library's topological sort derives the run order:

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

**What this does:**
- Builds a map from each class to the enricher that produces it, then refuses to start if any required class has no producer.
- Sorts enrichers by name first, so the plan never changes between runs, and then applies `TopologicalSorter`.
- Raises an error on a dependency loop, because `TopologicalSorter` reports a cycle.

Listed in any order, the host enricher still runs first, and an enricher whose input nobody makes is refused before anything runs. The pipeline checks each delta with its stored context, and quarantined facts still leave a coverage record:

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

**What this does:**
- Runs each enricher in the planned order and checks its facts against the shapes, using the stored context.
- If a delta breaks a shape, marks the coverage `partial`, drops the facts and still writes the coverage record.
- Writes each enricher's output as its own named graph, so each source's results can be traced and dropped separately.

Coverage gives a question three answers rather than two:

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

**What this does:**
- Returns "yes" if an alert fired on the host in the half hour before the time asked about.
- Returns "no" only if the alert source was read in that window and nothing fired.
- Otherwise returns "unknown", because the source was never covered.

With Loki switched off, a quiet host's "no" became "unknown". The last guard is the catalogue gate:

```python
# kg/enrich.py
SERVICE_LIKE = re.compile(r"(?<![\w.-])([a-z][a-z0-9]+(?:-[a-z0-9]+)+)(?![\w-])")


def gate(texts, is_host, in_catalogue) -> Counter:
    # service-looking tokens the catalogue refuses, with counts
    seen = Counter(tok for t in texts for tok in SERVICE_LIKE.findall(t.lower()) if not is_host(tok))
    return Counter({tok: n for tok, n in seen.items() if not in_catalogue(tok)})
```

**What this does:**
- Finds service-like tokens in alert text, such as `mw-web`, that are not known hosts.
- Counts them and keeps only those the catalogue does not recognise, so the results show what the gate would refuse.

Of 3,841 such strings in Loki alerts, the catalogue knew 107 and refused 3,734. Without the gate, each refused name would become an ownerless application in the graph.

> ❌ **Anti-pattern — ungated names as entities:** a service-looking string is written straight into the graph. **Why people do it:** it is faster than checking a catalogue. **Consequence:** the graph fills with applications nobody owns, which cannot be routed or fixed. **Fix:** gate each name against the catalogue and keep the refusals visible.

## Extraction Ladder
Incident chat is free text. Netflix's write-up climbs five rungs, cheap ones first, with a model on the fourth. On 13,331 human chat lines around 128 incidents, the first two rungs get this far:

```text
#### OUTPUT ####
  after layer                      text typed   lines linked
  1 regular expressions                  2.0%           8.8%
  2 catalogue validation                 2.7%           7.1%
```

**What this does:**
- Shows the share of lines with typed text, and the share linked to a known entity, after rungs 1 (regular expressions) and 2 (catalogue validation).
- Shows that 93% of lines still link to nothing, which is why a model is needed.

Every model call goes through one client with a cache, a ledger and at most six calls in flight:

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

**What this does:**
- Opens a SQLite cache and a JSONL ledger, so answers are reused and every paid call is recorded across restarts.
- Creates one HTTP client with a bearer key and sets a semaphore for six concurrent calls.
- Sets up the cache table and the lock used for writes.

The budget is checked before a call is sent, and a cached question is free and repeats its answer:

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

**What this does:**
- Hashes the full request body, so the same question always maps to the same cache key.
- Returns a cached reply for free, and refuses a new call that would take a phase over its credit cap.
- Retries rate-limited or server-error replies with a growing wait, and logs every answered call.

One helper pulls JSON out of a reply, even when it sits inside a code fence:

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

**What this does:**
- Finds the first curly-brace object in the reply and parses it.
- Returns `None` rather than raising when the reply is not valid JSON, so the caller can decide what to do.

The model must not invent categories. Its vocabulary is read from the shapes file:

```python
# kg/ontology.py
def closed_list(shape: str, path: URIRef) -> list[str]:
    # the sh:in list of one property
    prop = next(p for p in SHAPES.objects(URIRef(f"{BASE}shapes#{shape}"), SH.property) if SHAPES.value(p, SH.path) == path)
    return [str(v) for v in Collection(SHAPES, SHAPES.value(prop, SH["in"]))]
```

**What this does:**
- Reads the allowed values of one property from the `sh:in` list in the shapes file.
- Means the category and sentiment lists exist in one place, and the prompt cannot drift from the validator.

The reply is checked in code before anything is written:

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

**What this does:**
- Builds the system prompt from the closed lists, so the model is told its only allowed categories and sentiments.
- The validator accepts a reply only if category and sentiment are on the list, `incident_related` is a boolean and every mention is a candidate id.

Here the model labels the ten conversations around the April incident:

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

**What this does:**
- Sends each conversation with its candidate ids to the flash model in the classification phase.
- Prints only the replies that pass validation and are incident-related, so invalid output never reaches the graph.

The printed rows show three incident-related conversations, two of them urgent. Model answers come from the author's recorded run on 31 October and 1 November 2026, with shortened prompts. Asked three times at temperature 0.7, the labels repeated 90% of the time with the closed vocabulary. With free JSON they never repeated, and Claude with structured outputs managed 70%.

> ❌ **Anti-pattern — free-form model labels:** the model answers with whatever category it likes. **Why people do it:** JSON mode feels like enough structure. **Consequence:** the same conversation gets different labels on each run, and the labels cannot be counted. **Fix:** give the model a closed list read from the schema and validate every reply in code.

This is the [structured decision model](/concepts/ai-ml/structured-decision-model.md) pattern applied to chat: typed outputs with fixed choices rather than free text.

## Correlation Engine
Every incident review asks what changed just before. Wikimedia logged 283,743 changes, one every 10.1 minutes, and the correlation code starts by counting how many changes sit near a moment.

```python
"""correlate.py: how often a change is nearby by chance, and chains from a cause to a symptom."""
import numpy as np


def changes_before(moments: np.ndarray, minutes: float, times: np.ndarray) -> np.ndarray:
    """Changes in (t - minutes, t] for every moment t, by binary search."""
    return np.searchsorted(times, moments, side="right") - np.searchsorted(times, moments - minutes * 60, side="right")
```

**What this does:**
- Uses binary search on sorted change times, so counting changes in each window is fast even for hundreds of thousands of changes.
- Returns, for every moment, the number of changes in the window before it.

Dropped at 20,000 random moments, it finds a change in the hour before 58.9% of them. A deploy precedes 40.4% of incidents within 30 minutes, against 31.4% of the same hours in other weeks.

> ❌ **Anti-pattern — timing-only correlation:** a change is blamed because it came first. **Why people do it:** timing is cheap and intuitive. **Consequence:** production is always busy, so timing mostly measures activity, not causes. **Fix:** require a plausible path on the call graph before linking.

The detectors compare each OpenRCA window with the sixty minutes before it and flagged 863,062 minutes. Each signal needs the answer key's component names:

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

**What this does:**
- Maps each measurement to component names, with special handling for the market system's node and pod naming.
- Returns the raw entity for bank and telecom, which already use the key's names.

A signal may explain another only if the second component calls the first, directly or one step up, or runs on it:

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

**What this does:**
- `related` accepts a link when the two components are the same service family, when one runs on the other's node, or when the call graph links them directly or through one intermediate caller.
- `chain_inputs` gives each signal a severity between 0 and 1, using a logarithm so that a z-score of 50 or more is fully severe, and keeps the strongest few per component and kind.

A hop is scored by the rules in the write-up. A recognised cause and effect earns 0.9, and any other hop earns a score built from closeness in time and severity, inside a 10-minute window. The author adds one change: lineage may stretch that window.

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

**What this does:**
- Gives a known causal pair, such as deployment to log error, a fixed score of 0.9.
- For any other hop, combines time closeness (60%) and severity (40%), which is the write-up's rule without its 0.6 floor.
- `may_link` refuses backward links, and refuses links outside the window unless lineage says the deploy shipped the commit.

The depth-first search follows the five best next hops towards the symptoms:

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

**What this does:**
- Sorts signals by time and keeps only the five best next hops from each signal.
- Walks from every signal towards the symptoms, keeping the chain with the highest mean score, up to four hops.

Components rank by their best chain, and unreached components follow, loudest first:

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

**What this does:**
- `rank_loudest` is the baseline: components sorted by their most severe signal.
- `rank_components` ranks components by the best chain that reaches a symptom, and places the ones no chain reaches after them, in loudness order.

The engine must first reproduce the write-up's example chain:

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

**What this does:**
- Builds the write-up's example chain (commit, deployment, log error, alert, incident) with severity 0.825.
- Checks that the engine returns 0.870 for the full path, and that without lineage the 14-minute deploy hop is refused.

It reproduces 0.87, and without lineage the 14-minute hop is refused.

On the 41 development failures with a named component, the topology lifts accuracy substantially:

```text
#### OUTPUT ####
  ranking                  AC@1     AC@3
  loudest signal           4.9%     4.9%
  timing-only chains       4.9%     9.8%
  topology chains          9.8%    26.8%
```

**What this does:**
- Compares three rankings: the loudest signal, timing-only chains and topology chains.
- Shows that topology chains raise top-three accuracy (AC@3) from 4.9% to 26.8%.

Only 6% of the chains scoring above 0.9 pointed at the correct component, for a Brier score of 0.871. The score orders causes, and it is not a probability. The [calibration](/concepts/ai-ml/calibration.md) of the chain score is poor, so it should be read as a ranking.

> 🔎 Added context: the Brier score measures the mean squared gap between predicted probabilities and actual outcomes, where 0 is perfect and higher is worse. A score of 0.871 therefore shows the chain score is not a usable probability.

## Knowledge Flywheel
Each resolved failure comes with its answer, and every answer can teach the next investigation where to look. The full run learned how much more often each kind of signal sits on a root cause than on a bystander. Graded on lifts learned from the other failures, it put the right component first in 29.3% of them, against 4.9% for loudness. On a cluster it had never seen, it beat hand-written rules on 7 failures and lost on 8.

## Root-Cause Specialists
Reading the evidence is the job of specialist agents. Each is an [agent harness](/concepts/ai-ml/agent-harness.md): a fixed model wrapped in typed tools, a turn limit and a required output shape. A specialist never reads raw data. Its typed tools return rows with ids it may cite, and they remember every id they gave out.

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

**What this does:**
- Keeps the signal table with a unique id per row, and a record (`given`) of every id any tool has handed out.
- Returns at most twelve rows per call, sorted by strength, so a tool cannot flood the context.
- Formats times in the answer key's UTC+8 clock, so the agent and the key agree.

Each tool is a short method whose docstring the model reads:

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

**What this does:**
- `top_anomalies` lists the strongest anomalies, optionally by kind.
- `component_signals` lists every anomaly on one component.
- `first_anomaly` returns the first anomaly on a component and records its id as given.
- `neighbours` returns callers and callees from the traces.

The tool spec is built from the code, and a bad argument returns an error the agent can read:

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

**What this does:**
- Builds each tool's schema from its method name, its docstring and its parameter names, so the code is the specification.
- Catches any exception raised by a tool and returns it as data, so the agent can correct itself.

The three specialists are given their tools and a brief each. Specialist roles are shown below:

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

**What this does:**
- Gives the metrics, traces and logs specialists different tool sets and briefs; traces, for example, are told that a slow caller is often the victim of a slow callee.
- Requires the same JSON output for all three: a hypothesis with a component, reason, time in UTC+8, evidence ids and a confidence, at most three.
- Runs each specialist for at most three rounds of tools and then one turn without tools, so it must answer.

The loop that runs each specialist:

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

**What this does:**
- Sends each specialist its tools as a schema, executes every tool call it makes, and feeds each result back as a tool message capped at 6,000 characters.
- Withholds tools on the final turn, so the specialist must answer, and returns its best three hypotheses with the trail of tool names, turns and seconds.

The three specialists ran on the flash model, with these results:

```text
#### OUTPUT ####
  metrics  4 turns, 7 tool calls, 11.3 s   -> node-6, node memory consumption, 15:48
  traces   4 turns, 5 tool calls,  9.1 s   -> cartservice-0, container read I/O load, 15:48
  logs     4 turns, 6 tool calls, 11.0 s   -> cartservice-0, container read I/O load, 15:46
  answer key                               -> cartservice, container packet loss, 15:45:31
```

**What this does:**
- Shows each specialist's turns, tool calls and time, and the component it blamed.
- Compares them with the answer key: the metrics and traces specialists reached the failing service, and the key names container packet loss.

Two specialists reached the failing service. The author's comment is that both blame disk reads while the answer key names lost packets.

> ❌ **Anti-pattern — trusting a plausible answer:** the agent's first hypothesis is taken as the cause. **Why people do it:** the reply is fluent and confident. **Consequence:** a wrong cause, such as disk reads instead of packet loss, reaches the report. **Fix:** verify every citation and let a stronger model reconcile several independent findings.

## Evidence Verifier and Reconciler
Agents sometimes cite ids that no tool gave them, and a fake id reads like a real one. Catching it needs no model:

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

**What this does:**
- Checks that every cited id was handed out by the tools, and that the component and reason are on the allowed lists.
- Checks that the time falls inside the window (with a five-minute margin), and that the hypothesis cites at least one piece of evidence.
- Counts each rejection reason, so the report shows why hypotheses were dropped.

Only verified hypotheses reach the **reconciler**, a stronger model (Qwen/Qwen3.8-Max) which may ask one specialist one follow-up question:

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

**What this does:**
- Gives the reconciler only the verified hypotheses from every specialist, plus the allowed component and reason lists.
- Asks it to prefer a cause that explains the other findings, and to answer in JSON with an optional single question back to one specialist.

One investigation runs all three specialists in parallel, verifies their claims and reconciles them:

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

**What this does:**
- Runs the three specialists side by side in a thread pool, and verifies each one's hypotheses against its own tool record.
- Lets the reconciler ask one specialist one follow-up question, and never loops.
- Counts the model calls, which is 13 in the test.

To test the verifier, the author added a fake hypothesis that cites `s999999`, an id no tool ever produced:

```text
#### OUTPUT ####
  verifier     planted 1 rejected ('cites evidence it was not given'), 9 real hypotheses pass
  answer       cartservice-0, container read I/O load, 2022-03-21 15:46:00, confidence 0.7
  asked back   None, 13 model calls, 33.2 s one after another, 13.1 s side by side
  score        partial 0.33 against the answer key
```

**What this does:**
- Shows the planted fake citation rejected as "cites evidence it was not given", while nine real hypotheses pass.
- Gives the final answer, cartservice-0 with container read I/O load at 15:46 on 21 March 2022 and confidence 0.7, with partial credit of 0.33.
- Shows that the reconciler did not ask a follow-up, and that the sequential run took 33.2 seconds against 13.1 seconds in parallel.

The fake citation is caught, and the answer gets the pod and the minute right but the reason wrong. In the full run, the answer becomes a fix card that waits for a person.

## Label Adjudication
Wikimedia's 146 incident reports have no answer key, so models did the labelling. Two models from different families had to agree. Claude Opus 5.5 and Qwen/Qwen3.7-Max read every report, and Cohen's kappa measures their agreement beyond what guessing would give.

```text
#### OUTPUT ####
  trigger        kappa 0.73
  detected by    kappa 0.92
  agreed on trigger and components: 100 of 146
```

**What this does:**
- Reports kappa of 0.73 for the trigger and 0.92 for detection, which the author reads as agreement between the two models.
- Shows that both models agreed on the trigger and components for 100 of 146 reports.

> 🔎 Added context: Cohen's kappa adjusts raw agreement for the agreement two raters would reach by chance, so 1.0 is perfect and 0 is chance level.

For the other 46 reports, a Claude model and Qwen/Qwen3.8-Max judged blind and agreed on 27. The other 19 left the test. Before any test data was opened, the splits, floors and targets were fingerprinted with sha256, so the design could not be changed after seeing results.

## Graph Versus Raw Context
One SPARQL query matched five lookups across five sources on all 217 alert moments with a change. Then four arms read the 24 test incidents, as they were known 15 minutes in:

```text
#### OUTPUT ####
  arm                     trigger right   cites unseen   credits
  graph, one prompt                 42%             0%      0.20
  raw, one prompt                   25%             4%      0.20
  graph, specialists                33%             0%      3.38
  raw, specialists                  33%             0%      3.36
```

**What this does:**
- Compares graph facts and raw text, each in one prompt and in the specialist setup.
- Shows graph facts identifying the trigger at 42% in one prompt against 25% for raw text, and only raw text citing unseen evidence (4%).
- Shows the specialist arms costing about 3.4 credits, against 0.20 for one prompt.

Graph facts beat raw text in one prompt, 42% against 25%, and only raw text cited unseen evidence. With 24 incidents, though, no gap here exceeds four incidents, so the result is suggestive rather than conclusive.

## Benchmark Comparison
RCAEval tells each method when the fault was injected. BARO, its strongest published baseline on Train Ticket, scores each service by how much its metrics shift after that moment. The author's model-free ranker does the same in a simple way:

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

**What this does:**
- Uses the first column as the clock and the time of injection as the split between normal and faulty periods.
- Scores each service by its most extreme metric after the fault, measured in units of the normal spread (IQR, or standard deviation as fallback).
- Skips any metric with fewer than ten normal readings, because there is too little history to know normal.

On all 360 RCAEval cases, with no model and no training, the injected service is among the first three guesses 91% of the time. On Train Ticket that is 80%, against [BARO's 82%](https://arxiv.org/abs/2412.17015). The first guess is right 31% of the time, against BARO's 69%.

OpenRCA hides the injection time. Frozen and run once on the cluster held back for testing, the author's agents earned 16.9% of the partial credit and 5.1% under strict scoring. A [follow-up study](https://arxiv.org/abs/2607.13548) ran its own arms on this cluster, and all of them beat the author's agents, because the author's detectors hide the cause before any agent looks.

## Claim Verdicts
An exact test decides each of the twelve claims. **McNemar's test** counts only the cases where two methods disagree, and seven comparisons share **Holm's correction**.

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

**What this does:**
- `mcnemar` counts the cases where only one method is right and uses an exact binomial test on those disagreements.
- `holm` adjusts the p-values for multiple comparisons, so the family of tests keeps its overall error rate.

> 🔎 Added context: McNemar's test compares two methods on the same cases using only the discordant pairs. Holm's correction is a step-down adjustment for several tests at once, which makes each individual p-value stricter.

A claim needs every comparison pointing its way with a corrected p-value below 0.05:

```text
#### OUTPUT ####
  comparison                          n   what we saw                          p   Holm p
  closed vocabulary repeats itself   10   only closed 9, only free 0       0.004    0.027
  topology over timing               41   first: topology 4, timing 2      0.500    1.000
  incidents over placebo            853   33/146 incidents, 103/707       0.018    0.111
  learned over manual, held out      78   better on 7, worse on 8          1.000    1.000
  three more with chat, raw text and coverage, each 1 incident against 0, Holm p 1.000
```

**What this does:**
- Lists the comparisons with the counts, raw p-values and Holm-adjusted p-values.
- Shows that only the closed vocabulary claim survives correction, with a Holm p of 0.027, while the others sit at 0.111 or above.

After the correction, the closed vocabulary alone survives. Five claims hold, including one query that replaces three tools. Two fail, the edge cap and the flywheel, and five stay open or cannot be tested.

## Stream Replay and Fleet Sizing
Netflix's logging stream tops 38 million events per second. To measure one machine, the author streams all 525 million OpenRCA events, shuffled into small batches, through minute-level aggregates.

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

**What this does:**
- Defines the columns each kind of data needs and one aggregate query per kind, so each batch is reduced to per-minute totals.
- `micro_batches` lists every row group of every file and shuffles them with a fixed seed, so the stream is reproducible and resembles live traffic.

Each worker loops over its share of the batches until the clock runs out:

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

**What this does:**
- Each process reads one row group at a time, which is one micro-batch, and runs the aggregate on it.
- Counts rows as it goes, and stops when the time limit is reached.

The rate is measured from the first start to the last finished batch:

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

**What this does:**
- Splits the batches across processes and gives each an equal share of the cores.
- Divides the events by the wall-clock time from the first start to the last finish, so the rate is honest.

The author ran 1 to 16 workers for 45 seconds each, then 16 for ten minutes:

```python
from kg.replay import micro_batches, replay

batches = micro_batches(openrca_files)        # {kind: Parquet files}, under if __name__ == "__main__" in a script
long = replay(batches, 16, 600)
print(f"  ten minutes: {long['events']:,} events, {long['events_per_s'] / 1e6:.1f}M a second")

#### OUTPUT ####
  ten minutes: 15,723,611,150 events, 26.1M a second
```

**What this does:**
- Runs sixteen workers for ten minutes over the shuffled stream.
- Reports 15,723,611,150 events at 26.1 million a second.

The replay ran at 8.4 million events per second on one worker and 26.4 million on sixteen. Across a ten-minute run, sixteen workers held 26.1 million. Netflix's stream therefore needs about 1.5 such laptops, or 2.0 if events arrive as JSON. The graph sees only signals: one signal condenses 82,681 events, so the stream becomes 2,753 triples a second. The full run loaded 16,393,203 triples into one store. With hour buckets, a typical question stayed under 1.4 ms however large the store grew, and live writes, each batch checked by the shapes before its commit, reached 39,871 triples per second, which is enough for the stream fourteen times over.

> ❌ **Anti-pattern — writing raw events to the graph:** every event becomes a triple. **Why people do it:** it keeps everything. **Consequence:** the store grows with the stream, not with the incidents. **Fix:** condense events into signals first, and store only the signals in the graph.

## My architecture Limits
This study never touched Netflix's own telemetry. The author lists the other limits, with the detectors the weakest. Three pieces stay in the repository and were left out of the write-up: the Wikimedia harvest client and pseudonyms, the pageview converter and the detectors, which the author says to build when a new source is added; the shape compiler and the store's bulk writers, which are needed past a million triples; and the flywheel, alert episodes, change kinds, the plan fingerprint and the fix card, which are needed once people act on answers.

All the code is in the author's [GitHub repository](https://github.com/FareedKhan-dev/ontology-driven-observability).

## 🧠 Mental models
### Shared names before joins
**The model:** Facts join only when every machine has one canonical name, and aliases are the first failure in any incident join.
**How to apply it:** Before building a graph, mint one URI function and test that different spellings of a machine resolve to the same node.

### Closed-world shapes at write time
**The model:** Data enters the store only when it passes rules, so bad facts are refused rather than cleaned up later.
**How to apply it:** Put type, range and vocabulary checks in shapes, validate only the new delta, and turn off inference in the validator.

### Silence needs coverage
**The model:** "Nothing fired" means something only if the source was actually read.
**How to apply it:** Record coverage per run as ok, partial or missing, and answer "unknown" when a source was not covered.

### Closed lists for models, checks in code
**The model:** A model picks from enumerated labels and cites only ids that tools returned, and code verifies both.
**How to apply it:** Read the allowed values from the schema, validate every reply, and reject any citation the tools never handed out.

### Topology over timing
**The model:** A cause is credible when the call graph links it to the symptom, not merely because it happened first.
**How to apply it:** Gate each hop on the call graph, and compare any score with a random-moment baseline before trusting it.

## 🔑 Key takeaways
- Joining incident tools needs one canonical name per machine: the 44% catalogue match today shows how far aliases and retired hosts break joins.
- Shapes checked at write time caught all eight planted violations, and a SPARQL-level text number silently broke a filter in two engines.
- Call-graph topology lifted top-three accuracy on 41 development failures from 4.9% (timing-only 9.8%) to 26.8%.
- A closed vocabulary made the model repeat its labels on 90% of conversations, against no repeats with free JSON.
- The verifier caught a planted citation to an id no tool produced, without asking any model.
- On the held-out OpenRCA cluster the author's agents scored 5.1% strict and 16.9% partial, and the detectors, not the agents, were the bottleneck.
- A single laptop sustained 26.1 million events a second, which the author says covers Netflix's stream with about 1.5 machines.

## 🏛️ For Kenji
Your watsonx.governance background maps well onto the provenance layer: per-run named graphs and as-of queries are an audit trail, and coverage records are the evidence that a control actually looked. In architecture reviews this is a reusable pattern for any system that must explain its past decisions. The question it raises for you: which of your current systems would fail the "nothing fired versus nobody looked" test, and how would you know? This is your interpretation, not the author's.

## 📚 Reading plan
### From the article
- [Netflix tech blog: ontology-driven observability](https://netflixtechblog.com/ontology-driven-observability-building-an-end-to-end-knowledge-graph-at-netflix-scale-421392cf5adf) — the original design that this rebuild follows.
- [QCon London talk (InfoQ)](https://www.infoq.com/presentations/netflix-observability-aiops-ontology-scale/) — Netflix's own presentation of the approach.
- [GitHub repository](https://github.com/FareedKhan-dev/ontology-driven-observability) — the full code for every module in this note.
- [BARO (arXiv 2412.17015)](https://arxiv.org/abs/2412.17015) — the strongest published baseline on Train Ticket.
- [Follow-up study (arXiv 2607.13548)](https://arxiv.org/abs/2607.13548) — the arms the author compares against on the OpenRCA cluster.

### To go further
- OpenRCA and RCAEval — the public datasets with answer keys used for the tests.
- W3C PROV-O and SHACL — the provenance vocabulary and the shape language behind the store and the validator.

## Concepts
[Agent harness](/concepts/ai-ml/agent-harness.md) · [Structured decision model](/concepts/ai-ml/structured-decision-model.md) · [Calibration](/concepts/ai-ml/calibration.md) · [Knowledge graph](/concepts/data/knowledge-graph.md)
