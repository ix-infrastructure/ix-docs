# Quick Start

## 1. Start the backend

```sh
ix docker start
```

This starts ArangoDB and the Memory Layer as Docker containers.

## 2. Map your project

```sh
cd ~/my-project
ix map ./src
```

This parses your source files, builds the knowledge graph, and runs architectural analysis. You'll see a hierarchy of detected systems, subsystems, and modules.

## 3. Explore your code

```sh
# Search for a symbol
ix search UserService

# See what it contains
ix overview UserService

# See what depends on it
ix impact UserService

# Explain what it does
ix explain UserService

# Find who calls a function
ix callers processPayment

# Follow a dependency chain
ix trace DatabaseClient --upstream
```

## 4. Keep the graph updated

```sh
# Auto-ingest on file changes
ix watch

# Or re-map after significant changes
ix map ./src
```

## 5. Use with Claude Code

If you have Claude Code installed, Ix hooks are set up automatically. When you:
- **Search** (Grep/Glob): Ix injects graph-aware results before the search runs
- **Edit** (Write/Edit): Ix auto-ingests the changed file to keep the graph current

No configuration needed — just use Claude Code as normal and Ix enhances it in the background.

## Common Workflows

### "What are the most important classes?"
```sh
ix rank --by dependents --kind class --top 10
```

### "What would break if I change this?"
```sh
ix impact AuthProvider --depth 2
```

### "How does this connect to that?"
```sh
ix trace AuthProvider --to DatabaseClient
```

### "List all functions in a directory"
```sh
ix inventory --kind function --path src/auth/
```

### "What changed between two ingestions?"
```sh
ix diff 10 15 --summary
```
