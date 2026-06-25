# Example: `observability` module

An **external** module whose sensor is backed by an **MCP server** — the
static aurea manifest composing with a live runtime. Demonstrates the
`uri` path and the compose-with-MCP story. Illustrative, not normative.

## Producer manifest

```yaml
kind: harness-module
module: observability
version: "1.0.0"
uri: https://github.com/your-org/aurea-module-observability
summary: Logging and telemetry conventions, checked against live signals.
regulates:
  - behaviour
  - maintainability
provides:
  - name: telemetry-guide
    control: guide
    execution: inferential
    path: workflow.md
    description: What to instrument, log levels, and structured-log fields.
  - name: slo-check
    control: sensor
    execution: computational
    path: sensors/slo-check.md
    description: >-
      Queries the team's observability MCP server (e.g. Datadog or Logfire)
      for the changed service's error rate and latency against its SLO.
```

## Adopter declaration

Declared with a `uri`, so the checker treats it as an external module and
records it without resolving the target:

```yaml
kind: harness-adoption
modules:
  # ... your other modules ...
  - module: observability
    version: "1.0.0"
    uri: https://github.com/your-org/aurea-module-observability
    enforcement: advisory
```

## What it demonstrates

Two things nothing else in these examples shows:

1. **External modules.** With a `uri`, the checker emits `PASS module
   observability declared (uri)` — it validates field formats only and does
   not fetch or deep-check the target. This is how a repository adopts a
   module owned by another repo or a community catalog.
2. **Composing with a runtime.** aurea is a static, git-versioned manifest;
   MCP is a live client-server protocol. The `slo-check` sensor is a static
   declaration that *points at* an MCP server for the actual data. aurea
   records that the module is adopted; the MCP server supplies the signal at
   check time. The layers compose cleanly — aurea never needs to run the
   MCP server itself. See [`workflow.md`](workflow.md).
