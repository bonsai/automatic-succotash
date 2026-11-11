# Operations

## First run

1. Keep all deployment secrets unset.
2. Review the workflow files and this document.
3. Add repository variables only for non-secret resource identifiers.
4. Add secrets only after the deployment contract and protected Environments are reviewed.
5. Run manual workflows with their default dry-run setting.
6. Do not enable scheduled backups until the backup and restore procedure has passed a test.

## Environments

Create protected GitHub Environments:
- `production`: required reviewers; restrict deployment branches to `main`.
- `azure-staging`: staging-only credentials and resource scope.

## Release procedure

1. Review and approve the PR.
2. Confirm CI evidence and deployment artifact.
3. Run the relevant workflow with dry-run enabled.
4. Review the planned changes.
5. For production rsync, rerun with dry_run=false and type DEPLOY. Environment approval must also pass.
6. Verify the deployed service and retain workflow logs/artifacts.
7. Roll back using the separately documented rollback procedure. Do not improvise destructive sync flags.

## Incident response

- Disable the affected workflow or set its enable variable to false.
- Rotate exposed credentials immediately.
- Preserve workflow run URLs, logs and affected commit SHA.
- Restore from a verified backup only after checking integrity and recovery point.
