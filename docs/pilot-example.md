# Pilot example factory

[← README](../README.md) · [Extensibility](extensibility.md)

The PanDA Pilot factory, in `examples/pilot/`. It uses the
[PanDA pack](panda-pack.md) and is an example, not a requirement.

| Part | Setting |
|---|---|
| Repository | A fork of `pilot3` |
| Skills | `pilot-conventions` (style, logging, error codes), `pilot-harness` (unit tests, Python versions, job-definition replay) |
| Merge-risk additions | Server and Harvester compatibility, error-code semantics, unusual site environments |
| Deliverable | Report only, until the factory has a track record |

## Validation on CERN-PTEST

1. **Package.** A deterministic script builds `pilot3-agent-dev.tar.gz` from
   the task branch, with the task ID embedded. The control build,
   `pilot3-agent-control.tar.gz`, is the released pilot.
2. **Stage.** The executor copies both to the drop directory. A
   Harvester-launched script at CERN picks them up for the `CERN-PTEST`
   queue.
3. **Submit.** Test and control jobs go to `CERN-PTEST` together, so site,
   nodes and timing are the same. They run in parallel and usually finish
   within minutes.
4. **Verify and analyse.** The validator checks from the job log that the
   tagged build ran, then compares test and control.

## To settle

- The fixed tarball name allows one validation at a time; the executor needs
  a lock.
- How the executor knows the new tarball has been picked up before it
  submits.
- How each job tells the pickup script which tarball to use.
