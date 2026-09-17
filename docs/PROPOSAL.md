# SkeinDB

**SkeinDB — weaving data, code, relationships, and inference into one substrate.**

## 1. Goal Statement

SkeinDB is a Rust-native, embeddable, multimodel knowledge database designed for highly concurrent agent swarms.

Its long-term goal is to combine the major capabilities represented by:

- **SurrealDB**
- **Glean**
- **DuckDB**
- **AllegroGraph**

inside one coherent database engine with shared identity, storage, querying, provenance, temporal versioning, code intelligence, analytics, reasoning, and concurrent execution.

SkeinDB should provide:

- transactional application data;
- documents and records;
- property graphs;
- source-code intelligence;
- typed facts and predicates;
- derived facts and recursive rules;
- full-text and fuzzy search;
- vector and semantic retrieval;
- analytical and columnar execution;
- RDF;
- SPARQL;
- ontologies;
- logical inference;
- SHACL-style validation;
- temporal and historical querying;
- bitemporal data;
- first-class provenance;
- database-state history;
- native Git-compatible source-project tracking;
- concurrent multi-agent read and write lanes;
- embedded operation;
- server operation;
- distributed operation.

SkeinDB must be usable directly inside a Rust application without requiring a separate database server.

Agent-swarm concurrency, embeddability, deterministic provenance, temporal versioning, and project-aware source control are foundational architectural properties.

SkeinDB begins as a fork of SurrealDB and progressively evolves into its own database architecture.

The intention is not to run SurrealDB, Glean, DuckDB, AllegroGraph, and Git as independent systems behind one facade.

The intention is to integrate their strongest architectural capabilities into **one Rust-native database engine and knowledge substrate**.

---

## 2. Fundamental Architecture

SkeinDB is built around five major domains:

```text
SkeinDB
│
├── Operational Database
│   ├── records
│   ├── documents
│   ├── transactions
│   ├── graphs
│   ├── indexes
│   └── vectors
│
├── Knowledge Engine
│   ├── entities
│   ├── facts
│   ├── predicates
│   ├── provenance
│   ├── derivation
│   └── reasoning
│
├── Analytical Engine
│   ├── columnar execution
│   ├── Arrow
│   ├── DataFusion
│   └── OLAP
│
├── Semantic Engine
│   ├── RDF
│   ├── SPARQL
│   ├── ontology
│   ├── OWL
│   └── SHACL
│
└── Project Intelligence
    ├── files
    ├── Git-compatible history
    ├── symbols
    ├── code graph
    ├── project revisions
    └── semantic code analysis
```

These domains must share identity and provenance where they refer to the same logical object.

---

## 3. Core Architectural Principles

### 3.1 One Identity

A logical entity has one SkeinDB identity regardless of how many database models refer to it.

For example, a Rust function may simultaneously be:

```text
record
graph node
code symbol
fact subject
RDF resource
analytical entity
vector-search entity
```

These are views of one entity, not independent copies.

### 3.2 One Knowledge Substrate

Records, relationships, facts, code knowledge, semantic knowledge, and analytical representations must be able to reference the same entities.

Physical representations may differ internally.

Identity, provenance, temporal state, and revision context must remain coherent.

### 3.3 Many Concurrent Actors

SkeinDB assumes that many autonomous actors may interact with the database simultaneously.

These may include:

```text
humans
coding agents
research agents
planning agents
language servers
indexers
test runners
build systems
reasoning workers
analytical workers
models
services
UI processes
```

The database must therefore be designed around concurrent execution contexts rather than a single mutable connection.

### 3.4 Explicit Knowledge Type

SkeinDB distinguishes between:

```text
asserted
structural
inferred
analytical
probabilistic
```

knowledge.

Approximate or probabilistic information must never silently become equivalent to deterministic fact.

### 3.5 Explainability

Important data should be capable of answering:

```text
What is this?
Where did it come from?
Who produced it?
When did SkeinDB learn it?
When was it valid?
Which project revision did it describe?
Was it asserted, extracted, inferred, or estimated?
What other facts produced it?
```

### 3.6 Database History and Project History Are Different Systems

SkeinDB contains both:

```text
DATABASE VERSIONING
```

and:

```text
PROJECT VERSION CONTROL
```

These are related but must never be conflated.

Database versioning describes the history of knowledge stored in SkeinDB.

