# scoop-mcscert

[![Tests](https://github.com/jpipe-mcscert/scoop-mcscert/actions/workflows/ci.yml/badge.svg)](https://github.com/jpipe-mcscert/scoop-mcscert/actions/workflows/ci.yml)

A [Scoop](https://scoop.sh) bucket for software released by
[McSCert](https://www.mcscert.ca/) (McMaster Centre for Software Certification).

## Installation

```pwsh
scoop bucket add mcscert https://github.com/jpipe-mcscert/scoop-mcscert
scoop install mcscert/jpipe
```

Scoop resolves the Java 25 runtime and Graphviz automatically, adding the
`java` bucket if it is not already present.

> [!NOTE]
> The `jpipe` manifest is seeded with placeholder `version`, `url` and `hash`
> values, so `scoop install` does not work yet. It starts working once the
> jpipe-compiler release pipeline publishes a Windows `.zip` asset and pushes
> the real values here — see [Maintenance](#maintenance).

## Manifests

| App | Description | Source |
|-----|-------------|--------|
| `jpipe` | Compiler and language environment for justification models | [jpipe-mcscert/jpipe-compiler](https://github.com/jpipe-mcscert/jpipe-compiler) |

### Installing a specific version

Manifests are updated with one commit per release, so any previously released
version can be installed from the bucket's history:

```pwsh
scoop install mcscert/jpipe@2.3.0    # install a specific release
scoop reset jpipe@2.4.0              # switch back to another installed version
```

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
