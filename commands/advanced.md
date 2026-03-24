# Advanced Commands

These commands are available but hidden from the main help. Use them for low-level graph queries.

## callers

Show what calls a function or method.

```sh
ix callers validateToken
ix callers processPayment --limit 20 --format json
```

Falls back to text search (ripgrep) if no graph edges are found.

## callees

Show what a function or method calls.

```sh
ix callees processPayment
ix callees handle --kind method --format json
```

## contains

Show members of a container (class, module, file).

```sh
ix contains UserService
ix contains auth/provider.ts --format json
```

## imports

Show what an entity imports.

```sh
ix imports auth/provider.ts
ix imports IngestionService --format json
```

## imported-by

Show what imports an entity.

```sh
ix imported-by AuthProvider
ix imported-by UserService --format json
```

## depends

Show the full dependency tree — who depends on this entity.

```sh
ix depends validateToken --depth 2
ix depends AuthProvider --cap 50 --format json
```

## entity

Fetch full entity details by ID (UUID).

```sh
ix entity abc123-def456 --format json
```

Returns node data, claims, and edges.

## text

Fast text search using ripgrep. Searches file contents directly, not the graph.

```sh
ix text "verify_token" --language python --limit 20
ix text "TODO" --path src/ --format json
```

## history

Show the provenance chain for an entity — all patches that touched it.

```sh
ix history UserService
ix history abc123 --format json
```

## diff

Show changes between two graph revisions.

```sh
ix diff 10 15 --summary
ix diff 1 5 --entity abc123 --format json
```

## conflicts

List detected contradictions in the graph.

```sh
ix conflicts --format json
```

## stats

Show graph statistics — node and edge counts by type.

```sh
ix stats --format json
```

## reset

Wipe all graph data for a clean re-ingest.

```sh
ix reset -y           # Skip confirmation
ix reset -y --ingest  # Wipe and re-ingest
```

## config

Show or update configuration.

```sh
ix config show
ix config get endpoint
ix config set endpoint http://localhost:8090
```
