# Supported Languages

Ix uses Tree-Sitter grammars to parse source files. The following languages are supported:

| Language | Extensions |
|----------|-----------|
| TypeScript | `.ts`, `.tsx` |
| JavaScript | `.js`, `.jsx`, `.mjs`, `.cjs` |
| Python | `.py` |
| Java | `.java` |
| Scala | `.scala`, `.sc` |
| Go | `.go` |
| Rust | `.rs` |
| C | `.c`, `.h` |
| C++ | `.cpp`, `.cc`, `.cxx`, `.hpp` |
| C# | `.cs` |
| Ruby | `.rb` |
| PHP | `.php` |
| Kotlin | `.kt`, `.kts` |
| Swift | `.swift` |

## Config Files

These are also ingested but with simpler parsing:

| Format | Extensions |
|--------|-----------|
| JSON | `.json` |
| YAML | `.yaml`, `.yml` |
| TOML | `.toml` |
| Markdown | `.md` |

## What Gets Extracted

For each supported language, Ix extracts:

- **Definitions**: classes, interfaces, traits, functions, methods, variables
- **Relationships**: function calls, imports, type references, inheritance
- **Structure**: file → module → class → method containment hierarchy
- **Position**: line start and end for each entity

## Ignored Directories

These are automatically skipped during ingestion:

`node_modules`, `.git`, `dist`, `build`, `target`, `.next`, `.cache`, `__pycache__`, `.ix`, `.claude`
