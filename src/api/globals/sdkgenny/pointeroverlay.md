# sdkgenny.PointerOverlay

A PointerOverlay is a special Lua type for interacting with pointers. When the pointed to structure is an `sdkgenny.Struct`, it is not much different from an `sdkgenny.StructOverlay`, but when the pointed to type is a primitive type or a pointer, it has different behavior.

## Methods

### `self.index(key: string or number)`

Or in other words, `self.foo` or `self[1]`.

### When `key` is a string
It returns the value of the member at the given key. This will only work if the pointed to type is an `sdkgenny.Struct`.

#### When `key` is a number
It treats the pointer as an array. 

If the pointed to type is a primitive type, it returns the value at the given index.
Essentially, it returns `self:ptr() + (key * self:type():to():size())`, and dereferences the pointer.

If the pointed to type is a pointer, it returns a new `sdkgenny.PointerOverlay` at the given index. Essentially, it returns `self:ptr() + (key * platform_pointer_size)`, and dereferences the pointer.

### `self.newindex(key: string, value: any)`

Or in other words, `self.foo = 123`

When `key` is a string, it sets the value of the member at the given key.

### `self:address()`

Returns the address of the pointer itself.

### `self:ptr()`

Returns the pointed to address.

### `self:type()`

Returns the [sdkgenny.Pointer](types/pointer.md) that this overlay is for.