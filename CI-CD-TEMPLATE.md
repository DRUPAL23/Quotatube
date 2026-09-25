# CI/CD Template v0.16.0

## Flow
feature -> pull request -> CI -> main -> Docker/GHCR -> production VPS

## Workflows
- CI: validation, lint, test, build
- Docker: build and publish to GHCR
- Deploy: SSH deployment to VPS

## Required production environment secrets
- VPS_HOST
- VPS_USERNAME
- VPS_SSH_KEY

Never commit production secrets.
