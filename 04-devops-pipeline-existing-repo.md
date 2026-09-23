[← Back to Index](index.md)

# DevOps Pipeline: Adding Code to an Existing Repo

The standard CI/CD flow used every time code is added or changed in a repo that
is already set up (see [DevOps: New Repo Setup](03-devops-new-repo-setup.md) for
first-time setup).

## Flow

```mermaid
flowchart TD
    A[Developer: feature branch\nsee Development Guide] --> B[Push branch]
    B --> C[Open Pull Request into main]
    C --> D[CI build triggers automatically]
    D --> E[Lint + unit tests]
    E --> F{Build passes?}
    F -- No --> A
    F -- Yes --> G[Peer review\nper Developer Guidelines]
    G --> H{Approved?}
    H -- No --> A
    H -- Yes --> I[Merge to main]
    I --> J[Release pipeline triggers]
    J --> K[Deploy to Dev]
    K --> L[Automated / manual verification in Dev]
    L --> M{Verified?}
    M -- No --> N[Fix forward or rollback\nsee Code Rollback]
    M -- Yes --> O[Approval gate: promote to Test]
    O --> P[Deploy to Test]
    P --> Q[Verification in Test]
    Q --> R[Approval gate: promote to Prod]
    R --> S[Deploy to Prod]
    S --> T[Post-deploy verification]
    T --> U[Record deployment report\nsee Deployment Reports]
```

## What the Pipeline Does at Each Stage

| Stage | Automated actions |
|-------|--------------------|
| **Build (on PR)** | Checkout, install dependencies, lint, run unit tests against `etl.py` / `main.py`, validate param file schema. |
| **Deploy — Dev** | Sync `dags/`, `src/etl/`, `params/` to the Dev Airflow environment; does **not** modify DB scaffolding automatically. |
| **Deploy — Test** | Same as Dev, against Test environment; requires approval gate. |
| **Deploy — Prod** | Same, against Prod; requires approval gate and typically a change ticket per [Developer Guidelines](06-developer-guidelines.md). |

## Notes

- Param file changes go through the same PR/pipeline flow as code changes — they
  are not edited directly in an environment.
- If a change requires new or altered DB scaffolding tables, that DDL change is
  reviewed in the PR but **applied manually** to each environment in step with
  the deployment (not run automatically by the release pipeline) — see
  [Git Folder Setup](02-git-folder-setup.md).
- Every promotion to Test or Prod should have a corresponding entry in
  [Deployment Reports](08-deployment-reports.md).
