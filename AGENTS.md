# Repository instructions

## Application image names (September 22, 2026)

New releases use `ghcr.io/steve-droid/driftplain-backend`, `driftplain-frontend`,
`driftplain-agent` and `driftplain-agent-security`. Follow the
[image naming policy](https://github.com/Steve-droid/driftplain/blob/main/IMAGE-NAMING.md).
Continue each existing version sequence; do not reset versions, reuse published tags or
delete old `modelmatch-*` packages. The verified starting points are backend 1.1.1,
frontend 1.1.0 and agents 1.1.3; check fresh tags before choosing the next version.

Keep existing production image pins until the new packages are published, public and
verified by an anonymous pull. Update both repository and digest for the first deployment
under a new name. Preserve Kubernetes, database, volume and CI credential/environment names.
This policy overrides older image-naming statements below; it does not authorize a deployment.
