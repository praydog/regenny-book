# sdkgenny.Sdk

The `Sdk` is the top level object returned by `sdkgenny.parse()` or `sdkgenny.parse_file()`. It owns the global namespace and controls code generation settings.

## Methods

### `self:global_ns()`

Returns the root [Namespace](types/namespace.md). All types are created within this namespace or nested namespaces below it.

```lua
local ns = sdk:global_ns()
local my_struct = ns:struct("MyStruct")
```

### `self:generate(output_path: string)`

Generates SDK header and source files to the given directory path.

### `self:preamble(text: string)`

Sets the preamble text inserted before all generated code. Returns `self` for chaining.

### `self:postamble(text: string)`

Sets the postamble text inserted after all generated code. Returns `self` for chaining.

### `self:include(header: string)`

Adds a global `#include <header>` directive to the generated output. Returns `self` for chaining.

### `self:include_local(header: string)`

Adds a local `#include "header"` directive to the generated output. Returns `self` for chaining.

### `self:header_extension()` / `self:header_extension(ext: string)`

Gets or sets the file extension for generated header files (default: `.hpp`). Returns `self` when setting.

### `self:source_extension()` / `self:source_extension(ext: string)`

Gets or sets the file extension for generated source files (default: `.cpp`). Returns `self` when setting.

### `self:generate_namespaces()` / `self:generate_namespaces(flag: boolean)`

Gets or sets whether to emit namespace wrappers in generated output. Returns `self` when setting.
