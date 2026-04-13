# sdkgenny.Pointer

Inherits from [Reference](reference.md).

A pointer type (`T*`). Inherits the `to()` method from [Reference](reference.md) and has no additional methods of its own.

Pointer types are typically created via the `Type:ptr()` factory method rather than directly.

```lua
local int_type = ns:type("int"):size(4)
local int_ptr = int_type:ptr() -- creates int*
print(int_ptr:to():name())     -- prints "int"
```
