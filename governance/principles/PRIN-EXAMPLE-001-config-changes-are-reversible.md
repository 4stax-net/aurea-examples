---
id: PRIN-EXAMPLE-001
status: trial
domain: example-platform
last_review_date: 2026-06-25
owners:
  - platform-team
deviates_from: null
adrs:
  - ADR-0001
  - ADR-0002
enforcement_mode: review
---
# Configuration Changes Are Reversible

## Statement

Every change to a service's runtime configuration must be reversible:
the previous value must be recoverable so an operator can roll back to
it without reconstructing it by hand.

## Rationale

Configuration is the fastest way to change production behavior and the
fastest way to break it. When the prior value is recoverable, a bad
change is a one-step rollback instead of an incident. This principle
protects the tradeoff between shipping config changes quickly and
keeping a safe path back.

## Enforcement

`enforcement_mode` is `review`. Reviewers check that config changes are
written through the audited history path rather than mutating values in
place, and reject changes that overwrite a value without preserving the
prior one. The durable mechanism is recorded in `ADR-0001`.

## Failure mode

When the previous value is not recoverable, a bad config change forces
operators to reconstruct the old state from memory or scattered logs
under incident pressure. Rollbacks become slow and error-prone, and the
same mistake is easy to repeat.

## Deviations

Temporary, tactical deviations may be recorded in
`harness-exceptions.json` with an owner and an expiry. Architectural,
long-lived, or policy-level deviations require an ADR that sets
`deviates_from` to this principle. `ADR-0002` is the worked example of
such a deviation.
