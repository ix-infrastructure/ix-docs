# Tutorial: Map and Explore a Codebase

This tutorial walks you through using Ix on a real project. We'll use the Ix repo itself as our example, but every step works the same on any codebase.

## Install Ix

```sh
curl -fsSL https://raw.githubusercontent.com/ix-infrastructure/Ix/main/install.sh | sh
```

This installs the CLI, starts the backend containers, and sets up Claude Code hooks if you have Claude installed.

## Clone the example project

```sh
git clone https://github.com/ix-infrastructure/Ix.git
cd Ix
```

## Map the project

```sh
ix map .
```

This is where everything starts. Ix walks through the source files, parses each one with Tree-Sitter, and extracts the structure: every class, function, method, import, and call relationship. Then it groups files into systems based on how they actually connect to each other.

```
System: cli-commands (confidence: 0.93)
  ix-cli/src/cli/commands/docker.ts
  ix-cli/src/cli/commands/upgrade.ts
  ix-cli/src/cli/commands/map.ts
  ix-cli/src/cli/commands/search.ts
  ...

System: core-ingestion (confidence: 0.91)
  core-ingestion/src/parser.ts
  core-ingestion/src/languages.ts
  core-ingestion/src/patch-builder.ts

System: memory-layer (confidence: 0.89)
  memory-layer/src/main/scala/ix/memory/Main.scala
  memory-layer/src/main/scala/ix/memory/routes/...
```

You now have a map of the entire project. Not based on folder names, but on how the code actually couples together.

When you're using an AI assistant, this is what lets it reason about your project at the right level. Instead of reading every file to understand what the project is, it queries the map and gets the architectural layout immediately.

## Search for something

Say you want to understand how the Docker management works.

```sh
ix search registerDockerCommand --kind function
```

```
registerDockerCommand (function) - ix-cli/src/cli/commands/docker.ts:95-340  [score: 100]
```

Ix searches the knowledge graph, not file contents. Results are ranked by relevance and you can filter by kind (class, function, method, file) to cut through noise.

You can also search more broadly:

```sh
ix search docker --kind function
```

This returns all functions with "docker" in the name, ranked by match quality.

## Understand what you found

```sh
ix explain registerDockerCommand
```

```
registerDockerCommand (function) - ix-cli/src/cli/commands/docker.ts:95-340

  Role: CLI command registration
  Importance: High (referenced by oss.ts)

  Contains:
    - docker.start (command)
    - docker.stop (command)
    - docker.status (command)
    - docker.logs (command)
    - docker.restart (command)

  Used by:
    - registerOssCommands (oss.ts)
```

Without reading the file, you know what this function does, what subcommands it registers, and where it fits in the system. You know it's called from `oss.ts`, which is the command registration entry point.

When your AI assistant runs this, it gets the same understanding. It can answer "how does the docker command work?" without reading 340 lines of code. It only reads source when it needs specific implementation details.

## See the structure

```sh
ix overview registerDockerCommand
```

This gives you a quick structural view: what it contains, what the key items are, and where it sits in the architectural hierarchy.

For a broader view, try it on a file:

```sh
ix overview docker.ts
```

## Check what would break

Before changing something, check the blast radius:

```sh
ix impact registerDockerCommand
```

```
registerDockerCommand - entities at risk

  Critical (direct dependents):
    registerOssCommands - ix-cli/src/cli/register/oss.ts
```

This traces the dependency graph outward and categorizes everything by risk level. You see exactly what depends on the thing you're about to change.

This is where Ix becomes essential for an AI assistant. When you ask "can I refactor this?", the assistant can check the impact first and give you an informed answer instead of a guess.

## Follow the connections

How does the ingestion pipeline connect from CLI to database?

```sh
ix trace ingestFiles --downstream
```

This shows the call tree: what `ingestFiles` calls, what those functions call, and so on. You see the full chain from user command to database write.

Go the other direction:

```sh
ix trace ingestFiles --upstream
```

Now you see what calls `ingestFiles` and how it gets triggered.

You can also find the path between two symbols:

```sh
ix trace registerMapCommand --to ingestFiles
```

This shows how two parts of the system connect, even if the path goes through multiple intermediate functions.

## Find the important parts

Which classes or functions are the most central to the project?

```sh
ix rank --by dependents --kind function --top 10
```

These are the load-bearing functions. If you're onboarding, start here. If you're refactoring, be careful here.

```sh
ix rank --by callers --kind function --top 10
```

Most-called functions. The core utilities that everything relies on.

## List everything of a kind

```sh
ix inventory --kind class
ix inventory --kind function --path ix-cli/src/cli/commands/
```

Simple listing. Filter by kind, scope by path.

## Read the code

After you understand the architecture, the dependencies, and the impact, now you read the specific code you need:

```sh
ix read isHealthy
```

```
ix-cli/src/cli/commands/docker.ts:62-70

  62 | function isHealthy(): boolean {
  63 |   try {
  64 |     execFileSync("curl", ["-sf", HEALTH_URL], { stdio: "ignore", timeout: 5000 });
  65 |     execFileSync("curl", ["-sf", ARANGO_URL], { stdio: "ignore", timeout: 5000 });
  66 |     return true;
  67 |   } catch {
  68 |     return false;
  69 |   }
  70 | }
```

You can also read by file and line range:

```sh
ix read ix-cli/src/cli/commands/docker.ts:95-120
```

The point is that reading code is the last step, not the first. You already know what the function does in the system, who calls it, and what depends on it. The source code confirms implementation details.

## Keep it current

Two options:

**Watch mode** runs in the background and re-ingests files as you save them:

```sh
ix watch
```

**Claude Code hooks** do this automatically. Every time you edit a file through Claude, Ix updates the graph in the background. And every time Claude searches for something, Ix injects graph-aware context before the search results come back.

No manual step needed. The graph stays current as you work.

## Coming back tomorrow

This is the key part. When you start a new session, the graph is still there. You don't re-map. You don't re-read files. You pick up where you left off.

Your AI assistant does the same thing. Session two starts with the same understanding that session one built. It queries the graph, gets the architecture, and is productive immediately.

## The flow

```
ix map       See the whole system
ix search    Find what you're looking for
ix explain   Understand what it is and why it matters
ix overview  Quick structural layout
ix impact    Know what breaks before you change it
ix trace     Follow the dependency chain
ix rank      Find the most important pieces
ix read      Read only the code you need
ix watch     Keep it all current
```

Each command gives you a different lens on the same codebase. Start wide, narrow down, analyze, and only read code when you need the specifics.
