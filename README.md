# automatic-succotash

GitHub Actions orchestration for the raildiag application lifecycle.

**Status: bootstrap / not production-ready.** The application source, deployment paths, cloud resource identifiers and backup contract have not yet been verified. Deployment workflows are gated; the backup transfer and autonomous implementation agent are intentionally not enabled.

## Workflow map

- `01-deploy-production.yml` — Sakura rsync, dry-run by default, no remote deletion.
- `02-daily-backup-s3.yml` — scheduled scaffold; disabled until explicitly enabled and completed.
- `03-deploy-azure-job.yml` — Azure Container Apps Job scaffold.
- `04-deploy-azure-functions.yml` — Azure Functions scaffold.
- `05-ai-pbi-triage.yml` — OpenAI-assisted Issue/PBI draft.
- `06-ai-implementation-handoff.yml` — plan-only handoff artifact; does not execute an agent.
- `07-pr-test-review.yml` — PR diff and workflow YAML validation.

## Start here

1. Read [architecture](docs/architecture.md).
2. Read [operations](docs/operations.md).
3. Configure credentials only after reviewing [secrets](docs/secrets.md).
4. Review [environment template](config/environments.example.yml).

Do not enable production credentials or scheduled backups until the relevant contracts, approvals, tests and rollback/restore procedures are complete.
