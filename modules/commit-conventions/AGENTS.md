# Example: `commit-conventions` module

The smallest real module: one guide, one computational sensor. A good
reference for the floor of the model, not the ceiling. Illustrative, not
normative.

## Producer manifest

```yaml
kind: harness-module
module: commit-conventions
version: "1.0.0"
uri: https://github.com/your-org/aurea-module-commit-conventions
summary: The commit message format this repo uses, and the lint that checks it.
regulates:
  - maintainability
provides:
  - name: commit-guide
    control: guide
    execution: inferential
    path: workflow.md
    description: The commit format and a few worked examples.
  - name: commitlint
    control: sensor
    execution: computational
    path: .github/workflows/commitlint.yml
    description: Validates message format on every PR.
```

## Adopter declaration

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: commit-conventions
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-commit-conventions
    enforcement: required
```

## What it demonstrates

A minimal, single-purpose module — proof that not every module needs a
large surface. One guide, one deterministic sensor, `regulates:
maintainability`. See [`workflow.md`](workflow.md).
