# Driftplain

AI code review in your CI, using your own model credentials.

[Open the app](https://driftplain.dev) · [Frontend](https://github.com/Steve-droid/driftplain-frontend) · [Backend and agents](https://github.com/Steve-droid/driftplain-backend) · [Infrastructure](https://github.com/Steve-droid/driftplain-infra) · [GitOps](https://github.com/Steve-droid/driftplain-gitops)

Driftplain connects a model catalog to AI agents running in your Jenkins pipeline.
Configure an agent in the web app, add the generated pipeline stage, and view its findings
and usage in the dashboard.

Built and maintained by [Steve Levit](https://github.com/Steve-droid) as a DevOps portfolio
project. This repo is the starting point; the code lives in the four repos below.

## What you can do today

| Task | How it runs | Default CI gate |
|---|---|---|
| PR code review | One model call with the pull-request diff and your review preferences. | High or critical findings fail the stage. |
| Security analysis | An OpenCode agent inspects a read-only repository checkout over multiple steps. | Critical findings fail the stage. |

The agents use your model credentials in your CI environment. Credentials stay in Jenkins;
the hosted app receives findings and usage. Agents do not edit or push your code.
You can sign in with a password or Google, and example projects let you explore the dashboard.

## How it fits together

1. Choose a task and model in the React app. The FastAPI backend saves the project and
   generates a Jenkins stage with a project CI token.
2. Jenkins starts an agent container with your provider credentials and the diff or checkout.
   The agent reports findings, sets the stage's pass or fail result, and submits usage to the API.
3. Open the dashboard to inspect runs, token usage, cost calculations and feedback on findings.

The current model ranking combines task-specific benchmark scores and token prices.
Cost comparisons price a run's token usage at both the selected model's rates and a baseline
model's rates. The baseline is not run, so the difference is an estimate rather than measured
savings. The hosted chat assistant is currently offline.

## Repositories

Start with the frontend for the user flow, then the backend for how it works. The infrastructure
and GitOps repos explain how the application is deployed and operated.

| Repository | What it contains |
|---|---|
| [driftplain-frontend](https://github.com/Steve-droid/driftplain-frontend) | React and TypeScript web app: agent setup, dashboards, authentication and browser tests. |
| [driftplain-backend](https://github.com/Steve-droid/driftplain-backend) | FastAPI service: model catalog, projects, PostgreSQL data, usage accounting and both CI agents. |
| [driftplain-infra](https://github.com/Steve-droid/driftplain-infra) | Terraform for external services, home-server setup, identity, backups and recovery tools. |
| [driftplain-gitops](https://github.com/Steve-droid/driftplain-gitops) | Helm charts and ArgoCD applications that deploy the app and supporting services to Kubernetes. |

Each repo has local setup instructions and a guide to its main components.

## Deployment

The app runs on a single-node K3s cluster on an Ubuntu home server, exposed through
Cloudflare Tunnel. CloudNativePG manages PostgreSQL inside the cluster. The application
moved from AWS EKS in September 2026, and the original Terraform remains available for reference.
AWS still holds storage, identity and DNS resources.

GitHub Actions publishes versioned images to GHCR. A separate PR in the GitOps repo pins an
image digest, then ArgoCD deploys it. Publishing an image does not change the running app.
Database migrations run separately from application startup.

The deployment includes health checks, monitoring, encrypted backups and tested restore
procedures. It depends on one server, its power and its internet connection.

See the [GitOps home profile](https://github.com/Steve-droid/driftplain-gitops/tree/main/argocd/home-server)
and [operations guide](https://github.com/Steve-droid/driftplain-infra/blob/main/home-server/HM5-OPERATIONS.md)
for the configuration and operating details. New images use `driftplain-*` names; existing
`modelmatch-*` packages stay available. Version numbers continue across the rename. See the
[image naming policy](IMAGE-NAMING.md) for package names and release steps.
Kubernetes resources, database names and CI credential identifiers keep their existing names.

## Development

Tests use fake model responses and fixtures without paid API calls. CI agents enforce token
limits, and the security agent also limits steps and runtime.

Work is underway to add public benchmark browsing, separate it from CI setup, and replace
the cost comparison with selected-model usage and cost. The backend already includes read-only
catalog APIs; the browsing UI is still in development. Test generation, CI failure diagnosis
and custom tasks are planned.

For a fixed code walkthrough, the September 22, 2026 `v1.1.0` releases preserve the application
before these changes:

[Overview](https://github.com/Steve-droid/driftplain/releases/tag/v1.1.0) · [Frontend](https://github.com/Steve-droid/driftplain-frontend/releases/tag/v1.1.0) · [Backend](https://github.com/Steve-droid/driftplain-backend/releases/tag/v1.1.0) · [Infrastructure](https://github.com/Steve-droid/driftplain-infra/releases/tag/v1.1.0) · [GitOps](https://github.com/Steve-droid/driftplain-gitops/releases/tag/v1.1.0)
