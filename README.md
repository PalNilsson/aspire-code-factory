# ASPIRE Code Factory

**A**gentic **S**oftware **P**roduction, **I**ntegration, **R**eview and
**E**ngineering.

An agent-based software factory. Requests arrive from
GitHub, AskPanDA or chat, move through a blueprint of stages
(route → triage → spec → implement → review → deliver), and come out as
reviewed reports or pull requests. Humans approve specs, resolve escalations
and merge.

The core is generic and works on any repository. PanDA support comes as an
optional pack, and the **PanDA Pilot** factory is the first worked example.

**Status:** Phase 0 — design. No code yet.

## Documentation

**Core**

| Document | What it covers |
|---|---|
| [Architecture](docs/architecture.md) | Components, deployment, security |
| [Stages](docs/stages.md) | What each stage must produce |
| [Agents](docs/agents.md) | Current agents and possible future ones |
| [Configuration](docs/configuration.md) | The `factory.yaml` format |
| [Models](docs/models.md) | Model choice, hosting and cost |
| [Data model](docs/data-model.md) | Tasks, runs, events, conversations |
| [Interfaces](docs/interfaces.md) | Runner, agent backends, API, MCP tools |
| [Extensibility](docs/extensibility.md) | Packs, factories and extension points |
| [Chat UI](docs/chat-ui.md) | The chat front end and its modes |

**PanDA**

| Document | What it covers |
|---|---|
| [PanDA pack](docs/panda-pack.md) | AskPanDA, Bamboo and test-job integrations |
| [Pilot example](docs/pilot-example.md) | The PanDA Pilot factory and CERN-PTEST validation |

**Project**

| Document | What it covers |
|---|---|
| [Decisions](docs/decisions.md) | What is decided, proposed and open |
| [Roadmap](docs/roadmap.md) | Phases and exit criteria |

## Principles

- The core is product-agnostic; domain support comes from optional packs.
- Orchestration is deterministic code; agents do the work.
- One append-only event log feeds the dashboard, audit, scoring and replay.
- Configuration is code, changed through reviewed pull requests.
- The reviewer uses a different model family from the builder.
- Agents run sandboxed, with no production credentials, and treat all
  trigger content as untrusted.

## License

Apache License 2.0 — see [LICENSE](LICENSE).
