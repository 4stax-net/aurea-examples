# Speckit workflow (example)

A portable description of the Speckit specify/plan/tasks/implement loop.
Adapt it to your repository; aurea does not validate
framework-specific syntax.

## Loop

1. **Specify.** Write the feature spec: the user-facing intent and
   acceptance criteria, free of implementation detail.
2. **Plan.** Turn the spec into a technical plan: architecture,
   contracts, and the constraints that bound the work.
3. **Tasks.** Decompose the plan into ordered, independently
   verifiable tasks.
4. **Implement.** Work the tasks in order, checking each against its
   acceptance criteria before moving on.

## Discoverability

- Root `AGENTS.md` declares `Framework: Speckit` and routes here.
- An agent unfamiliar with Speckit can read the central aurea
  example for the contract, then follow this file for repo specifics.
