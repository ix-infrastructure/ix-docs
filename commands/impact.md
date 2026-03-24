# ix impact

Blast-radius analysis — what's at risk if a symbol changes.

## Usage

```sh
ix impact <target>
```

## What It Does

For **containers** (classes, modules): shows child members and how changes propagate outward.

For **leaf symbols** (functions, methods): shows callers, importers, and risk levels.

Risk levels:
- **Critical**: direct callers/dependents
- **High**: one hop away
- **Medium**: two hops
- **Low**: three+ hops

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | Disambiguate by entity kind |
| `--pick <n>` | Select nth result when ambiguous |
| `--depth <n>` | Propagation depth 1-3 (default: 1) |
| `--limit <n>` | Max impacted members to show |
| `--format json` | JSON output |

## Examples

```sh
# What breaks if I change this class?
ix impact UserService

# Deep analysis
ix impact validateToken --depth 3

# JSON for programmatic use
ix impact AuthProvider --format json
```
