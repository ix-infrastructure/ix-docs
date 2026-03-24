# ix explain

Explain an entity — its role, importance, and structural context.

## Usage

```sh
ix explain <symbol>
```

## What It Does

1. Resolves the symbol in the graph
2. Collects facts: edges, container, children, claims
3. Infers role (e.g., "entry point", "utility", "data model")
4. Scores importance based on dependents
5. Renders a human-readable explanation

## Flags

| Flag | Description |
|------|-------------|
| `--kind <kind>` | Disambiguate by entity kind |
| `--path <path>` | Disambiguate by file path |
| `--pick <n>` | Select nth result when ambiguous |
| `--format json` | JSON output |
| `--raw` | Legacy metadata dump |

## Examples

```sh
# Explain a class
ix explain AuthProvider

# Explain a specific function
ix explain validate --kind function

# Disambiguate
ix explain parse --pick 2

# JSON for LLM consumption
ix explain UserService --format json
```

## Output

```
AuthProvider (class) — auth/provider.ts:15-89

  Role: Core authentication provider
  Importance: High (12 dependents)

  Contains:
    - validateToken (method)
    - refreshSession (method)
    - revokeAccess (method)

  Used by:
    - AuthMiddleware.handle
    - UserRouter.authenticate

  Why it matters:
    Central to the auth system — changes here affect 12 downstream consumers.
```
