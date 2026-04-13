# sdkgenny.Namespace

Inherits from [Typename](typename.md).

A C++ namespace that can contain types, structs, enums, and nested namespaces.

## Methods

### `self:type(name: string)`

Finds or creates a named [Type](type.md) in this namespace.

### `self:generic_type(name: string)`

Finds or creates a [GenericType](generictype.md) in this namespace.

### `self:struct(name: string)`

Finds or creates a [Struct](struct.md) in this namespace.

### `self:enum(name: string)`

Finds or creates an [Enum](enum.md) in this namespace.

### `self:enum_class(name: string)`

Finds or creates an [EnumClass](enumclass.md) in this namespace.

### `self:namespace(name: string)`

Finds or creates a nested [Namespace](namespace.md) in this namespace.
