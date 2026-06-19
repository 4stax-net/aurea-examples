# Example: Autospec declaration

This shows one concrete shape for declaring an Autospec-style framework
(specs generated from intent) as the repository's spec-driven
development framework. It is illustrative, not normative — aurea
does not require Autospec.

## Root `AGENTS.md` declaration

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: spec-driven-development
    framework: Autospec
    path: autospec/AGENTS.md
    enforcement: required
```

Autospec generates a candidate specification from a short intent
statement, then has a human or agent review and refine it before
implementation. The root declaration routes agents to this nested
file.

## Local workflow

See [`workflow.md`](workflow.md) for the portable description of the
generate → review → implement flow.
