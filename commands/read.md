# ix read

Read source code — by file path with line range, or by symbol name.

## Usage

```sh
ix read <target>
```

## Formats

```sh
# File with line range
ix read src/auth/provider.ts:10-50

# Symbol name (resolves to file + shows source)
ix read validateToken
```

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | Disambiguate by entity kind |
| `--pick <n>` | Select nth result when ambiguous |
| `--format json` | JSON output |

## Examples

```sh
ix read UserService --kind class
ix read src/main.ts:1-30
ix read processPayment --format json
```
