# Driftplain

Compare benchmark evidence. Choose an eligible model for CI. Track usage and results.

[Open the app](https://driftplain.dev) · [Frontend](https://github.com/Steve-droid/driftplain-frontend) · [Backend and agents](https://github.com/Steve-droid/driftplain-backend) · [Infrastructure](https://github.com/Steve-droid/driftplain-infra) · [GitOps](https://github.com/Steve-droid/driftplain-gitops)

Driftplain separates public benchmark exploration from authenticated CI setup. Browse exact
source results, compare matching benchmark groups, and explicitly choose a supported model
for your task. CI agents use your provider credentials in Jenkins; the app receives reported
results and usage. Credentials stay in your CI environment.

Built and maintained by [Steve Levit](https://github.com/Steve-droid) as a DevOps portfolio
project. This repo is the starting point; code lives in the four repositories below.

## Published implementation and rollout status — September 24, 2026

| Surface | Published behavior | Remaining gate |
|---|---|---|
| Public Explore | Anonymous benchmarks/models, source versions, exact comparisons and explanations across a 19-family registry. | Production data import and deployment; unresolved source aliases and explicit coverage gaps. |
| CI setup | Review, security, test generation, failure diagnosis with optional fix, and Other in single-call/OpenCode modes. Explicit evidence/runtime selection and immutable revisions. | Expanded B8–B12 candidates remain pending live verification and operator activation. No eligible runtime means no selectable model. |
| Usage/results | Reported token categories, selected-run cost estimates with complete/partial/unavailable coverage, findings, reports and proposed patches. | Reviewed exact-runtime rates must be separately inserted; missing prices are unavailable. |
| Refresh | Bounded per-source checks, immutable acquisitions, report review and health metrics. | Schedules remain disabled/suspended and outside watched ArgoCD Applications. |

Backend **2.0.0** and frontend **2.0.0** retire weighted recommendations and legacy create/re-pick
actions. Existing projects, tokens, ingestion and history are preserved; re-picks use explicit
setup. Agents remain **1.8.0**, GitOps **1.2.0**. These are published artifacts, **not deployed versions**.
The maintained home profile still pins backend **1.0.25** and frontend **1.0.26**; chat/blob use
fake seams. No rollout, paid call, runtime activation or scheduler enablement occurred in B17.

The accepted base inputs have scores for at least three distinct source labels in 12 of 15
families. AIME has two labels; MRCR v2 and matching RealVuln scores remain absent. Labels are
not reviewed canonical identities: no new selectable-model coverage is inferred. TestGenEval
and supplementary task families do not replace base gaps. Review/security's six net-new verified
models and the new tasks' three-model/two-provider targets remain unmet.

## How it fits together

1. Browse source-attributed evidence independently of CI support. A benchmark score never
   enables a runtime. Task recommendations order only an explicitly selected comparable group.
2. Choose an exact eligible task/model and configure inputs, permissions and Jenkins commands.
   Review/security inspect inputs; Other, named tests and optional diagnosis repair can propose
   bounded patches from disposable checkouts. Validation is separate from model completion.
3. Inspect the executed revision, results, usage and estimated cost. Feedback describes rated
   findings, not recall, equivalence or a quality guarantee. Estimates are not provider invoices.

Historical baseline calculations remain compatibility data, not measured savings. There is
no blended price/quality ranking, cheapest-model promise or automatic custom-task recommendation.
See the component READMEs for the breaking upgrade boundary, rollout order and remaining gates.

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

B1–B17 implementation delivery is complete. Live model verification, reviewed aliases/rates,
production imports/migrations and deployment remain separate operational work. Rollout requires
backup/restore evidence, additive backend schema/API, compatible agents, frontend, then separately
reviewed GitOps image and schedule changes. Rollback retains additive schema and immutable history;
disable affected entry points/importers rather than downgrading or restoring over new records.

For a fixed code walkthrough, the September 22, 2026 `v1.1.0` releases preserve the application
before these changes:

[Overview](https://github.com/Steve-droid/driftplain/releases/tag/v1.1.0) · [Frontend](https://github.com/Steve-droid/driftplain-frontend/releases/tag/v1.1.0) · [Backend](https://github.com/Steve-droid/driftplain-backend/releases/tag/v1.1.0) · [Infrastructure](https://github.com/Steve-droid/driftplain-infra/releases/tag/v1.1.0) · [GitOps](https://github.com/Steve-droid/driftplain-gitops/releases/tag/v1.1.0)
