# scoop-mcscert

[![Tests](https://github.com/jpipe-mcscert/scoop-mcscert/actions/workflows/ci.yml/badge.svg)](https://github.com/jpipe-mcscert/scoop-mcscert/actions/workflows/ci.yml)

A [Scoop](https://scoop.sh) bucket for software released by
[McSCert](https://www.mcscert.ca/) (McMaster Centre for Software Certification).

## Installation

```pwsh
scoop install git
scoop bucket add java
scoop bucket add mcscert https://github.com/jpipe-mcscert/scoop-mcscert
scoop install mcscert/jpipe
```

All four steps are required:

- **`git`** — `scoop bucket add` clones the bucket repository, and Scoop
  refuses to add a bucket without it.
- **the `java` bucket** — jPipe depends on `java/temurin25-jre`. Scoop
  resolves dependencies only against buckets you have already added; it will
  **not** add a missing one for you, and the install fails if `java` is
  absent. Graphviz comes from `main`, which Scoop adds by default, so it
  needs no equivalent step.

## Manifests

| App | Description | Source |
|-----|-------------|--------|
| `jpipe` | Compiler and language environment for justification models | [jpipe-mcscert/jpipe-compiler](https://github.com/jpipe-mcscert/jpipe-compiler) |

### Installing a specific version

The bucket manifest always names the current release, but an older one can be
requested explicitly:

```pwsh
scoop install mcscert/jpipe@2.3.0    # install a specific release
scoop reset jpipe@2.4.0              # switch to an already-installed version
```

Scoop reconstructs the older manifest from the `autoupdate` block: it
substitutes the requested version into the download URL and computes the hash
by downloading the archive. **Versions before 2.3.0 are not available this
way** — they predate Windows packaging and publish no `.zip` asset, so the
download 404s. Take those from the
[jpipe-compiler releases page](https://github.com/jpipe-mcscert/jpipe-compiler/releases)
instead.

## Maintenance

Manifests in this bucket are **updated automatically and must not be edited by
hand.** The `release.yml` pipeline in each source repository bumps the
`version`, `url`, and `hash` fields when a release tag is pushed, and commits
the result here. Everything else in a manifest — `depends`, `bin`,
`post_install`, `autoupdate` — is owned by this repository.

Manifests carry `autoupdate` but deliberately **no `checkver`**: the block is
used only to reconstruct a manifest when someone asks for an explicit
`@version`. Nothing in this repository polls for new releases or updates
itself — the source repository's release pipeline remains the only writer.

For jPipe, that policy is recorded in
[ADR-0025](https://github.com/jpipe-mcscert/jpipe-compiler/blob/main/docs/adr/0025-mainstream-platform-distribution.md).

## Reporting problems

Issues with a packaged application belong in that application's own repository.
Use this repository's issue tracker only for problems with the manifests
themselves — a bad hash, a missing dependency, or a broken shim.
