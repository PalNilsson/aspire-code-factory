# Architecture

[← README](../README.md)

## Components

| Layer | Parts |
|---|---|
| Interfaces | Chat UI, dashboard, MCP clients (e.g. AskPanDA), triggers (GitHub, CLI, pack adapters) |
| Core | Factory API, orchestrator (blueprint state machine), task store and event log |
| Execution | Runners (one sandbox per stage run), outer loop (scorers, benchmarks) |

Every interface uses the same [API](interfaces.md); there is no private path
into the orchestrator. Domain-specific parts come from packs; see
[extensibility](extensibility.md).

## Inner loop

```
route → triage → spec* → implement ⇄ review → deliver      (* human gate)
```

The router is deterministic. Each agent stage ends with a declared outcome,
which the orchestrator maps to the next stage. Any stage can escalate to
`needs_human`. Loops have round limits and every task has a cost and time
budget. Details: [stages](stages.md).

## Outer loop (Phase 6)

Scorers grade finished runs. Benchmarks replay task snapshots under other
configurations. Self-improvement proposals become ordinary tasks against this
repository, behind a human gate.

## Deployment

| Stage | Where | Database | Runs |
|---|---|---|---|
| Development | Paul's Mac | SQLite | Local containers |
| Testing | aipanda033 | SQLite or server DB | Local containers |
| Production | PanDA Kubernetes | To be decided | Kubernetes Jobs |

Images are multi-arch (arm64 and amd64). Human sign-in (OIDC) is added at the
Kubernetes stage.

## Security

- Trigger content, logs and evidence from packs are untrusted data.
- Sandboxes hold no production credentials. Each worker declares its secrets
  and network egress allowlist; the runner enforces them.
- Pack tools (such as Bamboo) are read-only endpoints on a worker's
  allowlist.
- The factory acts on GitHub through its own identity, pushes only to `acf/`
  branches, and works on forks first.
- External executors (such as PanDA test jobs) run as services with their
  own narrowly scoped credentials, never inside a sandbox.
