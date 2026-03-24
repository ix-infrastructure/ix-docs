# ix locate

Resolve a symbol to its definition location in the codebase.

## Usage

```sh
ix locate <symbol>
```

## What It Does

Finds where a symbol is defined — file path, line range, container, and system path. Detects if the file has changed since last ingestion.

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | Filter by entity kind |
| `--path <path>` | Filter by file path |
| `--pick <n>` | Select nth result when ambiguous |
| `--format json` | JSON output |

## Examples

```sh
ix locate validateToken
ix locate UserService --kind class
ix locate parse --path src/ingestion/ --format json
```