Project version control describes the source-controlled history of a software or knowledge project.

---

## 4. Core Terminology

The following definitions are normative.

### 4.1 Entity

An **Entity** is a uniquely identifiable logical object.

Examples:

```text
person
file
repository
function
agent
task
concept
RDF resource
```

Every persistent entity receives an `EntityId`.

An Entity is not equivalent to a Record.

A Record is one representation of an Entity.

### 4.2 Value

A **Value** is data associated with a Record or Fact.

Examples:

```text
42
"authenticate_user"
true
timestamp
list
map
binary value
entity reference
```

### 4.3 Predicate

A **Predicate** defines the meaning of a Fact.

Examples:

```text
CALLS
IMPLEMENTS
HAS_NAME
INSTANCE_OF
MODIFIED_BY
```

A Predicate may define:

- domain;
- range;
- cardinality;
- constraints;
- indexing;
- derivation behavior.

### 4.4 Fact

A **Fact** is a versioned statement about one or more Entities.

Example:

```text
authenticate_user CALLS verify_password
```

Conceptually:

```text
Fact
├── FactId
├── Subject
├── Predicate
├── Object / Value
├── KnowledgeClass
├── Provenance
├── TransactionTime
├── ValidTime
├── ProjectRevision
└── Derivation
```

### 4.5 Record

A **Record** is a mutable structured database representation associated with an Entity.

Example:

```yaml
entity: user:42
name: Ilja
status: active
```

Records provide conventional database ergonomics.

Their historical values may be preserved through database versioning.

### 4.6 Relationship

A **Relationship** is a first-class graph connection between Entities.

Example:

```text
function:A -CALLS-> function:B
```

Relationships may contain fields.

Example:

```yaml
relationship: CALLS
from: function:A
to: function:B

properties:
  file: src/auth.rs
  line: 82
```

---

## 5. Knowledge Classes

### Asserted

Written directly or imported from an authoritative source.

Example:

```text
Project X uses Rust.
```

### Structural

Deterministically extracted.

Example:

```text
function A CALLS function B
```

### Inferred

Produced through logical rules or ontology reasoning.

Example:

```text
A TRANSITIVELY_DEPENDS_ON D
```

### Analytical

Produced through deterministic analysis or aggregation.

Example:

```text
Module X has a 12.7% test-failure rate.
```

### Probabilistic

Produced by approximate methods.

Examples:

```text
vector similarity
classifier output
language-model judgment
probabilistic relationship
```

Knowledge Class must remain machine-readable.

---

## 6. Actor

An **Actor** is anything capable of causing observable database activity.

Examples:

```text
human
agent
model
tool
indexer
service
database worker
```

Every Actor has an `ActorId`.

Authentication identity and Actor identity are separate.

One authenticated user may operate many Actors.

---

## 7. Lane

A **Lane** is an independently executing database workload context.

A Lane is:

- not a transaction;
- not a project branch;
- not a database version.

A Lane contains or references:

```text
LaneId
ActorId
ProjectContext
ProjectRevision
DatabaseSnapshot
TransactionContext
PermissionContext
Priority
ResourceBudget
ProvenanceContext
```

Example:

```text
Lane:
  actor: refactor-agent-17
  project: handshake
  project_branch: feature/auth
```

Multiple Lanes may operate against the same Project Branch.

One Actor may own several Lanes.

---

## 8. Transaction

A **Transaction** is a short-lived atomic database operation.

A Transaction:

- reads from a stable database snapshot;
- performs reads and/or writes;
- either commits completely or does not commit;
- participates in concurrency control;
- belongs to a Lane.

Transactions handle short-term database concurrency.

Typical duration:

```text
milliseconds → seconds
```

---

## 9. Database Snapshot

A **Database Snapshot** is an immutable logical view of SkeinDB at a defined transactional state.

Snapshots support:

```text
consistent reads
MVCC
historical inspection
analytical isolation
concurrent readers
```

A Database Snapshot is not a Git commit or Project Revision.

---

## 10. Database Versioning

SkeinDB should preserve historical database state as a first-class capability.

Database versioning exists to answer questions such as:

```text
What is the current value?

What was the value yesterday?

What value existed before transaction X?

Which actor changed this field?

When did this fact become invalid?

What did Agent A know when it made decision Y?
```

Database versioning applies to appropriate database objects including:

```text
records
record fields
facts
relationships
ontology assertions
schemas
derived knowledge
```

