# Example: Spec-Driven Development declaration

This directory shows how an adopter declares spec-driven development
(SDD) in root `AGENTS.md`. aurea does not require any specific SDD
framework. It asks only that a repository **picks one, declares it, and
routes agents to a local workflow file**.

## The common contract

Add a `spec-driven-development` module to the harness-adoption block in
your repository's root `AGENTS.md`:

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: spec-driven-development
    framework: <name of the framework you chose>
    path: <relative/path/to/the/local/workflow/file>
    enforcement: required
```

Three rules make this discoverable across repositories:

1. The declaration lives in the **root** `AGENTS.md` harness-adoption
   block. Nested `AGENTS.md` files may describe framework-specific
   behavior, but they do not satisfy the organization-wide declaration
   on their own.
2. `framework:` names the choice so an agent that does not know the
   framework can look it up.
3. `path:` points to a real file in the repository. The compliance
   checker checks it exists and reports per the module's `enforcement`.

## Choosing a shape

The subdirectories show one concrete shape per framework. They are
illustrative, not normative:

- [`openspec/`](openspec/AGENTS.md) — OpenSpec-style change proposals.
- [`speckit/`](speckit/AGENTS.md) — Speckit-style spec/plan/tasks flow.
- [`autospec/`](autospec/AGENTS.md) — Autospec-style generated specs.

Pick the one that matches your tooling, copy the root declaration into
your own `AGENTS.md`, and adapt the local `workflow.md` to how your
team actually works.

## What the checker does

The checker only runs checks for modules a repository declares. For a
declared `spec-driven-development` module:

- A declared `path:` that does not exist is reported.
- The status follows the module's `enforcement`: `required` fails,
  `advisory` and `report-only` are non-blocking.

Choose `enforcement: advisory` or `report-only` while adopting, then
promote to `required` once the workflow is in place.
