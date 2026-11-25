# Decisions

[← README](../README.md)

## Decided

| # | Decision |
|---|---|
| D1 | Build our own orchestrator; no Warp or other hosted factory. |
| D2 | Python 3.12, FastAPI, Pydantic, SQLAlchemy. |
| D3 | Event-sourced task store. |
| D4 | Orchestration is deterministic; the foreman only converses. |
| D5 | Stage transitions are keyed on declared outcomes. |
| D6 | Deterministic router before triage. |
| D7 | Runner interface; containers locally; multi-arch images. |
| D8 | Wrap an existing coding-agent harness rather than writing one. |
| D9 | Reviewer uses a different model family from the builder. |
| D10 | Deliverable is configurable; the pilot starts with reports only. |
| D11 | Factory has its own GitHub identity; `acf/` branches; forks first. |
| D12 | Trigger and log content is untrusted; no production credentials in sandboxes. |
| D13 | Self-improvement changes are reviewed PRs, tested on a held-out benchmark split. |
| D14 | Chat UI is stand-alone, Claude Desktop-style, with history in a left sidebar. |
| D15 | AskPanDA keeps its monitor front end; the chat UI adds it as a mode. |
| D16 | The core is product-agnostic. PanDA support is an optional pack; the pilot is an example factory. |
| D17 | Extensions register through Python entry points; core never imports pack code. |
| D18 | Name: ASPIRE Code Factory (repo `aspire-code-factory`, package `aspire-cf`, prefix `acf`). |
| D19 | Control jobs run on the same queue as test jobs. |

## Proposed

| # | Proposal |
|---|---|
| P1 | Chat UI stack: React, Vite, Tailwind, shadcn/ui; streaming API. |
| P2 | Bamboo is an evidence provider in the PanDA pack, instead of a log-analysis agent. |
| P3 | Validator stage using external executors; for PanDA, test and control jobs on CERN-PTEST. |
| P4 | Open-weight models by default; paid APIs where benchmarks show they pay off (see [models](models.md)). |

## Open questions

1. Which coding-agent harness (it must support OpenAI-compatible endpoints).
2. Which models CERN, NERSC or SLAC host, and what our API licensing allows.
3. Container runtime on the Mac.
4. Which databases the PanDA Kubernetes system provides.
5. What Bamboo returns for a failed job, including the pilot version.
6. CERN-PTEST details: drop-directory location, pickup confirmation, and
   per-job tarball selection (see [pilot example](pilot-example.md)).
7. Whether evidence capture is its own step after route or part of triage.
8. AskPanDA authentication and how a human's identity reaches gate decisions.
