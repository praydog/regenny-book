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


### Template Methods

### `self:template_parameter(name: string)`

Finds or creates a [TemplateParameter](templateparameter.md) child with the given name. Returns the TemplateParameter.

### `self:template_parameters()`

Returns a list of all TemplateParameter children.

### `self:is_template()`

Returns `true` if this struct has any template parameters.

### `self:instantiate({type1, type2, ...})`

Creates a concrete Struct by substituting template parameters with the given types. The new struct is added as a sibling in the same namespace. Returns the instantiated Struct, or `nil` on failure.

### `self:is_template_instance()`

Returns `true` if this struct was created by `instantiate()`.

### `self:template_source()`

Returns the template Struct this was instantiated from, or `nil`.

### Example

```lua
-- Define a template struct with one type parameter
local vec = sdk:struct("Vector")
vec:template_parameter("T")
vec:variable("x"):type("T")
vec:variable("y"):type("T")
vec:variable("z"):type("T")

print(vec:is_template())  -- true

-- Instantiate the template with a concrete type
local float_t = sdk:type("float"):size(4)
local vec_float = vec:instantiate({float_t})

print(vec_float:name())              -- "Vector<float>"
print(vec_float:is_template_instance()) -- true
print(vec_float:template_source())      -- the original Vector struct
```