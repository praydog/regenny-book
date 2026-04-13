# sdkgenny.Type

Inherits from [Typename](typename.md).

A named type with a byte size. Provides factory methods for creating derived reference, pointer, and array types.

## Methods

### `self:size()` / `self:size(bytes: number)`

Gets or sets the byte size of this type. Returns `self` when setting.

### `self:ref()`

Creates or retrieves a [Reference](reference.md) type wrapping this type (i.e. `T&`).

### `self:ptr()`

Creates or retrieves a [Pointer](pointer.md) type pointing to this type (i.e. `T*`).

### `self:array()`

Creates or retrieves an [Array](array.md) type for this element type.
