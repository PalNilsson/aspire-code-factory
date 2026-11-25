# Extensibility

[← README](../README.md)

The core knows nothing about PanDA. Everything domain-specific comes from
**packs** and **factories**.

| Layer | Contains | Example |
|---|---|---|
| Core | Orchestrator, stage framework, runners, API, dashboard, chat UI | — |
| Pack | Reusable integrations for one ecosystem | [PanDA pack](panda-pack.md) |
| Factory | Configuration for one product: repositories, blueprint, prompts, skills, settings | [Pilot example](pilot-example.md) |

## Extension points

| Point | Adds | PanDA pack provides |
|---|---|---|
| Trigger adapter | A new source of tasks | AskPanDA findings |
| Evidence provider | Data attached to a task and snapshotted | Bamboo job diagnosis |
| Tool | An MCP server workers may call | Bamboo (read-only) |
| External executor | Submit → wait → collect in a real environment | PanDA test jobs |
| Chat mode | A profile for the chat UI | AskPanDA mode |
| Skills, prompts, renderers | Knowledge and output formats | — |

Code-based extensions register through Python entry points. A factory enables
packs by name (`packs: [panda]`) and refers to their parts by name (for
example `executor: panda.test-job`).

## Rules

- Core never imports pack code; a CI import check enforces this.
- Core tests pass with no packs installed, and the generic example factory
  runs without any pack.
- If a pack needs a change to core, the missing piece becomes a generic
  extension point.
- Packs declare the credentials they need by name; the runner or executor
  service supplies them.

## Repository layout

```
src/aspire_cf/           core
packs/panda/             PanDA pack
examples/hello/          generic example factory (no packs)
examples/pilot/          PanDA Pilot factory
```

Packs and factories start in this repository and can move to their own
repositories later, for example next to the code they work on.
