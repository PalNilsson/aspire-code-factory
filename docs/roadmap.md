# Roadmap

[← README](../README.md)

## Factory

| Phase | Scope | Exit criterion |
|---|---|---|
| 0 | Design documents | Open questions 1–3 answered |
| 1 | CLI intake, router, triage → implement → deliver; generic example factory; SQLite event log; local open-weight models | A trivial task completes and can be replayed, with no packs installed |
| 2 | Spec gate, independent reviewer, limits, budgets | Ambiguous task stops at the gate; bad diff is bounced, then escalated |
| 3 | API, dashboard, MCP server | Spec approved from the dashboard; AskPanDA reads task status |
| 4 | PanDA pack (Bamboo), pilot example factory, GitHub trigger | Known historical pilot bugs fixed on the fork with evidence; core tests still pass without the pack |
| 5 | AskPanDA findings become tasks | One real error cluster becomes a reviewed report |
| 6 | Scorers, benchmarks, self-improvement | A configuration change chosen by benchmark |
| 7 | Kubernetes deployment, sign-in | Phase 4 tasks run on the cluster with the same results |

## Chat UI

Runs alongside the factory phases ([details](chat-ui.md)).

| Chat phase | Needs |
|---|---|
| 1 — Local chat | Can start during factory Phase 1 |
| 2 — AskPanDA mode | AskPanDA service API |
| 3 — Factory mode | Factory Phase 3 |
| 4 — Create fix task, Kubernetes | Factory Phases 5 and 7 |

## Future agents

Not scheduled. Candidates after Phase 4, in order: reproducer, validator
(CERN-PTEST test jobs for the pilot), visual presenter. See
[agents](agents.md).
