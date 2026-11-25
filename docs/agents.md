# Agents

[← README](../README.md)

## Current blueprint

| Agent | Stage | Backend | Role |
|---|---|---|---|
| Router | route | Deterministic | Classifies triggers; ambiguous cases go to triage |
| Triager | triage | coding-agent | Investigates request and code, using evidence from packs |
| Spec writer | spec | coding-agent | Writes the spec a human approves |
| Builder | implement | coding-agent | Edits code and runs tests in a sandbox |
| Reviewer | review | direct-model | Reviews the diff; different model family from the builder |
| Reporter | deliver | direct-model | Produces the report and/or pull request |
| Foreman | Chat UI, MCP | Conversational | Answers questions, helps write requests; never moves tasks |

## Possible future agents

None of these is committed to a phase.

| # | Agent | Stage | Purpose |
|---|---|---|---|
| 1 | Reproducer / test writer | after triage | Writes a failing test or minimal reproduction; gives the builder a target and the reviewer evidence |
| 2 | Validator | new `validate` stage, after review | Runs the change in a real environment next to a control run, and compares |
| 3 | Visual presenter | deliver | Diagrams for reports and PRs, drawn from static analysis output, never from memory |
| 4 | Regression bisector | triage | `git bisect` using the reproducer's test |
| 5 | Site-compatibility checker | review | Tests across Python versions and container images |
| 6 | Docs and changelog writer | deliver | Docstrings, docs and release notes in the same PR |
| 7 | Security / dependency auditor | review | Credentials, subprocess use, new dependencies; red-teams trigger injection |
| 8 | Performance profiler | review (by label) | Before/after benchmarks of hot paths |

## Validator

The validator uses an external executor from a pack to run the change in a
real environment, optionally with a control run of the released version in
parallel. Submission and waiting are code; only the comparison uses a model.
The task waits without holding a sandbox.

Outcomes: `passed`, `failed` (back to implement), `inconclusive` (needs
human).

For PanDA this means test jobs on a test queue; see the
[PanDA pack](panda-pack.md) and the [pilot example](pilot-example.md).
