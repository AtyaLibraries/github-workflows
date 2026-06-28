# Atya GitHub Workflows

Reusable GitHub Actions workflows and composite actions for Atya .NET repositories.
Callers must pin workflows by tag. The moving `v1` tag receives compatible updates;
use `v1.0.0` when an immutable reference is required.

## Package CI

The caller job must be named `ci`. This name forms part of the
[frozen required-check contexts](docs/CHECKS.md).

```yaml
# .github/workflows/ci.yml in a library repository
name: CI

on:
  push: { branches: [development, master] }
  pull_request: { branches: [development, master] }
  workflow_dispatch:

permissions:
  contents: read
  checks: write # Required by dorny/test-reporter to publish TRX results.

jobs:
  ci: # MUST be named "ci" because required check names are frozen.
    uses: AtyaLibraries/github-workflows/.github/workflows/dotnet-package-ci.yml@v1
    with:
      solution: ./Caching.sln
      test-project: ./tests/Caching.UnitTests/Caching.UnitTests.csproj
      package-project: ./src/Caching/Caching.csproj
```

Optional inputs are `runners` (a JSON array), `allow-empty-coverage` (default `false`),
`publish-coverage-report` (default `true`), `fail-on-coverage` (default `false`),
`coverage-line-min` (default `0`), and `coverage-branch-min` (default `0`).
The legacy `coverage-min` input is still accepted as a line threshold when
`fail-on-coverage` is enabled and `coverage-line-min` is left at `0`.

## CodeQL

```yaml
jobs:
  codeql:
    uses: AtyaLibraries/github-workflows/.github/workflows/codeql-csharp.yml@v1
    with:
      solution: ./Caching.sln
```

The workflow restores in locked mode and builds in `Release` before CodeQL analysis.

## Dependency Review

```yaml
jobs:
  dependency-review:
    uses: AtyaLibraries/github-workflows/.github/workflows/dependency-review.yml@v1
```

Run this caller on `pull_request`. The gate rejects known vulnerabilities and
dependencies using the denied strong-copyleft licenses configured in the reusable
workflow.
