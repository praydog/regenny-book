# sdkgenny.StructOverlay

An StructOverlay is a special Lua type that allows access to the memory of a structure.
It has index and newindex metamethods that access to the members of the structure
seamlessly, with no magic numbers/offsets, completely externally*.

\* This is not true if using [luagenny](https://github.com/praydog/luagenny) outside of regenny. By default it operates on memory as if it was in the same context as the current process. regenny overrides luagenny's read and write functions to read and write from the target process.

Example assuming Foo is:
```c
struct Foo {
    int a;
    int b;
};
```

```lua
local sdk = regenny:sdk() -- in another application, this would be different.
local overlay = sdkgenny.StructOverlay(0xdeadbeef, sdk:global_ns():find_type("Foo"))
overlay.a = 123
overlay.b = 456
```

## Static methods

`sdkgenny.StructOverlay(address: number, struct: sdkgenny.Struct)`

Creates and returns a new StructOverlay.

## Methods

### `self.index(key: string or number)`

Or in other words, `self.foo` or `self[1]`.

When `key` is a string, it returns the value of the member at the given key. [Structs](types/struct.md) and [Pointers](types/pointer.md) are automatically returned as [StructOverlay](structoverlay.md) or [PointerOverlay](pointeroverlay.md) respectively.

When `key` is a number, it treats the structure as an inlined array and returns a new `StructOverlay` at the given index. Essentially, it returns `self:address() + (key * self:type():size())`.

### `self.newindex(key: string, value: any)`

Or in other words, `self.foo = 123`

When `key` is a string, it sets the value of the member at the given key.

### `self:type()`

Returns the `sdkgenny.Struct` that this overlay is for.