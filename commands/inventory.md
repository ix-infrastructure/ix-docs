# ix inventory

List all entities of a specific kind, optionally scoped to a path.

## Usage

```sh
ix inventory --kind <kind>
```

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | **Required**. Entity kind: class, function, method, file, module, interface, trait, variable |
| `--path <path>` | Scope to a directory |
| `--limit <n>` | Max results (default: 50) |
| `--format json` | JSON output |

## Examples

```sh
# List all classes
ix inventory --kind class

# Functions in a specific directory
ix inventory --kind function --path src/auth/

# All files, JSON output
ix inventory --kind file --format json --limit 100
```
