# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Overview

A reusable library of custom Azure and third-party PlantUML component definitions.
Consumed over `!includeurl`, so **every change is published the moment it lands on `main`** —
there is no versioning or release step between a merge and every downstream diagram picking it up.

Published at `https://raw.githubusercontent.com/lickfett/PlantUML-Extras/refs/heads/main/dist`.

### Structure

- `dist/azure/azure-extras.puml` — Base Azure styling macros (`AzureExtrasEntityColoring`, `AzureExtrasColorEntity`, `AzureExtrasMonoEntity`)
- `dist/azure/containers.puml`, `databases.puml`, `networking.puml`, `storage.puml`, `web.puml` — Component definitions (also available as `dist/azure/containers/all.puml`, etc.)
- `dist/misc/misc-extras.puml` — Base misc styling
- `dist/misc/zuplo/` — Zuplo API Gateway definitions with PNG/SVG sprites
- `dist/misc/dreamhost/` — Dreamhost DNS definitions
- `dist/common.puml` — Generic `ExtrasEntity` macro and shared skinparams
- `dist/sketchy.puml` — Hand-drawn/sketchy styling override

### Entity Definition Pattern

Each component category follows the same macro pattern:

```plantuml
AzureExtrasEntityColoring(ComponentStereo)   ' registers the stereotype's colors
!define ComponentName(e_alias, e_label, e_techn) ...  ' 3-arg form
!define ComponentName(e_alias, e_label, e_techn, e_color) ...  ' 4-arg form with custom color
```

When adding new components to the library, follow this pattern from the existing files.

### Local Development vs GitHub

The sample file at `samples/extras-test.puml` uses a local path for iterating:

```plantuml
' swap comments for local
' !define Extras https://raw.githubusercontent.com/lickfett/PlantUML-Extras/refs/heads/main/dist
!define Extras dist
```

Consumer diagrams always reference the GitHub URL.

## Consumers

`jaylickfett-net/architecture-jaylickfett-net` is the known consumer. Its diagrams reference this
library by URL and pin nothing, so a breaking change to an existing macro breaks them silently on
their next render. Prefer adding a new macro over changing the signature of one already in use.

When a repurposed sprite becomes commonly reused in a consumer, promote it here — see
*Custom One-off Entities* in that repo's `CLAUDE.md` for the pattern it starts life as.

## Gotchas

Avoid a newline inside a macro's `e_techn` argument. The sprite macros wrap that value in sizing
markup, and a `\n` breaks out of it — the rendered label shows a literal `</size>//`. Use `·` as a
separator instead.
