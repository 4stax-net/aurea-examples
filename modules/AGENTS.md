# Example: Defining your own aurea module

aurea ships a few built-in module contracts (`spec-driven-development`,
`readme`, `runbook`). But the harness is meant to be extended: a team can
define a module for anything a codebase expects agents to respect —
testing, security, deploys, commit hygiene, observability. This directory
shows the shape.

These examples are **illustrative, not normative**. aurea does not require
any of them; it gives you a contract for declaring your own.

## A module has two sides

**Producer side — the `harness-module` manifest.** The repository that owns
a module publishes a `kind: harness-module` block describing what it
regulates and what it ships. aurea owns this contract and discoverability;
it does not host or vendor the module's content.

```yaml
kind: harness-module
module: testing
version: "1.0.0"
uri: https://github.com/your-org/aurea-module-testing
summary: How this org tests, and the gate that proves it.
implements: null              # omit unless it satisfies a built-in contract
requires:
  - aurea                     # every module implicitly requires module zero
regulates:
  - behaviour                 # maintainability | architecture-fitness | behaviour
provides:
  - name: testing-guide
    control: guide            # guide = steers before the agent acts
    execution: inferential    # inferential = semantic/LLM
    path: workflow.md
    description: When and how to add tests in this repo.
  - name: test-gate
    control: sensor           # sensor = observes after, enables self-correction
    execution: computational  # computational = deterministic (tests, linters)
    path: .github/workflows/tests.yml
    description: Runs the suite and coverage threshold on every PR.
```

**Adopter side — the `## Harness Adoption` entry.** A repository that adopts
the module declares it in its root `AGENTS.md`, with a `version` and an
`enforcement` level:

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: testing
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-testing
    enforcement: required
```

## How the checker sees a custom module

The checker only knows the built-in ids. For your own module:

- **Declared with a `uri`** → treated as an external module: the checker
  records it and emits `PASS module {id} declared ({uri})`. It validates
  field formats only; it does not resolve or deep-check the target.
- **Declared without a `uri`** → treated as an unknown module: `ADVISORY`.
  Fine while you are prototyping a purely local module, but a published
  module should carry its `uri`.

Either way, `enforcement` controls severity, and the same ramp applies:
start at `report-only` or `advisory` while you wire the module up, then
promote to `required` once the guide and sensor are in place.

## The guide/sensor vocabulary

`provides` carries aurea's harness-engineering taxonomy so a module's
purpose is legible to agents and humans:

- `control: guide` — feedforward; steers the agent **before** it acts (a
  playbook, a workflow doc, a Skill).
- `control: sensor` — feedback; observes **after** and enables
  self-correction (a test, a linter, a review agent).
- `execution: computational` — deterministic and fast (CPU: tests, type
  checks, scanners).
- `execution: inferential` — semantic (an LLM review agent or judge).

A complete module usually ships at least one guide and one sensor — the
feedforward and feedback halves of the loop.

## Module dependencies

A module may declare `requires:` other module ids. A repository that adopts
it must also declare every required id (at any enforcement level — the
requirement is on *presence*, not enforcement). Every module other than
`aurea` implicitly requires module zero. See [`deploy/`](deploy/AGENTS.md)
for a worked dependency chain.

## The example modules

Each subdirectory is a distinct module, chosen to show a different part of
the model:

- [`testing/`](testing/AGENTS.md) — the full loop: a guide plus a
  computational sensor, adopted `required`.
- [`security-review/`](security-review/AGENTS.md) — adds an **inferential**
  sensor, and shows the `advisory` → `required` ramp.
- [`commit-conventions/`](commit-conventions/AGENTS.md) — the smallest real
  module: one guide, one computational sensor.
- [`deploy/`](deploy/AGENTS.md) — declares `requires: [testing, runbook]`,
  demonstrating the dependency check.
- [`observability/`](observability/AGENTS.md) — an external module whose
  sensor is backed by an MCP server (static manifest composing with a
  runtime).

Copy the shape that fits, point `uri:` at wherever the module actually
lives, and adapt the `workflow.md` to how your team really works.
