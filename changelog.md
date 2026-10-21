# Changelog
## 1.2.0
* Update `StyleCop.Analyzers` from 1.2.0-beta.556 to 1.3.149-beta. \
  See their [Release Notes](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/releases) for details. Highlights:
  * Support for C# 8 through C# 15 syntax, including records, primary constructors, collection expressions,
    the `field` keyword and C# 14 extension blocks. Rules used to skip these constructs, so expect new diagnostics.
  * `SA1600` no longer treats extension blocks as private, so their public members must be documented.
  * `SA1407` now covers bitwise and unsigned shift operators, `SA1408` now covers `??` mixed with `&&`/`||`.
  * `SA1017` now requires a space after a parameter attribute list, e.g. `[NotNullWhen(true)] out string? foo`.
  * Code fixes for `SA1201`–`SA1204` and `SA1214` (member ordering) are now enabled.
  * All settings can now be configured via `.editorconfig` in addition to `stylecop.json`.
* Enable new StyleCop rules as errors:
  * `SA1143`: Conditional expressions must be formatted consistently.
  * `SA1315`: Parameter names must match inherited names.
  * `SA1317`: Identifiers must use Latin letters.
  * `SA1653`: Only one part of a partial type may have a `<summary>`.
  * `SA1654`: Use `{T}` rather than escaped `&lt;T&gt;` in `cref` attributes.
* `SX1412` (UTF-8 without BOM) is listed but disabled, like `SA1412`.
* Update `Microsoft.Build.NoTargets` SDK from 3.7.56 to 3.7.134.

## 1.1.0
* Disallow packing without enabling `ContinuousIntegrationBuild`.
* Update `IDisposableAnalyzers` from 4.0.2 to 4.0.8. \
  Excerpt from their [Release Notes](https://github.com/DotNetAnalyzers/IDisposableAnalyzers/blob/master/RELEASE_NOTES.md):
  > #### 4.0.7
  > BUGFIX: Handle target typed new
  >
  > #### 4.0.6
  > BUGFIX: Handle ref struct
  > BUGFIX: Handle target typed new
  >
  > #### 4.0.5
  > BUGFIX: Handle ValueTask
  > BUGFIX: awaited in expression
  >
  > ####  4.0.3
  > * BUGFIX: Handle recursive builder

## 1.0.0
Initial release
