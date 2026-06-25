# Observability workflow (example)

A portable description of how this repository expects code to be observable.
Adapt it to your telemetry stack; aurea does not mandate a vendor.

## What to instrument

- Every request path emits a span with the operation, outcome, and duration.
- Errors are logged with enough context to reproduce — ids, not just
  messages — and never include secrets or personal data.
- New user-facing behavior ships with the metric that would show it
  working (or failing) in production.

## Conventions

- Structured logs only: key-value fields, not interpolated prose.
- Log levels mean something: `error` is actionable, `warn` is suspicious,
  `info` is a milestone, `debug` is for development.

## The sensor (MCP-backed)

`sensors/slo-check` queries the team's observability MCP server for the
changed service's error rate and latency against its SLO, and surfaces the
result in review. Keep it `advisory`: live signals are context for a human,
not a deterministic gate. aurea records that the module is adopted; the MCP
server provides the numbers at check time.
