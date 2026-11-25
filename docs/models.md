# Models

[← README](../README.md)

Models are chosen per stage through named profiles (see
[configuration](configuration.md)), so any stage can switch provider without
code changes. Every provider is reached through an Anthropic or
OpenAI-compatible API.

## Default per stage

| Stage | Default | Reason |
|---|---|---|
| route | none | Deterministic |
| triage, reporter, scorers | Open-weight | Short, well-defined tasks |
| builder | Strongest available | Where quality pays off first |
| reviewer | Open-weight, different family | Independence comes for free |

## Hosting

| Option | Good for | Limits |
|---|---|---|
| Local (Mac Studio: Ollama, LM Studio, llama.cpp, MLX) | Development, short-context stages | Long contexts are slow |
| Lab-hosted (CERN, NERSC, SLAC) | Shared, no per-token cost | Availability to be investigated |
| Commercial API (Anthropic and others) | Builder | Cost; use prompt caching, and batch for benchmarks and scoring |

## Approach

1. Phase 1 runs on local open-weight models and records tokens and cost per
   stage.
2. Paid APIs are introduced stage by stage, where the numbers justify them.
3. From Phase 6, benchmarks on real pilot tasks choose the model for each
   stage.

Open points are listed in [decisions](decisions.md).
