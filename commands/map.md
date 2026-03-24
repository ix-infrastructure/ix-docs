# ix map

Map the architectural hierarchy of a codebase. This is the primary entry point for Ix — it parses source files, builds the knowledge graph, and runs community detection to identify systems, subsystems, and modules.

## Usage

```sh
ix map [path]
```

If no path is given, maps the current directory.

## What It Does

1. Ensures the backend is running
2. Parses all supported source files in the given path
3. Extracts entities (classes, functions, modules) and relationships (calls, imports, extends)
4. Commits the graph to the database
5. Runs Louvain community detection on file coupling
6. Outputs a multi-level architectural hierarchy

## Flags

| Flag | Description |
|------|-------------|
| `--format text\|json` | Output format (default: text) |
| `--level <n>` | Show specific hierarchy level |
| `--min-confidence <0-1>` | Filter regions below confidence threshold |
| `--full` | Override safety limits for large repos |
| `--verbose` | Show detailed parsing output |

## Examples

```sh
# Map entire project
ix map .

# Map a subdirectory
ix map ./src/auth

# JSON output for programmatic use
ix map ./src --format json

# Verbose output to see what's being parsed
ix map ./src --verbose
```

## Output

```
System: auth-core (confidence: 0.92)
  ├── auth/provider.ts
  ├── auth/middleware.ts
  └── auth/tokens.ts

System: api-layer (confidence: 0.87)
  ├── routes/users.ts
  ├── routes/payments.ts
  └── middleware/cors.ts
```

Each region shows:
- **Name**: derived from the dominant file paths
- **Confidence**: how cohesive the grouping is (0-1)
- **Files**: the source files in that region
