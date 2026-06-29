# Example: harness-adoption block

This directory shows a full `harness-adoption` block — the single
`kind: harness-adoption` YAML block that lives in a repository's root
`AGENTS.md` and declares which harness modules the repo follows, at
which version, and at which enforcement level.

Aurea does not require every module shown here. The example is
deliberately maximal so you can see every field and every enforcement
level in one place, then delete what you do not need.

## Files

- [`harness-adoption.example.yaml`](harness-adoption.example.yaml) — a
  standalone, annotated block valid against
  [`harness-adoption.schema.json`](https://github.com/4stax-net/aurea/blob/v0.1.6/templates/harness-adoption.schema.json).
  Copy it into the `## Harness Adoption` section of your root
  `AGENTS.md` as a fenced ` ```yaml ` block.

## The enforcement spectrum

Every module declares an `enforcement` value that decides how its
findings are treated:

| `enforcement` | Finding | Blocking? |
| --- | --- | --- |
| `required` | `FAIL` | Yes, under a failure gate. |
| `advisory` | `ADVISORY` | No — non-blocking warning. |
| `report-only` | `REPORT` | No — informational only. |

New adopters usually start a freshly adopted module at `report-only`
and promote it to `advisory`, then `required`, as the repository
becomes ready for its findings to be visible and then blocking.

## Module kinds in this example

- **`aurea`** (`required`) — the harness module itself, pinned to a
  concrete `version` and carrying a `registration:` record written by
  `harness register`.
- **`spec-driven-development`** (`required`) — names the chosen
  `framework` and a local `path` to the workflow file. See
  [`../spec-driven-development/`](../spec-driven-development/AGENTS.md).
- **`readme`** (`required`) — a contract module pinned to an integer
  contract `version`.
- **`runbook`** (`advisory`) — points at a local operational runbook
  via `path`; the checker confirms that file exists. See
  [`../runbook/`](../runbook/README.md).
- **A bring-your-own (BYO) module** (`report-only`) — an
  externally-owned module Aurea does not define, identified by a
  canonical `uri`.

## BYO modules honor their declared enforcement

Since [v0.1.4](https://github.com/4stax-net/aurea/blob/v0.1.6/docs/release-notes/v0.1.4.md),
a module Aurea does not define — not built-in, declared with its own
`uri` — is acknowledged at its declared `enforcement` instead of always
`ADVISORY`:

- `report-only` → a non-blocking `REPORT`.
- `advisory` / `required` → an `ADVISORY`.

An unknown module is **never failed**: Aurea cannot validate a module
it does not define, so it owns the contract and discoverability, not
the content. Declaring a BYO module `report-only` lets a repository run
its own harness module non-blocking even under a strict gate.

## Version pins

A module can pin its version two ways:

- A concrete `version` (a semver for distributed modules, or an integer
  contract version for contract modules like `readme` and `runbook`).
- A `track: latest` policy plus a `resolved:` lockfile field recording
  the concrete version the track currently resolves to. When `track` is
  set, `resolved` is required. The `readme` module below shows this
  shape.

## What the checker does

The checker only runs checks for modules a repository declares. A
module that is not listed is not adopted. For each declared module it
emits a finding at the module's `enforcement` level — for example, a
`runbook` whose `path` is missing fails when `required`, warns when
`advisory`, and is informational when `report-only`.
