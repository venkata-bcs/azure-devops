[← Back to Index](index.md)

# Developer Guidelines

Conventions for working in this repo. Applies on top of the
[Development Guide](01-development-guide.md) workflow.

## Branching & Commits

- Branch per task/ticket, named `feature/<ticket-id>-<short-description>` or
  `fix/<ticket-id>-<short-description>`.
- Keep commits scoped to one logical change; write descriptive commit messages
  referencing the ticket ID.
- Rebase/update from `main` before opening a PR to minimize merge conflicts.

## Code Organization

- One folder per source under `src/etl/<source>/`, containing `pre_etl/`,
  `main.py`, `etl.py`, and `post_etl/` — see
  [Git Folder Setup](02-git-folder-setup.md).
- Keep param files out of code folders; all configuration lives under
  `params/<source>/`.
- Never hard-code environment-specific values (paths, connection strings,
  credentials) in `etl.py` or `main.py` — read them from param files or
  environment variables.

## Pull Requests

- PRs must pass CI build (lint + unit tests) before review.
- At least one reviewer approval required before merge to `main`.
- PR description should state: what changed, which source(s)/DAG(s) affected,
  and whether DB scaffolding changes are included.
- If DB scaffolding DDL is part of the PR, flag it explicitly — it requires a
  manual apply step per [Git Folder Setup](02-git-folder-setup.md) and
  [Deployment Guide](07-deployment-guide.md).

## Testing

- Add/update unit tests for any change to `etl.py` or `main.py` logic.
- Test pre-ETL and post-ETL scripts against sample data before opening a PR.
- Run the full DAG in Dev at least once before requesting review for
  non-trivial changes.

## Param Files

- One param file per source per environment (or a single file with
  environment-scoped sections, per team convention).
- No secrets in param files — use the pipeline's secret store / service
  connections instead.

## DB Scaffolding Changes

- Any change to control/support tables is written as DDL under
  `sql/scaffolding/` and reviewed in the PR like any other code change.
- The actual apply to each environment remains a **manual** step performed
  during deployment — never assume the pipeline applies it automatically.

## Release Readiness

- Every change destined for Test/Prod should have a corresponding entry
  prepared for [Deployment Reports](08-deployment-reports.md) before the
  approval gate is requested.
