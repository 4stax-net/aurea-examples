# Commit conventions workflow (example)

A portable description of the commit message format this repository expects.
Adapt the format to your team; aurea does not mandate a specific convention.

## Format

```
<type>(<scope>): <summary>

<body — what and why, not how>

<footer — issue refs, breaking changes>
```

- `type`: one of `feat`, `fix`, `docs`, `refactor`, `test`, `chore`.
- `summary`: imperative mood, under ~72 characters, no trailing period.
- Breaking changes: start a footer line with `BREAKING CHANGE:`.

## Examples

- `feat(auth): add device-code login flow`
- `fix(parser): handle quoted module ids in the adoption block`

## The sensor

`commitlint.yml` validates each message against the format on every pull
request. It is deterministic, so adopt it `required` once the team has
agreed on the convention.
