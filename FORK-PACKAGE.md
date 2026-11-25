# Fork.EFCore.BulkExtensions.MIT.SqlServer

An EF Core 11 build of [EFCore.BulkExtensions.MIT](https://github.com/videokojot/EFCore.BulkExtensions.MIT)'s SQL Server package, published from the fork [sovist/EFCore.BulkExtensions.MIT](https://github.com/sovist/EFCore.BulkExtensions.MIT) until upstream releases EF Core 11 support.

- **Same code, assembly and namespaces** as upstream (`using EFCore.BulkExtensions;`): switching between this package and `EFCore.BulkExtensions.MIT.SqlServer` changes only the package reference.
- **Why it exists:** upstream's 10.x packages are compiled against EF Core 10 and throw `MissingMethodException` on EF Core 11 (`GetTableColumnMappings` now takes `IPropertyBase`). The change is offered upstream; once upstream publishes an 11.x package, use that one. This package will then be deprecated in its favor.
- **Versions:** `11.<upstream base version>`, e.g. `11.22.0` is upstream's 22.0 line built for EF Core 11.
- **What differs from upstream 10.22.0:** the EF Core 11 target, and a fix for `BatchUpdate(object)` on EF Core 11, where a SET parameter (`@Price`) could clash with EF Core 11's variable-named query parameter (`@price`) on SQL Server and write the wrong value.

Usage, documentation and issues: see [upstream](https://github.com/videokojot/EFCore.BulkExtensions.MIT). Problems specific to this build: [the fork's issues](https://github.com/sovist/EFCore.BulkExtensions.MIT/issues).

Not affiliated with the upstream maintainers. MIT license, as upstream.
