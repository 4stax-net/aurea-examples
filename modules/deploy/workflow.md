# Deploy workflow (example)

A portable description of how changes reach production in this repository.
Adapt it to your infrastructure; aurea does not validate deployment tooling.

## Environments

- Changes flow `dev` → `staging` → `production`. Nothing skips staging.
- Each environment is reproducible from version-controlled config.

## Promotion

- A build is promotable only when the `testing` gate is green on the merge
  commit.
- Promotion is a recorded action (who, what, when), not a manual hotfix.

## Rollback

- Every deploy has a known rollback: the previous release is one command
  away.
- The runbook names the rollback step and who to page. Roll back first,
  diagnose second.

## The sensor

`deploy.yml` refuses to promote unless the test gate passed. Because this
module `requires: [testing, runbook]`, an adopter that declares `deploy`
must also declare those modules — the guide and the rollback playbook are
part of the contract, not optional extras.
