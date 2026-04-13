# sdkgenny

This is the global API for [luagenny](https://github.com/praydog/luagenny). It provides functions for parsing `.genny` type definitions and constructing overlay objects for reading/writing typed memory.

## Functions

### `sdkgenny.parse(data: string)`

Parses an in-memory genny schema string and returns an [Sdk](sdkgenny/sdk.md) object.

```lua
local sdk = sdkgenny.parse([[
struct Foo {
    int a;
    float b;
};
]])
```

### `sdkgenny.parse_file(filename: string)`

Reads a `.genny` schema file from disk and parses it, returning an [Sdk](sdkgenny/sdk.md) object.

```lua
local sdk = sdkgenny.parse_file("types.genny")
```

## Types

- [Sdk](sdkgenny/sdk.md) - The root SDK object for code generation and reflection.
- [StructOverlay](sdkgenny/structoverlay.md) - Overlay for reading/writing struct memory by field name.
- [PointerOverlay](sdkgenny/pointeroverlay.md) - Overlay for interacting with pointer-typed memory.
- [Type hierarchy](sdkgenny/types.md) - The full type model (Object, Type, Struct, Variable, etc.).
