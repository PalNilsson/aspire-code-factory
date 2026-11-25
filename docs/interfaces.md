# Interfaces

[← README](../README.md)

## Runner

Creates one isolated sandbox per stage run: `start`, `status`,
`stream_logs`, `collect`, `cancel`, `cleanup`. Implementations: local
containers first, Kubernetes Jobs later.

Every sandbox has the same layout:

| Path | Contents |
|---|---|
| `/workspace` | Repository at the snapshot SHA or task branch |
| `/inputs/` | Task context, assembled prompts, attachments |
| `/outputs/result.json` | Required: outcome, summary, artifact list, metrics |
| `/outputs/artifacts/` | Files to keep |

The orchestrator rejects outcomes the stage did not declare.

## Agent backends

- `coding-agent` wraps an existing headless coding-agent harness (file
  edits, commands). Used by triager, spec writer and builder.
- `direct-model` calls a model with prepared context and no workspace
  access. Used by reviewer and reporter.

## Model registry

Maps profiles to provider, model, family (for reviewer independence) and
price. Provider credentials stay with the runner.

## Factory API

One API for the dashboard, chat UI, triggers and AskPanDA. REST plus a
streaming channel (SSE or WebSocket) for messages and run events.

| Resource | Operations |
|---|---|
| Tasks | submit, list, get, cancel, events (streamed), artifacts |
| Gates | list pending, decide |
| Conversations, messages | create, list, search, stream replies, export Markdown |
| Modes | list |
| Findings | get, create fix task |
| Metrics | costs by stage, model and period |
| Webhooks | GitHub trigger |

## MCP tools

`submit_task`, `get_task`, `list_tasks`, `list_pending_gates`,
`decide_gate`. A gate decision always carries the identity of the human it
is made for; an agent can relay but never approve.
