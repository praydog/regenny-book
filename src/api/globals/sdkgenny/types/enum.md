# sdkgenny.Enum

Inherits from [Type](type.md).

An enumeration type with named integer values.

## Methods

### `self:value(name: string, val: number)`

Adds an enumerator with the given name and integer value.

### `self:values()`

Returns all enumerators as a list of `{name, value}` pairs.

```lua
local e = ns:enum("Color")
e:value("Red", 0)
e:value("Green", 1)
e:value("Blue", 2)

for _, pair in ipairs(e:values()) do
    print(pair[1], pair[2]) -- name, value
end
```

### `self:type()` / `self:type(underlying_type: sdkgenny.Type)`

Gets or sets the underlying integer type of this enum. Returns `self` when setting.
