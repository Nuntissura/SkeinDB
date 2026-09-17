# SkeinDB

**SkeinDB — weaving data, code, relationships, and inference into one substrate.**

SkeinDB is a Rust-native, embeddable, multimodel knowledge database designed for highly concurrent agent swarms.

Its long-term goal is to combine the major capability domains represented by **SurrealDB**, **Glean**, **DuckDB**, and **AllegroGraph** inside one coherent engine with shared identity, storage, provenance, temporal versioning, code intelligence, analytics, semantic reasoning, and concurrent execution.

## Core goals

SkeinDB is intended to provide:

- transactional records and documents;
- property graphs;
- full-text, fuzzy, vector, and semantic retrieval;
- typed facts and predicates;
- deterministic source-code intelligence;
- derived facts and recursive rules;
- columnar and analytical execution;
- RDF, SPARQL, ontology, OWL, and SHACL capabilities;
- database history and bitemporal knowledge;
- first-class provenance;
- native Git-compatible project tracking;
- multiple concurrent agent read/write lanes;
- embedded, server, and eventually distributed deployment.

## Design principles

### One identity

A logical entity should have one SkeinDB identity even when it simultaneously appears as a record, graph node, code symbol, RDF resource, analytical entity, or vector-search target.

### One substrate, many lanes

SkeinDB is designed for concurrent machine workloads. Multiple agents, indexers, models, tools, analytical jobs, reasoning workers, and humans should be able to interact with the same database through independent execution lanes.

### Knowledge has history

Current state is only one view. SkeinDB distinguishes transaction time, valid time, provenance, project revision, and knowledge freshness.

### Project state and project understanding stay revision-bound

Code intelligence, call graphs, embeddings, derived facts, and semantic classifications must identify the exact project revision they describe. Stale derived knowledge must never silently appear current.

## Architectural domains

```text
SkeinDB
│
├── Operational Database
│   ├── records / documents
│   ├── transactions / MVCC
│   ├── property graphs
│   ├── indexes / full-text
│   └── vector search
│
├── Knowledge Engine
│   ├── entities
│   ├── facts / predicates
│   ├── provenance
│   ├── derivation
│   └── reasoning
│
├── Analytical Engine
│   ├── DataFusion
│   ├── Arrow
│   └── OLAP
│
├── Semantic Engine
│   ├── RDF / SPARQL
│   ├── ontology
│   ├── OWL
│   └── SHACL
│
└── Project Intelligence
    ├── Git-compatible history
    ├── files / symbols
    ├── definitions / references
    ├── call and dependency graphs
    └── semantic code analysis
```

## Rust-native direction

The core engine is intended to remain Rust-native. Candidate building blocks include:

- SurrealDB / SurrealKV
- DataFusion
- Apache Arrow / Parquet
- Oxigraph
- gitoxide / `gix`
- rust-analyzer / SCIP
- Tree-sitter
- Rust Datalog and OWL tooling

## Status

**Research and architecture phase.**

The current focus is defining the substrate, concurrency model, temporal/versioning semantics, project-revision binding, and an MVP that can be used inside an agentic development environment.

## Proposal

The current technical proposal is maintained in [`docs/PROPOSAL.md`](docs/PROPOSAL.md).

## MVP direction

The MVP should demonstrate one embedded Rust database that can safely serve concurrent agent lanes while combining:

- ACID/MVCC operational storage;
- historical database versions;
- shared entity identity;
- graph relationships;
- typed facts and provenance;
- deterministic Rust code intelligence;
- Git-compatible project tracking;
- vector retrieval;
- an initial DataFusion/Arrow analytical path;
- revision-aware derived knowledge.

## Project statement

> **SkeinDB — weaving data, code, relationships, and inference into one substrate.**

> **One substrate, many concurrent lanes.**

> **Knowledge has history. Current state is only one view.**

> **The project and the machine's understanding of the project share an explicit revision context.**
