# ix status

Show backend health, current graph revision, and stale file count.

## Usage

```sh
ix status
```

## Flags

| Flag | Description |
|------|-------------|
| `--format json` | JSON output |
| `--root <dir>` | Workspace root directory |

## Output

```
Ix Memory: ok
Endpoint:  http://localhost:8090
Revision:  42
Last ingest: 5m ago

Graph is up to date.
```

If files have changed since last ingestion:

```
⚠ 3 file(s) changed since last ingest:
  src/auth/provider.ts
  src/routes/users.ts
  src/middleware/cors.ts

Run ix map to update.
```
