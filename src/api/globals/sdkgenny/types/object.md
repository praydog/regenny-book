# sdkgenny.Object

All types inherit from this.

Most methods with an asterisk \* take many different strings, e.g.
```lua
object:is_struct() -- returns true if the object is a struct
object:is_variable() -- returns true if the object is a variable
object:is_function() -- returns true if the object is a function
```

## Methods

`self:is_*()`

Returns true if the object is of the given type.

`self:as_*()`

Returns the object as the given type, or nil if it is not of the given type.

`self:find_*()`

Searches the children of the object for the given type. Returns `nil` if not found.