# Security review workflow (example)

A portable description of how this repository expects security-sensitive
work to be done. Adapt it to your context; aurea does not judge whether a
given control is sufficient.

## Secrets

- Never commit credentials, tokens, or keys — not even in tests or
  examples. Use the secret store and reference by name.
- If a secret is exposed, rotate it first, then scrub history.

## Dependencies

- New dependencies are justified in the PR: what it does, why a library and
  not a few lines, and its license.
- Pin and lock. Update through the dependency bot, not ad-hoc bumps.

## Risky changes

- Authentication, authorization, crypto, deserialization, and anything that
  shells out or touches the filesystem get extra scrutiny.
- The agent describes the trust boundary it is changing and why the change
  is safe.

## The sensors

- `security.yml` runs secret scanning, a dependency audit, and SAST
  (computational — deterministic findings).
- The `security-review` skill reviews diffs and flags risk for a human
  (inferential — judgment, not a gate). Keep it `advisory`: it informs
  review, it does not block on its own.