The database may internally use MVCC versions, temporal records, append-only history, or another efficient representation.

The user-visible semantics must remain explicit.

---

## 11. Transaction Time

**Transaction Time** describes when SkeinDB stored a version.

Example:

```text
created:
2026-09-17T08:10

superseded:
2026-09-17T09:42
```

Transaction Time answers:

> When did the database know this?

---

## 12. Valid Time

**Valid Time** describes when the represented statement is considered true in the modeled world.

Example:

```text
valid_from:
2026-01-01

valid_until:
2026-08-31
```

Valid Time answers:

> When was this fact actually true?

---

## 13. Bitemporal Data

A database object supporting both:

```text
Transaction Time
+
Valid Time
```

is **Bitemporal**.

Example:

```text
Employee status valid:
January 1 → August 31

SkeinDB learned:
February 17

SkeinDB received correction:
September 4
```

SkeinDB should support bitemporal data as part of its mature temporal model.

---

## 14. Historical Query

A **Historical Query** explicitly requests database state from another point in Transaction Time, Valid Time, or both.

Conceptually:

```text
SELECT ...
AT TRANSACTION TIME ...
```

or:

```text
SELECT ...
VALID AT ...
```

Exact syntax remains part of future query-language design.

---

## 15. Database History Is Not Git

Database versioning does not imply:

```text
database branches
database pull requests
database cherry-picks
Git-style merge of arbitrary database state
```

Database history serves:

```text
temporal knowledge
auditing
provenance
agent reasoning history
recovery
historical querying
```

Project source control handles Git-style branching and merging.

---

## 16. Project

A **Project** is a first-class SkeinDB domain representing a version-controlled body of work.

A Project may contain:

```text
source files
documentation
configuration
assets
tests
build definitions
repository history
code intelligence
derived code facts
project ontology
project analytics
agent work
```

---

## 17. Project Revision

A **Project Revision** identifies one exact version of Project source state.

For Git-compatible projects it normally maps to a Git commit.

Example:

```text
Project:
handshake

Revision:
a81c239...
```

Every Project-derived representation must identify which Project Revision it describes.

---

## 18. Project Commit

A **Project Commit** is a source-control commit.

It contains:

```text
ProjectCommitId
ParentCommitIds
FileTreeRoot
Author
Timestamp
Message
Metadata
```

Project Commits form a DAG.

```text
C1 ── C2 ── C3
          \
           C4
```

A Project Commit is unrelated to an ACID Transaction Commit except where an operation happens to interact with both systems.

---

## 19. Project Branch

A **Project Branch** is a named mutable reference to a Project Commit.

Example:

```text
main -> C182

feature/auth -> C196
```

A Project Branch versions project files.

It does not branch the entire SkeinDB database.

---

## 20. Project Tag

A **Project Tag** is a named immutable reference to a Project Commit.

Examples:

```text
release/1.0
baseline-before-refactor
prototype-A
```

---

## 21. Project Workspace

A **Project Workspace** is a mutable working representation of a Project Branch or explicit Project Revision.

Example:

```text
Project: Handshake

Workspace A
  branch: feature/auth

Workspace B
  branch: main

Workspace C
  branch: analytics
```

Multiple workspaces may share repository storage.

---

## 22. Project Diff

A **Project Diff** compares two Project Revisions.

SkeinDB should support multiple levels.

### Textual Diff

Conventional file/line differences.

### Structural Diff

Examples:

```text
function added
function removed
signature changed
trait implementation changed
dependency changed
type changed
```

### Semantic Diff

Examples:

```text
public API changed
call graph changed
test coverage changed
dependency reachability changed
ontology classification changed
```

### Knowledge Diff

Compares Project-derived knowledge associated with two revisions.

---

## 23. Project Merge

A **Project Merge** combines divergent Project histories based on a common Project Commit ancestor.

Source-control semantics govern the file tree merge.

SkeinDB may augment the merge process with:

```text
structural code analysis
dependency analysis
semantic validation
ontology validation
test selection
blast-radius analysis
```

---

## 24. Project Conflict

A **Project Conflict** is a conflict encountered while combining Project histories.

Possible categories include:

```text
text conflict
file-tree conflict
symbol conflict
schema conflict
semantic inconsistency
ontology inconsistency
```

Conflicts should be represented structurally and be machine-readable.

---

## 25. Git Compatibility

