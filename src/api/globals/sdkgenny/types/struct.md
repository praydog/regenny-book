# sdkgenny.Struct

Inherits from [Type](type.md).

A C struct type. This is the central type for declaring fields, nested types, and member functions.

## Methods

### `self:size()` / `self:size(bytes: number)`

Gets or sets the explicit byte size of this struct. A size of `0` means the size is auto-calculated from the fields. Returns `self` when setting.

### `self:parent(parent_struct: sdkgenny.Struct)`

Adds a base/parent struct for inheritance. Returns `self` for chaining.

### `self:parents()`

Returns the list of direct parent structs.

### `self:variable(name: string)`

Finds or creates a member [Variable](variable.md).

### `self:constant(name: string)`

Finds or creates a named [Constant](constant.md).

### `self:struct(name: string)`

Finds or creates a nested [Struct](struct.md).

### `self:class(name: string)`

Finds or creates a nested [Class](class.md).

### `self:enum(name: string)`

Finds or creates a nested [Enum](enum.md).

### `self:enum_class(name: string)`

Finds or creates a nested [EnumClass](enumclass.md).

### `self:function(name: string)`

Finds or creates a member [Function](function.md).

### `self:virtual_function(name: string)`

Finds or creates a [VirtualFunction](virtualfunction.md).

### `self:static_function(name: string)`

Finds or creates a [StaticFunction](staticfunction.md).

### `self:bitfield(byte_offset: number)`

Returns a table mapping bit offsets to [Variable](variable.md) objects for all bitfield members at the given byte offset.

### `self:find_in_parents(typename: string, name: string)`

Searches parent structs recursively for a child of the given type with the given name. Returns `nil` if not found.

Like [Object](object.md), type-specific shortcut methods are also available. For example:

- `self:find_variable_in_parents(name)` - equivalent to `self:find_in_parents("variable", name)`
- `self:find_struct_in_parents(name)` - equivalent to `self:find_in_parents("struct", name)`

The same pattern applies for all type names.
