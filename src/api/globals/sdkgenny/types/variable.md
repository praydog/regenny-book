# sdkgenny.Variable

Inherits from [Object](object.md).

A struct/class member variable with type, offset, and optional bitfield information.

## Methods

### `self:type()` / `self:type(type: sdkgenny.Type)` / `self:type(typename: string)`

Gets or sets the variable's type. Accepts either a [Type](type.md) object or a type name string. Returns `self` when setting.

### `self:offset()` / `self:offset(bytes: number)`

Gets or sets the byte offset of this variable within its parent struct. Returns `self` when setting.

### `self:append()`

Sets this variable's offset to the end of the last variable in the parent struct, effectively auto-packing it after the previous field. Returns `self` for chaining.

### `self:end()`

Returns the byte offset immediately past the end of this variable (`offset + type size`).

### `self:bit_size()` / `self:bit_size(bits: number)`

Gets or sets the bit-size for bitfield variables. Returns `self` when setting.

### `self:bit_offset()` / `self:bit_offset(bits: number)`

Gets or sets the bit-offset within the storage unit for bitfield variables. Returns `self` when setting.

### `self:is_bitfield()`

Returns `true` if this variable is a bitfield.

### `self:bit_append()`

Appends this variable as the next bitfield after the previous bitfield in the same storage unit. Returns `self` for chaining.


### `self:delta()` / `self:delta(value: number)`

Gets or sets the `+ N` delta value for this variable. In genny schema, this corresponds to the `+ delta` syntax for relative padding. When called with no arguments, returns the current delta as a number. When called with a number, sets the delta and returns `self`.

### `self:has_delta()`

Returns `true` if a delta was explicitly set on this variable. This distinguishes `+ 0` (an intentional zero delta) from no delta at all.
