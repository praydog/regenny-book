# Example scripts

* [https://github.com/cursey/regenny/blob/master/examples/test.lua](https://github.com/cursey/regenny/blob/master/examples/test.lua)
* [https://github.com/cursey/regenny/pull/10](https://github.com/cursey/regenny/pull/10)

```lua
-- An overlay is a special Lua type that allows access to the memory of a structure.
-- It has index and newindex metamethods that access to the members of the structure
-- seamlessly, with no magic numbers/offsets, completely externally.
local foo = regenny:overlay()
if not foo or foo:type():name() ~= "Foo" then
    print("Overlay is not a Foo, cannot use this script.")
    return
end

-- Dynamically read the structure memory from the target process.
print(foo.a) -- prints the value of foo.a
print(foo.b) -- prints the value of foo.b, will be a string

-- Setter demonstrations
-- These actually set the values within the target process's structure behind the scenes.
foo.c = 0x12345 -- sets foo.c to 0x12345
foo.e = 3.14159 -- sets foo.e to 3.14159

local bar = foo.bar -- gets the value of foo.bar

-- Because pointers can be null.
if bar ~= nil then
    bar.b = 0x1337 -- sets bar.b to 0x1337
    bar.c = bar.b + 0x100 -- sets bar.c to bar.b + 0x100

    print(bar.parent:ptr() == foo:address()) -- prints true if bar.parent is equal to foo
    if bar.parent:ptr() ~= foo:address() then
        -- sets bar.parent to foo
        -- pointers can be explicitly written to with a raw integer value
        bar.parent = foo:address()
    end
end
```