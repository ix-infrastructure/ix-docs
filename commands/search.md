# ix search

Search the knowledge graph by term. Results are ranked by relevance.

## Usage

```sh
ix search <term>
```

## Ranking

Results are scored by match quality:
1. Exact name + matching kind (highest)
2. Exact name + structural match
3. Exact name (any kind)
4. Partial name match
5. Provenance match
6. Fuzzy match (lowest)

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | Filter by entity kind (class, function, method, file, etc.) |
| `--language <lang>` | Filter by language |
| `--path <path>` | Filter by file path prefix |
| `--limit <n>` | Max results (default: 10) |
| `--include-tests` | Include test files |
| `--tests-only` | Only show test files |
| `--as-of <rev>` | Search at a specific revision |
| `--format json` | JSON output |

## Examples

```sh
# Basic search
ix search UserService

# Find only functions
ix search parseFile --kind function

# Scope to a directory
ix search validate --path src/auth/

# Find Python classes
ix search Handler --kind class --language python

# JSON output for scripting
ix search payment --format json --limit 20
```
