# sdkgenny.Object

The base type that all sdkgenny types inherit from. Provides name access, metadata, and a polymorphic type dispatch system.

## Methods

### `self:name()` / `self:name(new_name: string)`

Gets or sets the object's name. Returns `self` when setting.

### `self:metadata()`

Returns the metadata string array for this object (e.g. `{"i32"}`, `{"f32"}`, `{"utf8*"}`).

### `self:is_a(typename: string)`

Returns `true` if this object is of the given type. Valid type names: `typename`, `type`, `generic_type`, `struct`, `class`, `enum`, `enum_class`, `namespace`, `reference`, `pointer`, `variable`, `function`, `virtual_function`, `static_function`, `array`, `parameter`, `constant`, `template_parameter`.

### `self:as(typename: string)`

Casts this object to the given type. Returns the casted object, or `nil` if the cast is invalid. Takes the same type names as `is_a`.

### `self:find(typename: string, name: string)`

Searches the direct children of this object for a child of the given type with the given name. Returns the child, or `nil` if not found.

### `self:find_in_owners(typename: string, name: string, include_self: boolean)`

Searches upward through ancestor owners for an object of the given type with the given name. If `include_self` is `true`, the search includes this object.

### `self:has_any(typename: string)`

Returns `true` if this object has any direct children of the given type.

### `self:has_any_in_children(typename: string)`

Returns `true` if any descendant (recursively) is of the given type.

### `self:owner(typename: string)`

Returns the immediate owner of the given type, or `nil`.

### `self:topmost_owner(typename: string)`

Returns the topmost (outermost) owner of the given type, or `nil`.

### `self:get_all(typename: string)`

Returns all direct children of the given type as a list.

### `self:get_comment()`

Returns the comment string for this object.

### `self:set_comment(str: string)`

Sets the comment for this object.

## Type shortcut methods

For every type name listed above, convenience methods are generated that eliminate the string argument. For example, for the type name `struct`:

- `self:is_struct()` - equivalent to `self:is_a("struct")`
- `self:as_struct()` - equivalent to `self:as("struct")`
- `self:find_struct(name)` - equivalent to `self:find("struct", name)`
- `self:find_struct_in_owners(name, include_self)` - equivalent to `self:find_in_owners("struct", name, include_self)`
- `self:has_any_struct()` - equivalent to `self:has_any("struct")`
- `self:has_any_struct_in_children()` - equivalent to `self:has_any_in_children("struct")`
- `self:owner_struct()` - equivalent to `self:owner("struct")`
- `self:topmost_owner_struct()` - equivalent to `self:topmost_owner("struct")`
- `self:get_all_struct()` - equivalent to `self:get_all("struct")`

The same pattern applies to all type names: `typename`, `type`, `generic_type`, `struct`, `class`, `enum`, `enum_class`, `namespace`, `reference`, `pointer`, `variable`, `function`, `virtual_function`, `static_function`, `array`, `parameter`, `constant`, `template_parameter`.

```lua
local obj = sdk:global_ns():find_type("Foo")
if obj:is_struct() then
    local s = obj:as_struct()
    local vars = s:get_all_variable()
end
```
