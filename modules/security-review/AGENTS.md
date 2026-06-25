# Example: `security-review` module

A module that pairs deterministic scanners with an **inferential** review
agent — and shows why you adopt it `advisory` first and promote later.
Illustrative, not normative.

## Producer manifest

```yaml
kind: harness-module
module: security-review
version: "1.0.0"
uri: https://github.com/your-org/aurea-module-security-review
summary: How agents handle secrets, dependencies, and risky changes here.
regulates:
  - behaviour
  - architecture-fitness
provides:
  - name: security-guide
    control: guide
    execution: inferential
    path: workflow.md
    description: Threat model, secret handling, and the dependency policy.
  - name: scanners
    control: sensor
    execution: computational
    path: .github/workflows/security.yml
    description: Secret scan, dependency audit, and SAST on every PR.
  - name: risk-review
    control: sensor
    execution: inferential
    path: skills/security-review/SKILL.md
    description: An LLM agent that flags risky diffs for human attention.
```

## Adopter declaration

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: security-review
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-security-review
    enforcement: advisory
```

## What it demonstrates

An **inferential sensor** (the risk-review agent) alongside computational
ones — the harness is not only linters and tests. It also models the
**ramp**: an inferential reviewer produces judgment calls, so start it at
`advisory` (findings stay visible but never block), build trust in its
signal, then promote the deterministic scanners to `required` while keeping
the agent advisory. See [`workflow.md`](workflow.md).
