# Release and versioning policy

Driftplain follows [Semantic Versioning 2.0.0](https://semver.org/). Version numbers describe
compatibility, not task size, development milestones or how long a change took.

## Choosing a version

Classify all changes in the release, then use the highest required bump.

| Bump | When to use it | Example from `1.1.1` |
|---|---|---|
| PATCH | Fix incorrect behavior without changing the supported interface. Also use for a compatible dependency, security or packaging fix that needs a new distributable. | `1.1.2` |
| MINOR | Add backward-compatible functionality, or deprecate a supported interface while keeping it working. | `1.2.0` |
| MAJOR | Change a supported interface in a way that requires consumers or operators to adapt. | `2.0.0` |
| No release | Documentation, comments, tests, development tooling or internal refactoring that does not need a new distributable or deployment configuration release. | No new tag |

A release containing a fix and a feature gets a minor bump. One containing a breaking change
gets a major bump. Minor bumps reset the patch to zero; major bumps reset both minor and patch.
A security fix does not justify silently breaking a supported interface in a patch.

If a compatible internal or build change needs a new image, use a patch. A substantial new
internal capability may warrant a minor release; record the reason. Dependency version numbers
do not determine Driftplain's bump: assess their effect on Driftplain's consumers.

## What compatibility means here

| Release line | Supported interface | Concrete examples |
|---|---|---|
| Backend | HTTP routes, request/response schemas, authentication, CI ingestion and runtime configuration used by the frontend and agents. | New catalog endpoints: minor. Fix an endpoint to match its documented behavior: patch. Remove an endpoint, change a field's type, or require a previously optional input: major. |
| Frontend | Supported user workflows, browser-facing routes and documented configuration. | Add benchmark browsing: minor. Fix a broken chart: patch. Remove a supported workflow without a compatible replacement: major. |
| CI agents | CLI arguments, environment variables, result JSON, exit codes and documented CI gate behavior. | Add an optional task or provider: minor. Fix parsing without changing the contract: patch. Change the meaning of an exit code or remove a required integration path: major. |
| Infrastructure | Terraform module inputs/outputs, operator commands, state/resource identity and documented upgrade procedures. | Add an optional backup capability: minor. Correct a compatible configuration defect: patch. Require manual state migration, disruptive stateful-resource replacement or an incompatible operator interface: major. |
| GitOps | Helm values, resource/storage identities and supported application deployment contracts. | Add an optional chart feature: minor. Bump an image digest for a compatible fix: patch. Ship a new app feature: minor. Deploy a breaking app contract or require a values/storage migration: major. |

For response additions, verify that existing consumers tolerate the new fields or values.
An added enum value can break an exhaustive client. For database changes, an additive migration
that preserves existing API and upgrade behavior is not automatically major. A change that
requires coordinated consumer changes or a destructive data migration is major.
Running an explicit migration job alone does not make a release major.

A GitOps change does not copy the application's version number. Assess the combined deployed
behavior and chart compatibility, then bump GitOps' own sequence. For infrastructure and GitOps,
a feature that creates new optional resources is not the same as replacing existing stateful
resources. Compatible rolling replacements or automatic state moves do not by themselves
require a major bump. Keep existing operational approvals for applies, migrations and deployments.

## Independent version sequences

- Backend uses `vX.Y.Z` tags in the backend repo.
- Frontend uses `vX.Y.Z` tags in the frontend repo.
- Both CI agent images share `agent-vX.Y.Z` tags in the backend repo, independently of the API.
  Use the higher bump required by either agent.
- Infrastructure and GitOps each use their own `vX.Y.Z` tags.
- The overview and portfolio documentation do not get a tag for every documentation change.
  An explicitly requested project checkpoint records component versions without aligning them.

Shared code changes may affect both backend and agents. Review each release line separately.
Helm chart `version` describes changes to that chart; `appVersion` records the application version.
Do not mechanically copy a repo, application or dependency version into every version field.

All current repository release lines have reached 1.x. Continue those sequences, including
across the [image package rename](IMAGE-NAMING.md); do not return to historical 0.x numbering.

## Release procedure

1. Fetch current remote tags and inspect published releases/packages for the relevant sequence.
   Use the last release of that component, not the last commit, last deployment or another
   component's tag, as the comparison point.
2. Review every relevant change since that release. In the PR or release notes, record
   `previous version -> next version`, the bump category and a short compatibility reason.
   A Conventional Commit prefix is a hint, not the decision.
3. Run focused checks for the changed behavior and review the complete release diff. Merge to
   `main`, then tag the reviewed commit. If another release landed meanwhile, recalculate.
4. Create an annotated tag and a GitHub Release for an intentional component release, including
   patches and minors. Release notes describe user-visible changes, compatibility, migration
   steps when needed, and whether deployment occurred. Include image digests when available.
   [GitHub Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
   are release records, not a signal reserved for major versions.
5. Publish through the existing workflows. Tagging or publishing does not authorize deployment,
   a Terraform apply, a database migration or paid model calls.

A merged PR or completed task does not automatically require a release. Bundle changes when
appropriate. An intentionally unstable candidate can use `X.Y.Z-rc.1` and a GitHub prerelease
once the relevant workflow supports that path; do not present it as a stable release.

## Published versions are immutable

Never move, delete, reuse or overwrite published tags or images to correct a versioning mistake.
Keep the `v1.1.0` checkpoints and all existing packages. Document a mistaken bump and use the
correct category on the next release; do not relabel history.

The already-published backend `1.1.1` added public catalog APIs and should have been a minor
release. Preserve it. The next backend release should establish at least the `1.2.0` line,
or use a higher version if subsequent changes or releases require it. Do not continue the
mistaken classification with another patch release.

This policy replaces earlier per-task tagging rules and fixed “next tag” suggestions.
