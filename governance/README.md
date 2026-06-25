# Example: governance — principles, ADRs, and deviations

This directory is a self-contained worked example of Aurea's
governance surface: a **principle**, the **ADRs** that record durable
decisions, and the **exception** record that tracks a temporary,
tactical deviation. The set is internally consistent — the exception
and the deviating ADR both point at the same example principle — so you
can see the whole deviation path end to end.

## Deviation tracking: two tiers

Aurea makes intentional non-compliance visible without pretending every
repository can follow every principle immediately. Use the lightest
durable record that fits the decision:

| Tier | Use when | Record |
| --- | --- | --- |
| **Exception** | Temporary, tactical, with a known resolution path | `harness-exceptions.json` |
| **ADR** | Architectural, long-lived, or policy-level | An ADR with `deviates_from` |

- **Exceptions** expire. Each entry names the `principle_id` it
  deviates from, a `reason`, an accountable `owner`, and an `expires_on`
  review date. The default lifespan is **90 days**; the hard maximum is
  **180 days**. Renewal means editing the date and reason in git, so a
  continued exception stays visible. Do not use an exception for a
  permanent policy fork.
- **ADRs** do not expire. An ADR deviation uses the normal ADR template
  and sets `deviates_from` to the principle ID, explaining the context,
  replacement rule, and consequences. If the decision changes, supersede
  the ADR rather than editing it away.

## Files

- [`harness-exceptions.json`](harness-exceptions.json) — one worked
  exception, valid against
  [`harness-exceptions.schema.json`](https://github.com/4stax-net/aurea/blob/v0.1.6/templates/harness-exceptions.schema.json).
  It deviates from `PRIN-EXAMPLE-001` and references `ADR-0002`.
- [`principles/PRIN-EXAMPLE-001-config-changes-are-reversible.md`](principles/PRIN-EXAMPLE-001-config-changes-are-reversible.md)
  — a complete worked principle (`enforcement_mode: review`) with every
  body section.
- [`adrs/0001-store-config-history-in-the-ledger.md`](adrs/0001-store-config-history-in-the-ledger.md)
  — a durable decision that supports the principle.
- [`adrs/0002-allow-temporary-irreversible-migration.md`](adrs/0002-allow-temporary-irreversible-migration.md)
  — an ADR whose `deviates_from` points at `PRIN-EXAMPLE-001`, showing
  the architectural deviation path.

## How the pieces connect

```
PRIN-EXAMPLE-001  (the principle)
      ├── supported by  ADR-0001  (deviates_from: null)
      ├── deviated from by  ADR-0002  (deviates_from: PRIN-EXAMPLE-001)   ← durable
      └── deviated from by  harness-exceptions.json entry                  ← temporary
              principle_id: PRIN-EXAMPLE-001
              adr: ADR-0002
              expires_on: 2026-09-23   (within 90 days of 2026-06-25)
```

The exception is the temporary, time-boxed record; `ADR-0002` is the
durable decision it points back to. When the migration in `ADR-0002`
completes, remove the exception. The principle's own frontmatter lists
both ADRs under `adrs:`.

## What the checker does

- It validates `harness-exceptions.json` against the schema and, when
  principle sources are available, confirms each `principle_id`
  resolves to a known principle.
- For repos pinned before v0.3, an expired exception produces an
  `ADVISORY`. For repos pinned to v0.3 or newer, an expired exception
  produces a `FAIL`: renew it, remove it, or replace it with an ADR
  before advancing the pin.
- ADRs need the required frontmatter (`id`, `status`,
  `last_review_date`, `deviates_from`) to pass compliance.
