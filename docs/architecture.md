# Architecture and workflow contract

## Purpose

This repository holds GitHub Actions orchestration for the raildiag application. The application repository/runtime, deployment paths, Azure resources, backup format and agent runtime have not yet been verified. Workflows therefore begin as guarded scaffolds, not production-ready deployment automation.

## Flow

1. Intake: Azure Functions (planned) receives feedback.
2. Planning: Issue triage creates a PBI/acceptance-criteria draft.
3. Implementation: a human-approved Issue is packaged as an agent handoff. Autonomous code execution is not enabled.
4. QA: PR checks validate diffs and workflow YAML. Application-specific tests are added after the app layout/runtime is confirmed.
5. Release: production and Azure workflows require manual dispatch, explicit confirmation and protected GitHub Environments.

## Workflow inventory

| File | Responsibility | Initial safety |
|---|---|---|
| 01-deploy-production | rsync to Sakura | manual, main only, dry-run default, no --delete |
| 02-daily-backup-s3 | Sakura to S3 backup | scheduled but disabled unless BACKUP_ENABLED=true; transfer intentionally blocked |
| 03-deploy-azure-job | Container Apps Job | manual, staging environment, update scaffold |
| 04-deploy-azure-functions | Functions deployment | manual, staging environment, package/runtime contract pending |
| 05-ai-pbi-triage | Issue to PBI draft | issue comment; OpenAI key required |
| 06-ai-implementation-handoff | Issue to agent handoff | plan-only artifact; no agent execution |
| 07-pr-test-review | PR checks | read-only permissions, diff and YAML validation |

## Trust boundaries

- Issue/PR content is untrusted input. Never interpolate it into shell source.
- Secrets are passed through environment variables and must never be printed.
- Pull request workflows do not receive write permissions or secrets.
- Human approval is required for deployment and for any future agent execution.
- Production deployment must use a protected GitHub Environment with required reviewers.
- Pin third-party Actions to reviewed full commit SHAs before production use.

## Required next design decisions

- Confirm source repository and deployable artifact layout.
- Confirm Sakura document root, deploy user, host-key fingerprints and rollback strategy.
- Define database dump, encryption, retention, off-site transfer and restore-test policy.
- Confirm Azure subscription, resource group, ACR, Container Apps Job and Functions runtime.
- Choose the agent runner, allowed tools, branch strategy and human approval boundary.
- Add application-specific tests and immutable release/evidence records.
