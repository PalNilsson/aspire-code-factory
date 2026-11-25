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

## How it works

```mermaid
%%{init: {"flowchart": {"curve": "basis", "nodeSpacing": 40, "rankSpacing": 45}}}%%
flowchart TD
    T(["📥 <b>Trigger</b><br/>GitHub · AskPanDA · chat"])
    R{{"🧭 <b>route</b><br/>fixed rules, no model"}}
    TR["🔍 <b>triage</b><br/>investigate and size"]
    S["📝 <b>spec</b><br/>👤 human approval"]
    I["🛠️ <b>implement</b><br/>change and verify"]
    RV["🔎 <b>review</b><br/>independent model"]
    V["🧪 <b>validate</b><br/>real environment + control"]
    D["📦 <b>deliver</b><br/>report or PR · 👤 merge"]

    T --> R
    R --> TR
    R -.->|clear-cut| I
    TR -->|needs spec| S
    TR -.->|small and clear| I
    S -->|approved| I
    I --> RV
    RV -.->|changes required| I
    RV -->|approve| V
    RV -.->|needs judgement| D
    V -.->|failed| I
    V -->|passed| D

    classDef trigger fill:#dbeafe,stroke:#2563eb,stroke-width:2px,color:#1e3a8a
    classDef rule fill:#f3f4f6,stroke:#6b7280,stroke-width:1.5px,color:#111827
    classDef agent fill:#e0f2f1,stroke:#0f766e,stroke-width:1.5px,color:#134e4a
    classDef human fill:#fef3c7,stroke:#d97706,stroke-width:2px,color:#78350f
    classDef future fill:#f5f3ff,stroke:#7c3aed,stroke-width:1.5px,stroke-dasharray:6 4,color:#4c1d95

    class T trigger
    class R rule
    class TR,I,RV agent
    class S,D human
    class V future

    linkStyle 2,4,9 stroke:#9ca3af,stroke-width:1.5px
    linkStyle 7,10 stroke:#dc2626,stroke-width:1.5px
```

A trigger becomes a task. The **router** uses fixed rules, not a model: it
sends clear-cut work straight to implement and everything else to
**triage**. Triage sends larger or ambiguous work to **spec**, which a human
approves. The builder **implements** and verifies the change. The
**reviewer**, a different model family from the builder, approves it, sends
clear problems back, or passes open questions on to a human. **Validate**
runs the change in a real environment next to a control run; without it,
approved work goes straight to deliver. **Deliver** produces a report, a
pull request or both, and a human decides whether to merge.

| Colour | Meaning |
|---|---|
| Blue | Where work enters |
| Grey | Deterministic step, no model |
| Teal | Agent stage |
| Amber | Stage with a human decision (👤) |
| Purple, dashed | Possible future stage |

Grey dashed arrows are shortcuts; red arrows send work back to implement.
Loops are capped, every task has a budget, and any stage can escalate to a
human (not drawn). Details: [stages](docs/stages.md).

Compared with the generic factory loop this project started from, ASPIRE adds
the deterministic router, the validate stage and the report as an
alternative to a pull request, and requires an independent reviewer.

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