SkeinDB should natively understand and interoperate with Git-compatible Project history.

The Rust implementation should investigate:

```text
gitoxide / gix
```

for Git plumbing.

Target operations include:

```text
clone
fetch
push
commit
branch
tag
merge
diff
history
remote synchronization
```

Interoperability targets include:

```text
Forgejo
GitHub
GitLab
ordinary Git servers
ordinary Git clients
existing CI/CD
```

---

## 26. Skein Project Model

Git-compatible history is one component of the richer Skein Project model.

```text
Skein Project
│
├── File Tree
├── Git-Compatible History
├── Branches
├── Tags
├── Workspaces
│
├── Code Intelligence
│   ├── symbols
│   ├── definitions
│   ├── references
│   ├── calls
│   └── types
│
├── Knowledge
│   ├── facts
│   ├── relationships
│   └── ontology
│
└── Analytics
```

Git provides source-control interoperability.

SkeinDB provides project understanding.

---

## 27. File Entity

Every tracked Project file may also be a SkeinDB Entity.

Example:

```text
entity:file:src/auth.rs
```

Associated information may include:

```text
path
content identity
project
revision
language
symbols
history
provenance
```

---

## 28. Code Symbol

A **Code Symbol** is a code Entity representing a semantically identifiable program construct.

Examples:

```text
function
method
type
struct
enum
trait
module
constant
variable
```

SkeinDB should maintain stable logical identity across Project Revisions where deterministic continuity can be established.

When continuity cannot be established reliably, SkeinDB must represent uncertainty rather than fabricate identity.

---

## 29. Project Revision Binding

Every Project-derived Fact must identify its Project Revision.

Example:

```yaml
fact:
  subject: function:login
  predicate: CALLS
  object: function:authenticate

project:
  id: handshake
  revision: a81c239

extractor:
  rust-analyzer
```

This requirement applies to:

```text
code symbols
references
call graphs
dependency graphs
AST facts
test associations
code-derived embeddings
code ontology classifications
```

---

## 30. Project Knowledge Freshness

Every Project-derived representation must expose a freshness state.

At minimum:

```text
CURRENT
STALE
REBUILDING
UNAVAILABLE
```

Example:

```text
Project revision:
abc123

Symbols:
CURRENT @ abc123

Call graph:
CURRENT @ abc123

Embeddings:
STALE @ abc120

Ontology classification:
REBUILDING
```

A stale representation must never silently appear current.

---

## 31. Code Intelligence

SkeinDB targets Glean-class source intelligence.

Native code entities should eventually include:

```text
repository
project
package
module
file
symbol
function
method
type
struct
enum
trait
implementation
parameter
variable
expression
call
reference
import
test
diagnostic
project revision
```

Relationships include:

```text
DEFINES
REFERENCES
CALLS
IMPORTS
IMPLEMENTS
CONTAINS
RETURNS
USES_TYPE
OVERRIDES
TESTS
DEPENDS_ON
GENERATED_FROM
MODIFIED_BY
```

---

## 32. Code Ingestion

For Rust, the initial semantic pipeline should reuse:

```text
rust-analyzer
SCIP
Tree-sitter
Git / gix
```

SkeinDB should consume deterministic language intelligence rather than recreating compiler semantics unnecessarily.

---

## 33. Code Queries

Revision-aware deterministic queries should include:

```text
symbol()
definition()
references()
callers()
callees()
implementations()
dependencies()
changed_between()
blast_radius()
affected_tests()
```

Project Revision must either be explicitly supplied or deterministically resolved from the Lane's Project context.

---

## 34. Provenance

**Provenance** describes how information entered SkeinDB.

It may reference:

```text
ActorId
LaneId
Transaction
ProjectId
ProjectRevision
Source
Extractor
Rule
Model
Timestamp
ParentFacts
```

Example:

```yaml
fact:
  subject: function:login
  predicate: CALLS
  object: function:authenticate

knowledge_class:
  structural

provenance:
  actor: indexer:rust
  extractor: rust-analyzer
  source: src/auth.rs
  project_revision: abc123
```

---

## 35. Derivation

A **Derivation** describes how knowledge was generated from existing Facts.

Example:

```text
A CALLS B
B CALLS C

therefore:

A TRANSITIVELY_DEPENDS_ON C
```

The result should retain:

```text
rule
rule version
input facts
database transaction time
project revision where applicable
```

