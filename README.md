# aurea-examples

Central, copy-ready examples for the [aurea](https://github.com/4stax-net/aurea)
harness spec. These are reference shapes an adopter can browse or vendor
directly; aurea itself does not require any of them.

## Contents

- [`harness-adoption/`](harness-adoption/README.md) — a full, annotated
  `harness-adoption` block spanning every enforcement level, a `runbook` module,
  a bring-your-own module, and a `track: latest` version pin.
- [`governance/`](governance/README.md) — the deviation-tracking tiers worked
  end to end: a principle, the ADRs that support and deviate from it, and a
  temporary `harness-exceptions.json` record.
- [`runbook/`](runbook/README.md) — a generic operational `RUNBOOK.md` (deploy,
  rollback, incident response) that a `runbook` module's `path` points at.
- [`spec-driven-development/`](spec-driven-development/AGENTS.md) — the common
  framework-declaration contract plus concrete OpenSpec, Speckit, and Autospec
  shapes.
- [`modules/`](modules/AGENTS.md) — how to define and adopt your own aurea
  module (the `harness-module` manifest plus the adoption declaration), with
  worked examples: `testing`, `security-review`, `commit-conventions`,
  `deploy` (dependencies), and `observability` (external + MCP-backed).
- [`github-actions/aurea-pr-comment.yml`](github-actions/aurea-pr-comment.yml) —
  a GitHub Action that posts the compliance report as a PR comment.

## Versioning

Tags track the aurea release they were cut alongside. Pin links and fetches to
a tag (for example `v0.1.0`) so a moving `main` never breaks an adopter's
references.
