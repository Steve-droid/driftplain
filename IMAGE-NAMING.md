# Container image naming

From September 22, 2026, new application releases publish to `ghcr.io/steve-droid/driftplain-*`.
Existing `modelmatch-*` packages, tags and digests stay available. There is no package deletion,
history rewrite or version reset.

## Package names and versions

| Component | Existing package | New package | Git tag |
|---|---|---|---|
| Backend | `modelmatch-backend` | `driftplain-backend` | `vX.Y.Z` in the backend repo |
| Frontend | `modelmatch-frontend` | `driftplain-frontend` | `vX.Y.Z` in the frontend repo |
| Review agent | `modelmatch-agent` | `driftplain-agent` | `agent-vX.Y.Z` in the backend repo |
| Security agent | `modelmatch-agent-security` | `driftplain-agent-security` | The same `agent-vX.Y.Z` tag |

All package names above use the `ghcr.io/steve-droid/` prefix. Image tags are plain `X.Y.Z`,
without `v` or `agent-v`. Backend, frontend and agents have separate version sequences;
the two agent images share a version.

At the naming change, the latest published versions were:

| Release line | Last version under the old name | Next patch version |
|---|---|---|
| Backend | `1.1.1` | `1.1.2` |
| Frontend | `1.1.0` | `1.1.1` |
| Both agents | `1.1.3` | `1.1.4` |

These are the versions verified on September 22, 2026, not permanent next-version settings.
Before releasing, fetch the repo's tags and check both the old and new packages. Continue
from the latest version in that release line, using a minor or major bump when appropriate.
Do not reuse an old version just because the new package is empty. The `v1.1.0` checkpoint
and all other published tags remain immutable.

## Release and deployment

1. Use the release workflows on `main` in the backend and frontend repos. They publish only
   to the new package names. Do not rerun an older workflow revision to publish another
   `modelmatch-*` release.
2. Push the next release tag after the code is reviewed and tested. The agent workflow
   builds both agent images from the same tag. A naming change alone does not require
   republishing historical images.
3. On the first publication, check each new GHCR package's visibility. New packages
   [default to private](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#pushing-container-images).
   Make each package public and verify an
   anonymous pull before using it in a deployment or a user's Jenkins stage.
4. For a frontend or backend deployment, update both `image.repository` and `image.digest`
   in GitOps `charts/modelmatch/values-home-server.yaml`. Keep
   `image.registry: ghcr.io/steve-droid`. Replacing only the digest would still select
   the old package.
5. For agents, update the complete `AGENT_IMAGE` and `AGENT_SECURITY_IMAGE` references
   in the backend configuration to the new packages and their verified digests. Existing
   Jenkins stages pinned to old images continue to work.
6. If a backend release needs a migration, select the new backend package for that explicit
   migration too. The PostgreSQL chart uses `migrate.image.registry`,
   `migrate.image.repository` and `migrate.image.tag`. Do not enable migration or seed
   jobs as a side effect of renaming images.

The current home-server deployment keeps its existing image pins until a separate release
is ready. Old image references remain valid for rollback and historical records.

## Names that stay unchanged

This change affects application image packages and local image examples. It does not rename
Helm charts, Kubernetes objects, databases, volumes, Compose projects, metrics, API identifiers,
Jenkins credentials or `MODELMATCH_*` environment variables.

The `home-server-backup` package also keeps its existing name. Retired EKS/Jenkins configuration
and historical evidence retain the names they actually used.

## Instructions for future coding sessions

Use the new package names for builds, release workflows, local image defaults and new
deployment references. Preserve old packages and existing deployment pins until their
replacement images have been published and verified. This policy supersedes older instructions
that say all images must retain the ModelMatch name.