---

## 36. Derived State

**Derived State** is reproducibly generated information.

Examples:

```text
embeddings
secondary indexes
full-text indexes
materialized inferred facts
code indexes
analytics caches
statistics
```

Derived State should have a validity state:

```text
CURRENT
STALE
INVALID
REBUILDING
```

Source changes must invalidate dependent derived state deterministically.

---

## 37. SurrealDB Capability Domain

SkeinDB begins from SurrealDB and targets continued or improved support for:

```text
records
documents
property graphs
ACID transactions
MVCC
schemas
indexes
full-text search
vector search
permissions
authentication
embedded operation
server operation
distributed operation
live queries
change feeds
```

---

## 38. Glean Capability Domain

SkeinDB targets:

```text
typed facts
predicates
schemas
code facts
derived predicates
recursive rules
incremental indexing
code symbol identity
definitions
references
call graphs
type relationships
repository history
code navigation
dependency analysis
blast-radius analysis
provenance
historical code queries
```

---

## 39. DuckDB Capability Domain

SkeinDB targets DuckDB-class analytical capabilities through a Rust-native implementation.

Likely foundation:

```text
DataFusion
Apache Arrow
Parquet
```

Target capabilities include:

```text
analytical SQL
vectorized execution
columnar processing
parallel scans
aggregations
joins
window functions
sorting
statistics
cost-based optimization
predicate pushdown
projection pushdown
Arrow interoperability
Parquet interoperability
```

---

## 40. AllegroGraph Capability Domain

SkeinDB targets:

```text
RDF
triples
quads
named graphs
RDF-star
SPARQL
SPARQL Update
SPARQL federation
RDFS
OWL
SHACL
logical inference
ontology management
materialized reasoning
query-time reasoning
semantic validation
```

Semantic entities must share SkeinDB Entity identity.

---

## 41. Reasoning

SkeinDB should support rule-derived and ontology-derived knowledge.

Long-term reasoning targets include:

```text
recursive predicates
Datalog
fixpoint evaluation
RDFS
OWL RL
OWL EL
OWL DL
OWL 2 DL / SROIQ(D)
```

Reasoning results must retain provenance.

---

## 42. Unified Query Architecture

Potential query frontends include:

```text
SurrealQL
SQL
SPARQL
Datalog / rule queries
code-intelligence API
structured agent API
```

Where practical, these should compile into a shared logical representation.

```text
                  Query Frontends
                         │
          ┌──────────────┼───────────────┐
          ▼              ▼               ▼
      SurrealQL         SQL            SPARQL
          │              │               │
          └──────────────┼───────────────┘
                         ▼
                  Logical Query IR
                         │
                       Planner
                         │
                      Optimizer
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
 Operational         Analytical         Reasoning
 Executor            Executor           Executor
```

---

## 43. Hybrid Query

A **Hybrid Query** requires several SkeinDB capability domains.

Example:

> Find functions changed on `feature/auth` that are semantically related to authentication, classified by the ontology as security-sensitive, transitively reachable from public APIs, insufficiently tested, and associated with increased runtime failure rates.

This may require:

```text
Project history
+
code graph
+
vector search
+
ontology
+
derived facts
+
analytics
```

Hybrid querying is a defining long-term capability.

---

## 44. Agent-Swarm Concurrency

SkeinDB assumes simultaneous heterogeneous workloads.

Example:

```text
Lane A: READ       ───────────────────▶
Lane B: WRITE      ───────▶
Lane C: CODE INDEX ───────────────────▶
Lane D: OLAP       ───────────────────────▶
Lane E: GRAPH      ─────────────▶
Lane F: WRITE          ───────▶
Lane G: REASONING  ──────────────────▶
```

The concurrency model must support:

```text
multiple readers
multiple writers
stable snapshots
MVCC
fine-grained conflict detection
query cancellation
priority
backpressure
resource isolation
```

---

## 45. Lane vs Project Branch

This distinction is mandatory.

```text
LANE
= active database execution context

PROJECT BRANCH
= version-control lineage of project source
```

Example:

```text
Project Branch:
feature/auth

Lanes:
coding-agent
testing-agent
review-agent
indexer
```

All four may interact with the same Project Branch simultaneously.

---

## 46. Workload Scheduler

SkeinDB should eventually include a workload scheduler coordinating:

