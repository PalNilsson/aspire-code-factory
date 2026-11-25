# Configuration

[← README](../README.md)

A factory is a directory under version control:

```
examples/pilot/
  factory.yaml           # packs, repositories, models, defaults, router, blueprint
  workers/<name>.md      # role prompt + permissions (front matter)
  stages/<name>.md       # stage prompts
  skills/<name>/SKILL.md
  triggers/<name>.yaml   # how work enters
```

## Rules

- Transitions are keyed on each stage's declared outcomes.
- Models are referenced through named profiles, so benchmarks can swap them.
- Budgets and loop limits are explicit.
- Workers declare their secrets (by name) and network allowlist.
- Packs are enabled by name; their parts are referenced by name (see
  [extensibility](extensibility.md)).

## Example (abridged)

```yaml
schemaVersion: acf/v0
id: pilot
packs: [panda]
repositories:
  - id: pilot3
    url: https://github.com/PalNilsson/pilot3   # fork
    access: branch-push
models:
  profiles:
    local:    { provider: openai-compatible, endpoint: http://localhost:11434/v1,
                model: <open-weight-model>, family: <family> }
    builder:  { provider: anthropic, model: claude-sonnet-5-5, family: anthropic }
    reviewer: { provider: openai-compatible, endpoint: <lab-endpoint>,
                model: <open-weight-model>, family: <family> }
defaults:
  budget: { maxCostUsd: 10, maxWallMinutes: 90 }
  limits: { maxReviewRounds: 3, maxSpecRounds: 3 }
router:
  rules:
    - match: { source: github, labels: [docs] }
      to: implement
blueprint:
  start: route
  stages:
    triage:
      worker: builder
      model: local
      prompt: stages/triage.md
      outcomes:
        needs_spec: Open questions remain or the change is large.
        implement: The change is small and clear.
        needs_info: More input is needed.
        not_actionable: Duplicate, fixed or out of scope.
      transitions: { needs_spec: spec, implement: implement,
                     needs_info: needs_human, not_actionable: done }
    # spec, implement, review, deliver follow the same pattern;
    # review adds `independence: different-family`,
    # spec adds `gate: human-approval`,
    # deliver adds `deliverable: [report]`.
```

The schema will be defined as Pydantic models, which also generate a JSON
Schema for editor validation.
