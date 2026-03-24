# What is Ix?

Ix is a persistent knowledge graph for codebases. It parses your source files, builds a graph of how everything connects, and gives LLM assistants structured memory across conversations.

## The Problem

LLMs lose context between sessions. Every new conversation starts from zero — the assistant has to re-read files, re-discover architecture, and re-learn decisions you've already made.

## How Ix Solves It

Ix builds and maintains a versioned graph of your codebase:

- **Entities**: classes, functions, files, modules
- **Relationships**: calls, imports, extends, contains
- **Architecture**: automatically detected system regions and hierarchies

This graph persists across sessions. When an LLM starts a new conversation, it can query Ix to understand the codebase instantly instead of reading files from scratch.

## How It Works

```
Your Code → Tree-Sitter Parser → Knowledge Graph → LLM Queries
              (14+ languages)      (ArangoDB)       (ix CLI)
```

1. **Parse**: Ix uses Tree-Sitter to parse source files into ASTs
2. **Extract**: Functions, classes, imports, call relationships are extracted
3. **Store**: Everything goes into a versioned graph database (ArangoDB)
4. **Query**: The `ix` CLI lets you (or an LLM) query the graph

## Key Features

- **Multi-language**: TypeScript, Python, Java, Scala, Go, Rust, C/C++, Ruby, PHP, C#, and more
- **Versioned**: Every change is tracked with revisions — diff, history, provenance
- **Architectural mapping**: Automatic community detection groups files into systems and subsystems
- **Claude Code integration**: Hooks auto-ingest edited files and front-run searches with graph context
- **Local-only**: Everything runs on your machine — your code never leaves
