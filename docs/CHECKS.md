# Frozen required checks

Phase 4 organization rulesets require these status-check contexts verbatim:

```text
ci / build (ubuntu-latest)
ci / build (windows-latest)
ci / release-source
```

The calling job in each repository **must** be named `ci`. Renaming that job or the jobs
in `.github/workflows/dotnet-package-ci.yml` changes the check contexts and breaks the
organization rulesets.
