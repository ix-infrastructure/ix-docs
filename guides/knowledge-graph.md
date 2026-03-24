# Understanding the Knowledge Graph

Ix builds a versioned graph of your codebase. This guide explains what's in the graph and how to think about it.

## Entities (Nodes)

Every symbol in your code becomes a node in the graph:

| Kind | What it represents |
|------|-------------------|
| `file` | A source file |
| `module` | A module or namespace |
| `class` | A class definition |
| `interface` | An interface definition |
| `trait` | A trait (Scala, Rust) |
| `function` | A standalone function |
| `method` | A method on a class/trait |
| `variable` | A variable or constant |
| `object` | A singleton object (Scala) |

Each node has:
- **name**: the symbol name
- **kind**: entity type
- **line_start / line_end**: source position
- **language**: detected language
- **sourceUri**: file path

## Relationships (Edges)

Edges connect entities:

| Edge | Meaning | Example |
|------|---------|---------|
| `CONTAINS` | Parent has child | file → function, class → method |
| `CALLS` | Invokes | functionA → functionB |
| `IMPORTS` | Imports/requires | file → imported entity |
| `REFERENCES` | Type reference | function signature → type |
| `EXTENDS` | Inheritance | child class → parent class |
| `IN_REGION` | Architectural membership | file → detected region |

## Versioning

Every change to the graph is tracked:

- **Patches**: each ingestion creates a patch with a unique ID
- **Revisions**: monotonically increasing version numbers
- **Provenance**: every entity tracks when it was created, updated, and by what patch
- **Source hashes**: SHA256 of file content at time of ingestion

This means you can:
- See the history of an entity: `ix history UserService`
- Diff between revisions: `ix diff 10 15`
- Detect stale files: `ix status`

## Architectural Regions

When you run `ix map`, Ix runs Louvain community detection on the file coupling graph. This groups files into:

- **Systems**: top-level groupings (e.g., "auth", "api", "ingestion")
- **Subsystems**: mid-level groupings within systems
- **Modules**: fine-grained groupings

Each region has a confidence score (0-1) indicating how cohesive the grouping is.

## Querying the Graph

Think of commands in layers:

**Find things**: `search`, `locate`, `inventory`
**Understand things**: `explain`, `overview`, `read`
**Analyze connections**: `impact`, `trace`, `rank`, `callers`, `depends`
**Track changes**: `status`, `history`, `diff`
