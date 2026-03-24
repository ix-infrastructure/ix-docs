# Architecture

```
ix CLI (TypeScript)  →  Memory Layer (Scala/http4s)  →  ArangoDB
     local                   Docker :8090                Docker :8529
```

## Components

### ix CLI

TypeScript Node.js application. Handles:
- File discovery and parsing (Tree-Sitter)
- Graph patch construction
- User-facing commands and output formatting
- Claude Code hook scripts

Installed to `/usr/local/bin/ix` (or `~/.local/bin/ix`).

### Memory Layer

Scala application using Cats Effect and http4s. Handles:
- REST API for graph operations
- AQL query generation and execution
- Louvain community detection (architectural mapping)
- Patch management and versioning
- Conflict detection

Runs as a Docker container on port 8090.

### ArangoDB

Document and graph database. Stores:
- Nodes (entities)
- Edges (relationships)
- Patches (versioned changesets)
- Claims (confidence-scored assertions)

Runs as a Docker container on port 8529.

## Data Flow

### Ingestion
```
Source File → Tree-Sitter Parse → Entity/Edge Extraction → Patch → REST API → ArangoDB
```

### Query
```
ix command → REST API → AQL Query → ArangoDB → JSON → Formatted Output
```

## Storage

- **Graph data**: ArangoDB Docker volume (`arangodb-data`)
- **Config**: `~/.ix/config.yaml`
- **Backend compose**: `~/.ix/backend/docker-compose.yml`
- **CLI binary**: `~/.ix/cli/`
