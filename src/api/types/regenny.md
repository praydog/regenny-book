# ReGenny

This is the class used to access the ReGenny API. It can be access through the global `regenny` variable.

## Methods

### `self:address()`

Returns the current address set in the GUI.


### `self:overlay()`

Constructs and returns an `sdkgenny.StructOverlay` using the address and current structure set in the GUI.

Any member reads or writes will be done from/to the process memory dynamically on each access.

Example:

```lua
local baz = regenny:overlay()
if baz == nil then
    print("Cannot continue test, overlay does not exist.")
    return false
end

if baz:type():name() ~= "Baz" then
    print("Cannot continue test, selected type is not Baz")
    return false
end

print(baz)
print(baz:type())
print(string.format("%x", baz:address()))

print(baz.hello) -- Prints "Hello, world!", which is the value pointed to by baz.hello
baz.foo.a = baz.foo.a + 1 -- Increments baz.foo.a by 1
```

### `self:process()`

Returns the current [Process](process.md). Gets upcasted to a [WindowsProcess](windowsprocess.md) if the process is a Windows process.
