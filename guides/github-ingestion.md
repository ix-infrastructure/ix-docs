# GitHub Ingestion

Ix can ingest GitHub issues, PRs, and commits into the knowledge graph.

## Usage

```sh
ix map --github owner/repo
```

## Authentication

Auth is resolved in order:
1. `--token <pat>` flag
2. `GITHUB_TOKEN` environment variable
3. `gh auth token` (GitHub CLI)

## Flags

| Flag | Description |
|------|-------------|
| `--github <owner/repo>` | GitHub repository to ingest |
| `--token <pat>` | Personal access token |
| `--since <date>` | Only ingest items after this date (ISO 8601) |
| `--limit <n>` | Max items to ingest (default: 50) |

## Examples

```sh
# Ingest recent activity
ix map --github ix-infrastructure/Ix --limit 50

# Ingest since a date
ix map --github myorg/myrepo --since 2026-01-01

# With explicit token
ix map --github myorg/private-repo --token ghp_xxxx
```

## What Gets Ingested

- **Issues**: title, body, labels, state, author
- **Pull Requests**: title, body, state, merge status, author
- **Commits**: message, author, changed files

These become entities in the graph with relationships to code entities they reference.
