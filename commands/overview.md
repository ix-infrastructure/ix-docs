# ix overview

One-shot structural summary — what a symbol contains or what surrounds it.

## Usage

```sh
ix overview <target>
```

## What It Does

For **containers** (classes, files, modules): shows children grouped by kind, highlights key items.

For **leaf symbols** (functions, methods): shows the container, siblings grouped by kind, key siblings.

Includes the system path from architectural map data.

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | Disambiguate by entity kind |
| `--path <path>` | Disambiguate by file path |
| `--pick <n>` | Select nth result when ambiguous |
| `--format json` | JSON output |

## Examples

```sh
# What does this class contain?
ix overview UserService

# What's in this file?
ix overview auth/provider.ts

# JSON output
ix overview IngestionService --format json
```
