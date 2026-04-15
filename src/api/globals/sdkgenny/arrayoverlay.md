# sdkgenny.ArrayOverlay

An ArrayOverlay is a special Lua type for interacting with fixed-size array fields. It is returned automatically when accessing an Array-typed field through a [StructOverlay](structoverlay.md).

```lua
local arr = overlay.items  -- Array-typed field returns an ArrayOverlay
print(#arr)        -- element count
print(arr[0])      -- first element (0-based)
arr[2] = 42        -- write third element
```

## Methods

### `self.index(n: number)`

Or in other words, `self[0]`.

Returns the value at 0-based index `n`. If the element type is an `sdkgenny.Struct`, returns a [StructOverlay](structoverlay.md). If the element type is an `sdkgenny.Pointer`, returns a [PointerOverlay](pointeroverlay.md). Otherwise returns the primitive value directly.

Returns `nil` for out-of-bounds indices (when `n < 0` or `n >= count`).

### `self.newindex(n: number, value: any)`

Or in other words, `self[2] = 42`.

Writes `value` at 0-based index `n`. No-op for out-of-bounds indices.

### `#self` / `__len`

Returns the number of elements in the array.
