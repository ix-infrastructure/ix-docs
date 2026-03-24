# Configuration

## Config File

Location: `~/.ix/config.yaml`

```yaml
endpoint: http://localhost:8090
format: text
workspaces:
  - workspace_name: my-project
    root_path: /Users/me/my-project
    default: true
```

## Settings

| Key | Default | Description |
|-----|---------|-------------|
| `endpoint` | `http://localhost:8090` | Memory Layer URL |
| `format` | `text` | Default output format (`text` or `json`) |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `IX_HOME` | Override Ix home directory (default: `~/.ix`) |
| `IX_ENDPOINT` | Override Memory Layer endpoint |
| `IX_VERSION` | Pin install version |
| `IX_SKIP_BACKEND` | Set to `1` to skip backend in installer |
| `IX_SKIP_HOOKS` | Set to `1` to skip Claude Code hooks in installer |
| `GITHUB_TOKEN` | GitHub token for `--github` ingestion |

## Workspaces

Workspaces let Ix remember project roots. When you run `ix init` or `ix map` in a directory, it registers as a workspace.

```sh
# Show config including workspaces
ix config show

# Set a value
ix config set endpoint http://localhost:8090

# Get a value
ix config get format
```

## All Output Formats

Every command supports `--format json` for machine-readable output. This is useful for:
- Piping to `jq` for filtering
- LLM consumption
- Scripting and automation
