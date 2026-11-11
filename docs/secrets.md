# Secrets and variables

Never commit secret values. Use GitHub Actions Secrets for credentials and repository/Environment Variables for non-secret identifiers.

## Secrets

| Name | Scope | Purpose |
|---|---|---|
| DEPLOY_SSH_PRIVATE_KEY | production Environment | Sakura deploy SSH private key |
| DEPLOY_KNOWN_HOSTS | production Environment | pre-verified SSH known_hosts entry |
| AWS_ACCESS_KEY_ID | backup Environment (future) | restricted S3 writer identity |
| AWS_SECRET_ACCESS_KEY | backup Environment (future) | restricted S3 writer identity |
| AZURE_CREDENTIALS | azure-staging Environment | least-privilege Azure service principal JSON |
| AZURE_FUNCTIONAPP_PUBLISH_PROFILE | azure-staging Environment | Functions publish profile, only if chosen auth requires it |
| OPENAI_API_KEY | repository or triage Environment | OpenAI API access |

## Variables

| Name | Purpose |
|---|---|
| DEPLOY_HOST, DEPLOY_USER, DEPLOY_PATH | Sakura destination |
| BACKUP_ENABLED | exact string `true` enables scheduled backup workflow |
| BACKUP_HOST, BACKUP_USER, BACKUP_COMMAND | reviewed remote backup command contract |
| S3_BUCKET, AWS_REGION | backup destination |
| ACR_NAME, IMAGE_NAME | Azure Container Registry |
| AZURE_RESOURCE_GROUP, CONTAINER_APP_JOB_NAME | Container Apps Job target |
| AZURE_FUNCTIONAPP_NAME, FUNCTIONS_PACKAGE_PATH | Functions target and package |

## Security requirements

- Use separate credentials per environment and purpose.
- Restrict cloud identities to the minimum required resource and actions.
- Prefer OIDC federation over long-lived cloud credentials where supported.
- Rotate keys after personnel changes or suspected exposure.
- Do not use `ssh-keyscan` at runtime as a substitute for verified host keys.
- Do not grant `contents: write` to workflows that only inspect source.
- Configure required reviewers and branch restrictions on production Environments.
- The backup workflow is not a completed backup implementation; do not enable it until encryption, retention and restore tests exist.
