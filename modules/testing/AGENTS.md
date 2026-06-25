# Example: `testing` module

The canonical harness loop in one module: a **guide** that tells an agent
how this repository tests, and a **computational sensor** that proves it.
Illustrative, not normative.

## Producer manifest

```yaml
kind: harness-module
module: testing
version: "1.0.0"
uri: https://github.com/your-org/aurea-module-testing
summary: How this repo tests, and the gate that enforces it.
regulates:
  - behaviour
provides:
  - name: testing-guide
    control: guide
    execution: inferential
    path: workflow.md
    description: When to add tests, what to cover, what "done" means.
  - name: test-gate
    control: sensor
    execution: computational
    path: .github/workflows/tests.yml
    description: Runs the suite plus a coverage threshold on every PR.
```

## Adopter declaration

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: testing
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-testing
    enforcement: required
```

## What it demonstrates

Both halves of the feedback loop in one module: an agent reads
[`workflow.md`](workflow.md) **before** writing code (feedforward), and the
test gate catches regressions **after** (feedback). Adopt at `required`
once the gate is real — failing tests should block.