```text
transactions
interactive queries
graph traversals
vector retrieval
code indexing
OLAP
reasoning
project-history analysis
background derivation
maintenance
```

Scheduler controls may include:

```text
priority
fairness
memory budgets
CPU budgets
deadlines
cancellation
lane isolation
backpressure
admission control
```

---

## 47. Backpressure

**Backpressure** prevents producers from generating work faster than SkeinDB can safely consume it.

Mechanisms may include:

```text
bounded queues
write admission
batching
rate signalling
resource quotas
priority queues
```

System overload must be represented explicitly.

---

## 48. Embeddability

SkeinDB must support direct Rust embedding.

Conceptually:

```rust
let db = Skein::open("./knowledge").await?;
```

Embedded mode must support:

```text
persistent storage
multiple concurrent lanes
transactions
project tracking
code indexing
queries
analytics
reasoning
```

No separate server is required.

---

## 49. Deployment Modes

SkeinDB should support:

```text
embedded single-process
embedded multi-threaded
local server
remote server
distributed cluster
```

The same logical database semantics should apply across modes.

---

## 50. Rust-Native Requirement

The SkeinDB core must remain Rust-native.

Preferred technologies include:

```text
SurrealDB
SurrealKV
DataFusion
Apache Arrow
Oxigraph
gitoxide / gix
rust-analyzer
SCIP
Tree-sitter
Rust Datalog engines
Rust OWL tooling
Rust async infrastructure
```

Optional external integrations may use other languages at system boundaries.

Core database functionality must not require:

```text
JVM
Python runtime
Go runtime
C++ database engine
```

---

## 51. MVP Goal

The SkeinDB MVP must prove:

> **A single embedded Rust database can safely serve concurrent agent lanes while combining transactional data, historical database versions, graph relationships, deterministic code intelligence, Git-compatible Project tracking, facts, provenance, vector retrieval, and analytical execution through one coherent identity system.**

The MVP must be suitable for real use inside Handshake.

---

## 52. MVP Minimum Requirements

### 52.1 Database Foundation

The MVP must provide:

- independent SkeinDB fork;
- persistent embedded operation;
- ACID transactions;
- MVCC;
- records;
- documents;
- graph relationships;
- full-text search;
- vector search;
- multiple readers;
- multiple writers.

### 52.2 Agent Lanes

The MVP must implement:

```text
Actor
Lane
DatabaseSnapshot
TransactionContext
ProjectContext
ProjectRevisionContext
```

Multiple Lanes must be able to operate concurrently.

### 52.3 Database Versioning

The MVP must preserve historical versions for at least:

```text
facts
record fields
relationships
```

It must support:

```text
current value
previous value
history
actor attribution
transaction timestamp
historical lookup
```

### 52.4 Temporal Facts

Facts must support at minimum:

```text
TransactionTime
```

The data model must be compatible with later addition of:

```text
ValidTime
bitemporal querying
```

### 52.5 Shared Entity Identity

One Entity must be able to participate in:

```text
record
graph
fact
code
vector
provenance
```

using one stable logical identity.

### 52.6 Fact Substrate

The MVP must implement:

```text
Entity
Fact
Predicate
KnowledgeClass
Provenance
Derivation
Actor
Lane
```

as durable concepts.

### 52.7 Project Tracking

The MVP must implement:

```text
Project
ProjectRevision
ProjectCommit
ProjectBranch
ProjectWorkspace
ProjectDiff
```

with explicit separation between Project history and database history.

### 52.8 Git Compatibility

The MVP must support a Rust-native Git integration path providing at least:

```text
repository discovery
history reading
branches
commits
diffs
fetch
push
revision mapping
```

Full implementation may use `gix`/gitoxide.

### 52.9 Project Revision Binding

Every code-derived Fact must identify its exact Project Revision.

No code-derived knowledge may be considered current without revision evidence.

### 52.10 Rust Code Intelligence

The MVP must ingest Rust using mature semantic tooling.

At minimum:

```text
files
modules
symbols
definitions
references
calls
imports
implementations
source locations
Project Revision
```

### 52.11 Code Queries

Minimum queries:

```text
symbol()
definition()
references()
callers()
callees()
implementations()
dependencies()
changed_between()
```

### 52.12 Derived Fact

At least one recursive relationship must be implemented.

Example:

```text
A CALLS B
B CALLS C

→

A TRANSITIVELY_DEPENDS_ON C
```

