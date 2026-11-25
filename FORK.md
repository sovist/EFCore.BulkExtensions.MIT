# sovist/EFCore.BulkExtensions.MIT — fork notes

For whoever maintains this fork. `FORK-PACKAGE.md` is the readme of the published package.

## What this fork is for

Upstream ([videokojot/EFCore.BulkExtensions.MIT](https://github.com/videokojot/EFCore.BulkExtensions.MIT)) has no EF Core 11 release, and its 10.x packages fail on EF Core 11 with `MissingMethodException` (`RelationalPropertyExtensions.GetTableColumnMappings` takes `IPropertyBase` in EF Core 11). Until upstream publishes an 11.x package, this fork publishes the EF Core 11 SQL Server build as **`Fork.EFCore.BulkExtensions.MIT.SqlServer`**. `EFCore.BulkExtensions.MIT.*` belongs to upstream, and nuget.org's typosquatting check compares ids without their dots, so an id that differs only in punctuation (`EFCoreBulkExtensions.MIT.SqlServer` was refused) cannot be published: the `Fork.` prefix is what makes the id distinct. Keep it.

## Branches

1. **`main` mirrors upstream.** Sync only (GitHub "Sync fork"); never commit to it.
2. **Feature branches are upstream pull requests.** `features/net11-support` holds only what upstream would merge: no fork package id, URLs or workflows.
3. **`release` is what gets published.** It is `main` plus the pending upstream changes plus the fork-only commits: the package metadata in `EFCore.BulkExtensions.v11.SqlServer.csproj`, `FORK-PACKAGE.md`, `FORK.md` and `.github/workflows/publish-fork.yml`. Bring upstream in by merging `main` into `release`; never rebase `release`, since published packages point at its commits.

When upstream merges an EF Core 11 change (ours or another), sync `main` and merge it into `release`. Where upstream's version of a change differs from ours, take upstream's.

## What is published

Only `NugetProjects/EFCore.BulkExtensions.v11.SqlServer`. Each provider package compiles the whole library in, so it stands alone. The assembly name and namespaces stay `EFCore.BulkExtensions`, so consumers switch packages without touching code.

**Version** = `11.$(BaseVersion)` from `NugetProjects/Directory.Build.props`, upstream's scheme: `11.22.0` is upstream's 22.0 line built for EF Core 11. A release may be that version or a prerelease of it (`11.22.0-preview.1`). Publish a preview first when no consumer has run the build yet.

## Releasing

Push a tag `fork/<version>` on `release`:

```bash
git tag fork/11.22.0-preview.1 release
git push origin fork/11.22.0-preview.1
```

`publish-fork.yml` checks that the version matches the project's (or is a prerelease of it), runs the v11 test project, packs the SQL Server package as `<version>` and pushes it to nuget.org. `workflow_dispatch` reruns a release for an existing tag. A 409 fails the run: nuget.org uses it both for "version already exists" and for "package ID is reserved", so `--skip-duplicate` is not used.

Authentication is nuget.org **Trusted Publishing**: a policy on the package owner's nuget.org account bound to repository `sovist/EFCore.BulkExtensions.MIT`, workflow file `publish-fork.yml`, package `Fork.EFCore.BulkExtensions.MIT.SqlServer`. No secret is stored in the repository.

Upstream's `publish.yml` stays as it is: it publishes on a GitHub release with a `NUGET_APIKEY` secret, which this fork has neither of, so it can never push under upstream's ids.

## Exit

Once upstream publishes `EFCore.BulkExtensions.MIT.SqlServer` 11.x: switch consumers back to it, deprecate `Fork.EFCore.BulkExtensions.MIT.SqlServer` on nuget.org with upstream's package as the alternative, and stop releasing from `release`.
