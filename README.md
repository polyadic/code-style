# Polyadic Code Style

[![NuGet Version](https://img.shields.io/nuget/v/Polyadic.CodeStyle)](https://www.nuget.org/packages/Polyadic.CodeStyle)
[![Licence: MIT](https://img.shields.io/badge/licence-MIT-green)](https://github.com/polyadic/code-style/blob/main/LICENSE-MIT)
[![Licence: Apache](https://img.shields.io/badge/licence-Apache-green)](https://github.com/polyadic/code-style/blob/main/LICENSE-Apache)

Shared, opinionated code style for all [Polyadic](https://github.com/polyadic) projects, including
[Funcky](https://github.com/polyadic/funcky). Add one package reference and your project gets the same
analyzers and the same rule configuration as every other Polyadic project.

## What's included

The package pulls in these analyzers as transitive dependencies and configures them:

| Analyzer | Purpose |
|---|---|
| [StyleCop.Analyzers](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) | Formatting, naming, ordering, documentation and readability rules |
| [IDisposableAnalyzers](https://github.com/DotNetAnalyzers/IDisposableAnalyzers) | Detects leaked, double-disposed or un-owned `IDisposable` instances |

On top of that, the package applies the following settings to every project that references it:

* A **rule set** (`Default.ruleset`) that turns most StyleCop and all IDisposable rules into **errors**
  and disables the ones Polyadic deliberately does not follow.
* A **`stylecop.json`** that places `using` directives outside the namespace.
* A **global `.editorconfig`** that enforces file-scoped namespaces and flags unnecessary `using` directives.
* `EnforceCodeStyleInBuild=true`, so `IDE*` rules are checked during the build and not only in the IDE.
* `GenerateDocumentationFile=true`, so analyzers can inspect XML documentation comments (required by `SA0001`).
  Missing documentation itself (`CS1591`) is not reported.
* `nullable` warnings are promoted to errors in C# projects.
* A guard that fails `dotnet pack` for locally built packages so that only CI-built packages reach nuget.org
  (see [Packing](#packing)).

## Usage

Add the package to every project that should follow the code style:

```xml
<PackageReference Include="Polyadic.CodeStyle" Version="..." PrivateAssets="all" />
```

To apply it to a whole repository at once, put the reference into a `Directory.Build.props`
(or `Directory.Packages.props` when using central package management) at the repository root instead:

```xml
<Project>
  <ItemGroup>
    <PackageReference Include="Polyadic.CodeStyle" Version="..." PrivateAssets="all" />
  </ItemGroup>
</Project>
```

`PrivateAssets="all"` keeps the analyzers out of the dependency list of your own package.

The rules are configured with the expectation that CI builds with warnings treated as errors:

```sh
dotnet build /p:TreatWarningsAsErrors=true
```

### Overriding rules

Every rule can be adjusted per repository or per project with an `.editorconfig` entry, which takes
precedence over the packaged rule set:

```ini
[*.cs]
dotnet_diagnostic.SA1600.severity = none   # Elements should be documented
```

For one-off cases, prefer a `#pragma warning disable` or a `[SuppressMessage]` with a justification.

### Packing

`dotnet pack` fails on developer machines with the message
*"Do not publish locally built packages to NuGet.org"*. This is intentional: release packages should be
built by CI, where `ContinuousIntegrationBuild` is set. To pack locally anyway, for example to test a
package, run:

```sh
dotnet pack /p:ContinuousIntegrationBuild=true
```

The guard is skipped for non-packable projects and while generating an API compatibility suppression file
(`ApiCompatGenerateSuppressionFile=true`).

## Releasing

1. Bump `<Version>` in `CodeStyle/CodeStyle.csproj` and add an entry to
   [`changelog.md`](https://github.com/polyadic/code-style/blob/main/changelog.md).
2. Merge to `main`, then create and push a tag matching the version (e.g. `1.2.0`).
3. The [Publish Release Packages](https://github.com/polyadic/code-style/blob/main/.github/workflows/publish-release.yml)
   workflow verifies that the tag matches the version, packs the package and pushes it to nuget.org via
   trusted publishing. No API key is stored in the repository.

## Credits

The `Polyadic.CodeStyle` package started as a direct copy of
[`Messerli.CodeStyle`](https://github.com/messerli-informatik-ag/code-style), licensed under either Apache 2.0 or MIT.

## License

Licensed under either of

* Apache License, Version 2.0
  ([LICENSE-Apache](https://github.com/polyadic/code-style/blob/main/LICENSE-Apache) or <http://www.apache.org/licenses/LICENSE-2.0>)
* MIT license
  ([LICENSE-MIT](https://github.com/polyadic/code-style/blob/main/LICENSE-MIT) or <http://opensource.org/licenses/MIT>)

at your option.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
