# OpenSpec workflow (example)

A portable description of the OpenSpec change-proposal loop. Adapt it to
your repository; aurea does not validate framework-specific syntax.

## When to use

Use a change proposal before any non-trivial change: a new capability,
a behavior change, or anything that alters an existing spec.

## Loop

1. **Propose.** Create a change under `openspec/changes/<change-id>/`
   with a `proposal.md` describing the why, the what, and the affected
   specs.
2. **Spec the delta.** Capture the spec changes as `ADDED`,
   `MODIFIED`, or `REMOVED` requirements so the diff against the
   current spec is explicit.
3. **Plan tasks.** Break the change into ordered, verifiable tasks.
4. **Implement.** Work the tasks, keeping the proposal and specs in
   sync with reality.
5. **Archive.** Once merged, fold the delta into the canonical specs
   and archive the change.

## Discoverability

- Root `AGENTS.md` declares `Framework: OpenSpec` and routes here.
- An agent unfamiliar with OpenSpec can read the central aurea
  example for the contract, then follow this file for repo specifics.
