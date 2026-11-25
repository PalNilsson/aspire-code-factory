# Data model

[← README](../README.md)

The store is event-sourced: an append-only event log is the source of truth;
other tables are projections. Large content (transcripts, diffs, logs) is
stored as artifacts. Untrusted text never goes into event payloads.

## Factory entities

| Entity | Purpose |
|---|---|
| Task | One unit of requested work and its current state |
| Snapshot | Immutable intake record: payload, attachments, repo SHAs, config and model versions, evidence from packs |
| Run | One attempt at one stage: worker, model, outcome, cost |
| Gate | A pending or decided human decision |
| Artifact | Content-addressed file: transcript, spec, diff, log, report |
| Event | Append-only record with actor and small payload |

## Chat entities

| Entity | Purpose |
|---|---|
| Conversation | A chat: title, project, tags, pinned/archived, default mode |
| Message | One message, with the mode it was sent in |
| Mode | A profile: system prompt, tools, model, renderers |
| Finding | A structured result from a chat mode (e.g. AskPanDA: error class, sites, example jobs, links) |
| Conversation ↔ Task | Many-to-many link between chats and factory tasks |
| Finding → Task | Which task a "Create fix task" action started |

Search uses SQLite FTS5 locally; the server database is to be decided (see
[decisions](decisions.md)). SQLAlchemy keeps either option open.

## Main event types

`task.created`, `task.routed`, `stage.entered`, `run.started`,
`run.finished`, `gate.opened`, `gate.decided`, `artifact.created`,
`cost.recorded`, `limit.exceeded`, `task.state_changed`,
`deliverable.created`, `score.recorded`.
