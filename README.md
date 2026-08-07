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

The bucket always offers the current release. Asking for an older one with
`scoop install mcscert/jpipe@2.2.0` **does not work** — Scoop can only
reconstruct a non-current version from a manifest carrying an `autoupdate`
block, which this bucket deliberately omits (see [Maintenance](#maintenance)),
so the install aborts with *"does not have autoupdate capability"*.

If you have installed several versions over time, Scoop keeps them on disk and
you can switch between those copies:

```pwsh
scoop reset jpipe@2.2.0    # switch to an already-installed version
```

Otherwise, download the release you need directly from
[jpipe-compiler](https://github.com/jpipe-mcscert/jpipe-compiler/releases).

## Maintenance

Manifests in this bucket are **updated automatically and must not be edited by
hand.** The `release.yml` pipeline in each source repository bumps the
`version`, `url`, and `hash` fields when a release tag is pushed, and commits
the result here. Everything else in a manifest — `depends`, `bin`,
`post_install` — is owned by this repository.

For jPipe, that policy is recorded in
[ADR-0025](https://github.com/jpipe-mcscert/jpipe-compiler/blob/main/docs/adr/0025-mainstream-platform-distribution.md).

## Reporting problems

Issues with a packaged application belong in that application's own repository.
Use this repository's issue tracker only for problems with the manifests
themselves — a bad hash, a missing dependency, or a broken shim.
