# Example: `deploy` module

A module with **dependencies**: you should not be able to adopt a deploy
process without test gates and an operational runbook. Demonstrates
`requires:`. Illustrative, not normative.

## Producer manifest

```yaml
kind: harness-module
module: deploy
version: "1.0.0"
uri: https://github.com/your-org/aurea-module-deploy
summary: How releases reach production here, and what must hold first.
requires:
  - testing      # don't ship what you can't prove
  - runbook      # don't ship without an operational playbook
regulates:
  - behaviour
provides:
  - name: deploy-guide
    control: guide
    execution: inferential
    path: workflow.md
    description: Environments, promotion, and rollback.
  - name: deploy-gate
    control: sensor
    execution: computational
    path: .github/workflows/deploy.yml
    description: Blocks promotion unless the test gate is green.
```

## Adopter declaration

Because `deploy` requires `testing` and `runbook`, an adopting repository
declares all three (at any enforcement level — the requirement is on
presence):

```yaml
kind: harness-adoption
modules:
  - module: aurea
    version: "0.1.0"
    enforcement: required
  - module: testing
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-testing
    enforcement: required
  - module: runbook
    version: "1"
    path: docs/RUNBOOK.md
    enforcement: advisory
  - module: deploy
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-deploy
    enforcement: required
```

## What it demonstrates

The dependency mechanic. The checker emits a single **`module
dependencies satisfied`** result: it enforces the implicit `aurea`
dependency (module zero) and any built-in requirements offline, and
resolves an external module's `requires` when that manifest is available. A
missing required module is a structural failure regardless of enforcement
level — declaring `deploy` without `testing` is malformed, not merely
advisory. See [`workflow.md`](workflow.md).
