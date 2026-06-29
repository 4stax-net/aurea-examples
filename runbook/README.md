# Example: operational runbook

This directory shows the shape of the file a `runbook` discovery module
points at. Aurea does not require a runbook. When a repository declares
one, the `runbook` module's `path` names a real file and the compliance
checker confirms it exists, emitting a finding at the module's
enforcement level if it is missing.

## Files

- [`RUNBOOK.md`](RUNBOOK.md) — a small, generic operational runbook for
  a fictional service, covering deploy, rollback, and incident response.

## Declaring it

In your root `AGENTS.md` harness-adoption block, point a `runbook`
module at the file's repository-relative path:

```yaml
  - module: runbook
    version: "1"
    path: docs/RUNBOOK.md
    enforcement: advisory
```

Start at `report-only` or `advisory` while the runbook is taking shape,
then promote to `required` once it is the trusted operational reference.
See [`../harness-adoption/`](../harness-adoption/README.md) for the full
block.

## What a runbook is for

A runbook is the operational counterpart to principles and ADRs: not
*why* the system is built the way it is, but *what an operator does*
when they need to deploy, roll back, or respond to an incident. Keep it
short, current, and procedural — steps an on-call engineer can follow
under pressure without reading the whole codebase.
