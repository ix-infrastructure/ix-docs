# ix trace

Follow how a symbol connects — upstream dependencies, downstream dependents, or find the path between two symbols.

## Usage

```sh
ix trace <symbol>
```

## Modes

- **Both** (default): shows upstream and downstream
- `--upstream`: only what this symbol depends on
- `--downstream`: only what depends on this symbol
- `--to <target>`: find the path between two symbols

## Flags

| Flag | Description |
|------|-------------|
| `--upstream` | Show only upstream dependencies |
| `--downstream` | Show only downstream dependents |
| `--to <target>` | Find path to another symbol |
| `--kind <type>` | Edge type filter: calls, imports, depends, contains |
| `--depth <n>` | Max traversal depth |
| `--cap <n>` | Max nodes to visit |
| `--pick <n>` | Select nth result when ambiguous |
| `--path <path>` | Disambiguate by file path |
| `--format json` | JSON output |

## Examples

```sh
# What does this depend on and what depends on it?
ix trace UserService

# Only upstream
ix trace validateToken --upstream --depth 3

# Find path between two symbols
ix trace AuthProvider --to DatabaseClient

# Filter by edge type
ix trace parseFile --kind calls --downstream
```
