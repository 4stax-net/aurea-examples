---
id: ADR-0001
status: accepted
last_review_date: 2026-06-25
deviates_from: null
---
# 0001 Store Config History In The Ledger

## Status

Accepted.

## Statement

Runtime configuration changes will be written as append-only entries in
a config-history ledger, so the previous value of any setting is always
recoverable.

## Context

The fictional example service changes production behavior through
runtime configuration. `PRIN-EXAMPLE-001` requires those changes to be
reversible, which means the prior value must survive each change.

The options considered were mutating config values in place (no
history), keeping the last value only, or recording an append-only
history of every change.

## Decision

Record every configuration change as a new, immutable entry in a
config-history ledger keyed by setting. The current value is the latest
entry; rolling back means appending the prior entry's value. No entry is
ever edited or deleted in place.

## Consequences

### Easier

- Operators can roll back any setting to a known prior value in one
  step.
- Audits can explain when and why a setting changed.

### Harder

- Every write path must go through the ledger rather than mutating a
  value directly.
- The ledger grows over time and needs a retention policy.

## Related

- Supersedes:
- Superseded by:
