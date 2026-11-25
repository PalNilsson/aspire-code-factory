# Stages

[← README](../README.md)

Every agent stage writes a `result.json` with one of its declared outcomes
(see [interfaces](interfaces.md)). "Done" means the result is valid and the
listed evidence exists as artifacts.

| Stage | Done when | Outcomes |
|---|---|---|
| route | A routing event records the matched rule or fall-through. No model. | entry stage |
| triage | Report: request restated, affected code, reproduction result (for bugs), size and risk. | `needs_spec`, `implement`, `needs_info`, `not_actionable` |
| spec | Spec with product part, technical part and a validation checklist; human gate decides. | `approved`, `changes_requested`, `rejected` |
| implement | Change on a `acf/` branch, static checks and tests pass, merge-risk note written. | `ready_for_review`, `blocked` |
| review | Verdict with reasons; required changes and human questions listed separately. Reviewer does not see the builder's transcript. | `approve`, `changes_required`, `needs_judgement` |
| deliver | Report and/or pull request created; requester notified. | `delivered` |

## The report

Contains the request, triage findings, spec, diff, test evidence, review
verdict, merge-risk note, open questions, cost and duration. A pull request
carries the same content in its description.

Factories add their own skills, merge-risk categories and deliverable
choice; see the [pilot example](pilot-example.md).
