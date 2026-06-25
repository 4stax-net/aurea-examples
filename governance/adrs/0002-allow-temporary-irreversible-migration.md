---
id: ADR-0002
status: accepted
last_review_date: 2026-06-25
deviates_from: PRIN-EXAMPLE-001
---
# 0002 Allow Temporary Irreversible Migration

## Status

Accepted.

## Statement

The fictional example service will allow one config setting to change
irreversibly during a bounded migration window, deviating from
`PRIN-EXAMPLE-001` until the migration completes.

## Context

`PRIN-EXAMPLE-001` requires configuration changes to be reversible. A
one-time migration moves a legacy setting into a new format; the old
format cannot be reconstructed once the migration runs, so for the
duration of the migration this one setting is not reversible.

The deviation is real and architectural enough to record in an ADR, and
it is also time-boxed, so a temporary exception in
`harness-exceptions.json` tracks the window and points back to this ADR.
The options considered were blocking the migration until reversibility
was solved, silently breaking the principle, or recording the deviation
explicitly with an end date.

## Decision

Record the deviation here as a durable decision and track its window
with a temporary exception that references this ADR. When the migration
completes, remove the exception; this ADR remains as the audit trail.

## Consequences

### Easier

- The migration can proceed without pretending the principle still
  holds for the affected setting.
- The deviation is visible, owned, and time-boxed rather than hidden.

### Harder

- Reviewers must remember this one setting is exempt only until the
  migration completes.
- The exception must be removed once the window closes, or it will be
  reported as expired.

## Related

- Supersedes:
- Superseded by:
