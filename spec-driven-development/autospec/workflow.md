# Autospec workflow (example)

A portable description of an Autospec-style generate/review/implement
loop. Adapt it to your repository; aurea does not validate
framework-specific syntax.

## Loop

1. **State intent.** Capture a short intent statement for the change.
2. **Generate.** Produce a candidate specification from the intent —
   requirements, edge cases, and acceptance criteria.
3. **Review.** A human or agent corrects the generated spec. The spec,
   not the generator, is the source of truth once accepted.
4. **Implement.** Build against the reviewed spec and verify against
   its acceptance criteria.

## Discoverability

- Root `AGENTS.md` declares `Framework: Autospec` and routes here.
- An agent unfamiliar with the framework can read the central
  aurea example for the contract, then follow this file for repo
  specifics.
