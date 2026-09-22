# Driftplain

**AI code review in your CI, using your own model credentials.**

[Open the application](https://driftplain.dev) · [Stable release](https://github.com/Steve-droid/driftplain/releases/tag/v1.1.0) · [Frontend](https://github.com/Steve-droid/driftplain-frontend) · [Backend & agents](https://github.com/Steve-droid/driftplain-backend) · [Infrastructure](https://github.com/Steve-droid/driftplain-infra) · [GitOps](https://github.com/Steve-droid/driftplain-gitops)

Driftplain connects a model catalog to containerized AI agents running inside a developer's
Jenkins pipeline. Configure an agent in the web app, add the generated pipeline stage, and
inspect its findings and recorded usage in the dashboard.

Built and maintained by [Steve Levit](https://github.com/Steve-droid) as a DevOps portfolio
project. This repository is the project overview; implementation lives in the four repositories below.

## What you can do today

| Task | How it runs | CI result |
|---|---|---|
| PR code review | One model call over the pull-request diff and your review preferences | Findings; high or critical findings fail the review stage |
| Security analysis | An OpenCode agent inspects a read-only repository checkout | Vulnerability findings; critical findings fail the security stage |

The agents run on **your own API key** in your CI environment. Credentials remain in Jenkins;
the hosted application receives findings and usage. Agents do not edit or push your code.
Password login and Google sign-in are available, and example projects demonstrate the dashboard.

## How it fits together

1. **Configure:** select a task and model in the React app. The FastAPI backend stores the
   project and generates the Jenkins stage with a project-scoped CI token.
2. **Run:** Jenkins starts the appropriate agent container with your provider credentials
   and the diff or checkout. The agent applies its CI gate and submits findings and usage.
3. **Inspect:** view runs, token usage, cost calculations and feedback on findings in the dashboard.

The current catalog uses task-specific benchmark snapshots. Current cost comparisons reprice
the selected model's token usage at a baseline model's rates; they are **not measured savings
from running two models on the same task**. The hosted chat assistant is currently offline.

## Repositories

| Repository | What to explore |
|---|---|
| [driftplain-frontend](https://github.com/Steve-droid/driftplain-frontend) | React 19, TypeScript, onboarding, dashboards, authentication and browser tests |
| [driftplain-backend](https://github.com/Steve-droid/driftplain-backend) | FastAPI, catalog ingestion, PostgreSQL persistence, authentication, usage accounting and both CI-agent images |
| [driftplain-infra](https://github.com/Steve-droid/driftplain-infra) | Terraform, the original AWS platform, home-server provisioning, identity, backups and recovery runbooks |
| [driftplain-gitops](https://github.com/Steve-droid/driftplain-gitops) | Helm charts, ArgoCD applications, image digest pins, database migration jobs and deployment contract tests |

For local setup and tests, follow the relevant repository's README. The backend's
[agent guide](https://github.com/Steve-droid/driftplain-backend/tree/main/agent) explains CI execution.

## Deployment and engineering

The application runs on a maintained **single-node K3s cluster on a home Ubuntu server**,
exposed through Cloudflare Tunnel. PostgreSQL is managed by CloudNativePG. The original AWS
EKS deployment was retired in September 2026; its Terraform remains available as project history.

- **Image delivery:** GitHub Actions publishes versioned backend/frontend images to GHCR.
- **GitOps:** reviewed image digest changes flow through Helm and ArgoCD; publishing an image
  does not automatically change the running application.
- **Database changes:** explicit migration jobs run separately from application startup.
- **Bounded AI execution:** CI agents enforce resource and token limits; tests use fake model
  clients and fixtures instead of paid API calls.
- **Operations:** health checks, monitoring, encrypted backups and documented restore procedures
  support the maintained deployment.

Start with the [GitOps home profile](https://github.com/Steve-droid/driftplain-gitops/tree/main/argocd/home-server)
and [operations guide](https://github.com/Steve-droid/driftplain-infra/blob/main/home-server/HM5-OPERATIONS.md)
for a deployment walkthrough.

## Stable interview checkpoint

**`v1.1.0`** identifies the September 22, 2026 pre-refactor checkpoint across the four component
repositories. Each release records its exact source commit:

- [Frontend v1.1.0](https://github.com/Steve-droid/driftplain-frontend/releases/tag/v1.1.0)
- [Backend and agents v1.1.0](https://github.com/Steve-droid/driftplain-backend/releases/tag/v1.1.0)
- [Infrastructure v1.1.0](https://github.com/Steve-droid/driftplain-infra/releases/tag/v1.1.0)
- [GitOps v1.1.0](https://github.com/Steve-droid/driftplain-gitops/releases/tag/v1.1.0)

Use these tags for a stable code walkthrough while development continues. For runtime recovery,
the GitOps checkpoint retains the deployed image digests; use the
[recovery runbook](https://github.com/Steve-droid/driftplain-infra/blob/v1.1.0/home-server/RECOVERY.md)
and check database compatibility before restoring a deployment.

## Planned next

The next product iteration separates public benchmark exploration from CI setup, replaces
hypothetical savings with selected-model usage and cost, and adds test generation, CI failure
diagnosis and custom tasks. These features are planned; they are not part of this stable release.
