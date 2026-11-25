# QA candidate for same-AZ runner subnets

This release builds commit `def836bb92c9fe2e89924618d8e4885dee86e578` for a temporary QA trial. It includes Quindrix's same-AZ subnet changes with upstream `main` merged and the creation-path conflict resolved. It does not address the existing review comments about retry classification and subnet-exhaustion failover.

Related pull requests:

- https://github.com/Quindrix/terraform-aws-github-runner/pull/1
- https://github.com/github-aws-runners/terraform-aws-github-runner/pull/5304

## Source and artifacts

The release tag targets the candidate source commit. The release workflow lives on a separate branch. `BUILD-METADATA.json` records both commits, tool versions, and the workflow run. The provenance attestation identifies the workflow execution; the attested metadata file identifies the separately checked-out candidate source. Inspect both when verifying the release.

All five Lambda ZIPs are built from the candidate. `SHA256SUMS` covers the ZIPs and build metadata. These CI artifacts have their own hashes and supersede the earlier local packaging check for deployment purposes.

Use the Terraform module pinned to the candidate source commit with Lambda ZIPs from this release. Publishing the release does not deploy QA. It must be published before the existing unauthenticated release downloader can retrieve its assets.

## Validation required before draft creation

- Formatting and lint checks across the Lambda workspaces.
- Full Lambda test suite.
- Packaging and archive-integrity checks for all five Lambda ZIPs.
- Terraform formatting checks on selected integration files.
- Terraform validation for the root, runners, pool, and multi-runner modules.
- Mocked Terraform tests in `modules/runners/tests`.
- No tracked source changes after installation, validation, and packaging.

No AWS deployment or live runner test is performed by this release workflow.

## Rollback baseline

The QA baseline uses `spcaipers-arm/terraform-aws-github-runner` at `v7.10.1-patched`, source commit `b9e3801f17c08eb338bedc16e82fb978416dab7c`, with that release's Lambda ZIPs. It already contains a backport of same-AZ subnet support and idle confirmation. This candidate also incorporates newer upstream changes, so the QA Terraform plan must be reviewed before applying or rolling back.
