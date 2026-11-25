# PanDA pack

[← README](../README.md) · [Extensibility](extensibility.md)

Integrations shared by any PanDA-ecosystem factory (pilot, server,
Harvester, clients).

| Part | Type | Purpose |
|---|---|---|
| AskPanDA intake | Trigger adapter | Error clusters and findings become tasks |
| AskPanDA mode | Chat mode | AskPanDA in the chat UI |
| Bamboo | Evidence provider and tool | Job-failure evidence for triage |
| Test jobs | External executor | Run a change on a PanDA test queue |

## Bamboo evidence

Replaces a dedicated log-analysis agent. Requirements:

- Snapshot Bamboo responses, so replays see the same evidence.
- Record the software version the failed job ran, so agents read that code.
- Treat log content as untrusted.
- Reuse a diagnosis already attached by AskPanDA; call Bamboo only for gaps.

## Test-job executor

1. Stage the build the factory produced where the queue will pick it up.
2. Submit the test job and a control job (released version) together, on
   the same queue, so the build is the only difference. They run in
   parallel.
3. Poll BigPanDA until both finish.
4. Collect job logs as artifacts for the validator agent to compare.

Queue names, job templates, artifact names and locations come from the
factory configuration. The executor runs as a service holding a PanDA token
limited to test queues; sandboxes never hold it.

Later: also collect the HTCondor logs from the worker node, which are
currently only on one aipanda node.
