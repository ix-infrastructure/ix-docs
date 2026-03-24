# Ix Documentation

Ix gives your codebase a persistent memory. It parses your source files into a knowledge graph that you and your AI assistant can query across sessions.

## Get Started

1. [What is Ix?](overview.md)
2. [Installation](installation.md)
3. [Tutorial: Map and Explore a Codebase](guides/tutorial.md)

## Guides

- [Using Ix with Claude Code](guides/claude-code.md)
- [Understanding the Knowledge Graph](guides/knowledge-graph.md)
- [GitHub Ingestion](guides/github-ingestion.md)

## Commands

| Command | What it does |
|---------|-------------|
| [map](commands/map.md) | Parse and map codebase architecture |
| [search](commands/search.md) | Search the knowledge graph |
| [explain](commands/explain.md) | Understand a symbol's role and importance |
| [impact](commands/impact.md) | See what breaks if you change something |
| [overview](commands/overview.md) | Quick structural summary |
| [locate](commands/locate.md) | Find where a symbol is defined |
| [read](commands/read.md) | Read source code by symbol or line range |
| [trace](commands/trace.md) | Follow how things connect |
| [inventory](commands/inventory.md) | List entities by kind |
| [rank](commands/rank.md) | Find the most important pieces |
| [watch](commands/watch.md) | Auto-update the graph on file changes |
| [status](commands/status.md) | Check backend health |
| [upgrade](commands/upgrade.md) | Upgrade Ix to the latest version |
| [docker](commands/docker.md) | Manage backend containers |
| [Advanced](commands/advanced.md) | callers, imports, contains, depends, and more |

## Reference

- [Architecture](reference/architecture.md)
- [Supported Languages](reference/languages.md)
- [Configuration](reference/configuration.md)
