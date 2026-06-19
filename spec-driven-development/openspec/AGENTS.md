# Example: OpenSpec declaration

This shows one concrete shape for declaring [OpenSpec](https://github.com/Fission-AI/OpenSpec)
as the repository's spec-driven development framework. It is
illustrative, not normative — aurea does not require OpenSpec.

## Root `AGENTS.md` declaration

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: spec-driven-development
    framework: OpenSpec
    path: openspec/AGENTS.md
    enforcement: required
```

OpenSpec keeps its own `openspec/AGENTS.md` that instructs agents how
to author and apply change proposals. The root declaration routes
agents there; the nested file carries the framework-specific detail.

## Local workflow

See [`workflow.md`](workflow.md) for the portable description an agent
follows when the local OpenSpec instructions are not enough on their
own.
