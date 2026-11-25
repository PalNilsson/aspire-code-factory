# Chat UI

[← README](../README.md)

A stand-alone chat in the style of Claude Desktop: the conversation fills the
main view and organised chat history sits in a left sidebar. It is one of
several clients of the [factory API](interfaces.md).

AskPanDA keeps its own front end on the PanDA monitor. The chat UI is a
second entry point to it, as a mode.

## Modes

A mode is a backend profile: system prompt, tools (MCP servers), model and
result renderers. Each chat has a default mode, chosen by the input box; it
can be switched mid-chat and is recorded on each message.

| Mode | Talks to | Credentials | Provided by |
|---|---|---|---|
| Factory | The foreman and factory API | GitHub write via the factory identity | Core |
| AskPanDA | The shared AskPanDA service | PanDA / monitor read access | PanDA pack |

## Diagnose, then fix

AskPanDA findings appear as cards: error class, affected sites and tasks,
example job IDs, monitor links. **Create fix task** starts a factory task
linked to the finding.

## Layout

| Area | Contents |
|---|---|
| Sidebar | New chat, search, pinned, then chats grouped by project and time; mode icon and run-status badge per chat; mode filter |
| Main view | Message stream with collapsible cards for tool calls and agent steps |
| Right panel | Diffs and test output for factory runs; logs and monitor views for AskPanDA |

Also: auto-titles, tags, pin/archive, full-text search and Markdown export
(useful for milestone reports).

## Proposed stack (not yet confirmed)

Web app: React, Vite, Tailwind and shadcn/ui, served locally now and from
Kubernetes later (a Tauri wrapper is optional). Replies and agent steps
stream from the API, and runs outlive the browser tab.

## Phases (proposed)

1. Local chat with streaming, history, search; mode field in the schema.
2. AskPanDA mode.
3. Factory mode: run cards, status badges, diff panel.
4. Create fix task; then containerisation and sign-in for Kubernetes.
