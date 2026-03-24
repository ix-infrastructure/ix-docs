# ix rank

Hotspot discovery — find the most important entities by graph metrics.

## Usage

```sh
ix rank --by <metric> --kind <kind>
```

## Metrics

| Metric | What it measures |
|--------|-----------------|
| `dependents` | How many things depend on this |
| `callers` | How many functions call this |
| `importers` | How many files import this |
| `members` | How many children this contains |

## Flags

| Flag | Description |
|------|-------------|
| `--by <metric>` | **Required**. Ranking metric |
| `--kind <kind>` | **Required**. Entity kind to rank |
| `--top <n>` | Number of results (default: 10) |
| `--path <path>` | Scope to a directory |
| `--exclude-path <path>` | Exclude a directory |
| `--exclude-kind <kinds>` | Exclude entity kinds |
| `--format json` | JSON output |

## Examples

```sh
# Most depended-on classes
ix rank --by dependents --kind class --top 10

# Most-called functions
ix rank --by callers --kind function --top 20

# Largest files by members
ix rank --by members --kind file --path src/

# JSON for analysis
ix rank --by importers --kind class --format json
```
