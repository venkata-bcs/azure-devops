[← Back to Index](index.md)

# Deployment Reports

What to record for every deployment to Test or Prod, so releases and rollbacks
(see [Code Rollback](05-code-rollback.md)) can be traced later.

## Report Template

```markdown
## Deployment Report — <YYYY-MM-DD> — <Environment>

- **Ticket(s):** <ticket ID(s)>
- **Commit / build:** <commit hash or build number>
- **Source(s) / DAG(s) affected:** <e.g. source_a>
- **Code changes:** <short summary — pre-ETL / main.py / etl.py / post-ETL>
- **Param file changes:** <yes/no — summary>
- **DB scaffolding changes:** <yes/no — DDL applied, by whom, when>
- **Source folder changes:** <yes/no — created/updated, where>
- **Deployed by:** <name>
- **Approved by:** <name(s)>
- **Verification:** <DAG run ID / result, checks performed>
- **Rollback plan:** <last known-good commit / build>
- **Status:** Success / Rolled back / Partial
- **Notes:** <anything unusual>
```

## Log

Keep one entry per deployment, newest first, in this section (or link out to a
separate log file/board if preferred).

| Date | Environment | Commit/Build | Source(s) | DB Scaffolding Change? | Status |
|------|-------------|--------------|-----------|--------------------------|--------|
| _(add rows here)_ | | | | | |

## Why This Matters

- The [Code Rollback](05-code-rollback.md) procedure depends on knowing the last
  known-good commit/build per environment.
- DB scaffolding changes are manual and not tracked by the pipeline's own
  history — this report is the record of when/where they were applied.
- Gives a presentation-ready log of what shipped, when, and by whom.