The result must retain derivation provenance.

### 52.13 Vector Retrieval

The MVP must support vector indexing and semantic retrieval.

### 52.14 Hybrid Structural + Semantic Query

At least one query must combine:

```text
semantic/vector retrieval
+
deterministic code structure
```

Example:

```text
find functions semantically related to authentication

then restrict to:

functions reachable from public API endpoints
```

### 52.15 Analytical Execution

The MVP must integrate a Rust-native analytical path supporting:

```text
scan
filter
join
group
aggregate
```

over SkeinDB-managed data.

### 52.16 Provenance

Imported, structural, and derived Facts must identify:

```text
source
actor
database transaction
Project Revision where applicable
extractor/rule
parent facts where applicable
```

### 52.17 Knowledge Freshness

Project-derived knowledge must expose:

```text
CURRENT
STALE
REBUILDING
UNAVAILABLE
```

along with the Project Revision it describes.

### 52.18 Swarm Benchmark

The MVP must contain a repeatable benchmark exercising simultaneous:

```text
reads
writes
historical reads
code indexing
graph traversal
vector search
analytical queries
Project-history queries
```

Metrics should include:

```text
read latency
write latency
tail latency
throughput
conflict rate
memory usage
index lag
knowledge freshness lag
```

---

## 53. Post-MVP Capability Tracks

Development should proceed across explicit capability tracks.

### Operational Database

Progress toward SurrealDB parity and beyond.

### Code Intelligence

Progress toward Glean parity.

### Analytics

Progress toward DuckDB-class capability.

### Semantic Knowledge

Progress toward AllegroGraph-class capability.

### Database Versioning

Progress toward:

```text
full historical records
bitemporal queries
historical graph traversal
historical reasoning
historical analytics
```

### Project VCS

Progress toward:

```text
complete Git compatibility
semantic diff
code-aware merge analysis
workspace management
revision-aware indexing
remote synchronization
```

### Agent Swarms

Progress toward:

```text
high writer concurrency
resource scheduling
distributed lanes
agent workspaces
high-volume ingestion
```

---

## 54. Full Parity Tracking

The full specification should contain explicit parity matrices.

### SurrealDB

Track:

```text
storage
transactions
graphs
documents
schemas
search
vectors
authentication
permissions
live queries
distributed operation
```

### Glean

Track:

```text
facts
predicates
schemas
derived predicates
recursive queries
indexing
cross-language code intelligence
provenance
historical code queries
```

### DuckDB

Track:

```text
SQL
columnar execution
vectorization
optimizer
joins
aggregations
windows
Parquet
Arrow
parallel execution
statistics
extensions
```

### AllegroGraph

Track:

```text
RDF
SPARQL
RDFS
OWL
SHACL
reasoning
named graphs
federation
semantic validation
```

### Git

Track:

```text
commits
branches
tags
diff
merge
history
clone
fetch
push
remotes
workspaces
Git protocol compatibility
```

---

## 55. Long-Term Example

A mature SkeinDB should be able to answer:

> On `feature/auth`, identify functions modified since `main` that are semantically related to authentication, classified by the ontology as security-sensitive, transitively reachable from public APIs, and insufficiently tested. Compare their failure rate over the previous thirty days, show their source-code diff, identify the exact Project Revision every piece of evidence describes, show historical changes to relevant database facts, and explain the provenance of every deterministic and inferred conclusion.

This query combines:

```text
Project/Git history
+
database history
+
code intelligence
+
graph traversal
+
vector retrieval
+
ontology
+
rules
+
analytics
+
provenance
+
temporal querying
```

within one database.

---

## 56. Long-Term Vision

SkeinDB should become a Rust-native knowledge database for autonomous software, research, and machine-operated systems.

It should understand:

```text
what exists
how things relate
what relationships mean
what code does
how project source changed
which revision knowledge describes
how database knowledge changed
when something became known
when something was valid
who or what produced it
what can be logically inferred
what analytics reveal
what models estimate
```

while allowing many autonomous actors to work simultaneously.

The defining product statement remains:

> **SkeinDB — weaving data, code, relationships, and inference into one substrate.**

The concurrency principle is:

> **One substrate, many concurrent lanes.**

The temporal principle is:

> **Knowledge has history. Current state is only one view.**

The project principle is:

> **The project and the machine's understanding of the project share an explicit revision context.**
