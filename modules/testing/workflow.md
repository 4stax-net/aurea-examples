# Testing workflow (example)

A portable description of how this repository expects tests to be written.
Adapt it to your stack; aurea does not validate test framework specifics.

## When to add tests

- Any new behavior ships with tests that would fail without the change.
- Any bug fix ships with a test that reproduces the bug first.
- Refactors keep the existing tests green; they do not rewrite them to pass.

## What to cover

- The contract, not the implementation: assert observable behavior so the
  test survives a refactor.
- The unhappy paths — error handling, empty inputs, boundaries — not only
  the happy path.
- One clear reason to fail per test.

## What "done" means

- The suite passes locally and in CI.
- Coverage stays at or above the repository threshold; new code is covered.
- No test is skipped or marked flaky without a tracking issue.

## The sensor

`.github/workflows/tests.yml` runs the suite and the coverage threshold on
every pull request. Under `enforcement: required`, a failure blocks; while
adopting, run it `report-only` and read the output before promoting.
