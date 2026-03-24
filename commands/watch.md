# ix watch

Watch for file changes and auto-ingest to keep the graph current.

## Usage

```sh
ix watch
```

## What It Does

1. Watches the project directory for file changes
2. Debounces changes (300ms) to avoid re-ingesting during saves
3. Checks content hash — skips if file content hasn't actually changed
4. Parses changed files and updates the graph

## Flags

| Flag | Description |
|------|-------------|
| `--path <path>` | Watch a subdirectory instead of the whole project |
| `--root <dir>` | Workspace root directory |

## Ignored Directories

node_modules, .git, dist, build, target, .next, .cache, __pycache__, .ix, .claude

## Examples

```sh
# Watch entire project
ix watch

# Watch a subdirectory
ix watch --path src/
```
