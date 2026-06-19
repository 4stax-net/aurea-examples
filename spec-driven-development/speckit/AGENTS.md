# Example: Speckit declaration

This shows one concrete shape for declaring [Speckit](https://github.com/github/spec-kit)
as the repository's spec-driven development framework. It is
illustrative, not normative — aurea does not require Speckit.

## Root `AGENTS.md` declaration

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: spec-driven-development
    framework: Speckit
    path: speckit/AGENTS.md
    enforcement: required
```

Speckit drives work through a specify → plan → tasks → implement flow.
The root declaration routes agents to this nested file; the local
`workflow.md` describes the flow your team follows.

## Local workflow

See [`workflow.md`](workflow.md) for the portable description of the
Speckit flow.
