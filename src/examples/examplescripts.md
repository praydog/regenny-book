# Example scripts

* [https://github.com/cursey/regenny/blob/master/examples/test.lua](https://github.com/cursey/regenny/blob/master/examples/test.lua)
* [https://github.com/cursey/regenny/pull/10](https://github.com/cursey/regenny/pull/10)

## Basic overlay usage

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

## Standalone usage with sdkgenny.parse_file()

Outside of the ReGenny GUI, you can parse a `.genny` schema file directly and build overlays manually.

```lua
-- Parse the schema
local sdk = sdkgenny.parse_file("types.genny")
local ns = sdk:global_ns()

-- Resolve a type through namespaces
local my_struct = ns:namespace("engine"):struct("Player")

-- Build an overlay at a known address
local player = sdkgenny.StructOverlay(0xDEADBEEF, my_struct)
print(player.health)
print(player.position.x, player.position.y, player.position.z)
```

## Resolving C++ type names to genny types

When working with RTTI strings like `"class engine::Player"`, you can parse them into namespace and type lookups:

```lua
function resolve_type(typename)
    local ns = sdk:global_ns()
    -- Split on "::" and strip "class " prefix
    local pieces = {}
    for piece in typename:gmatch("([^:]+)") do
        piece = piece:gsub("^class ", "")
        table.insert(pieces, piece)
    end

    local result = nil
    for i = 1, #pieces do
        if i < #pieces then
            -- Intermediate pieces are namespaces
            if i == 1 then
                result = ns:find_namespace(pieces[i])
            elseif result then
                result = result:find_namespace(pieces[i])
            end
        else
            -- Last piece is the struct name
            if i == 1 then
                result = ns:find_struct(pieces[i])
            elseif result then
                result = result:find_struct(pieces[i])
            end
        end
    end

    return result
end

local t = resolve_type("class engine::Player")
if t then
    print("Found: " .. t:name()) -- prints "Player"
end
```

## Navigating overlay chains

Nested struct fields and pointer fields are automatically wrapped in `StructOverlay` or `PointerOverlay`. You can chain through them with nil-guards for safety:

```lua
local entity = sdkgenny.StructOverlay(entity_addr, entity_type)

-- Chain through nested structs and pointers
-- Each field access returns an overlay if the field is a struct or pointer
local weapon_manager = entity.weapon_manager
local weapon_storage = weapon_manager and weapon_manager.weapon_storage
local weapon_slots = weapon_storage and weapon_storage.weapon_slots
local weapon = weapon_slots and weapon_slots.active_weapon

if weapon then
    print("Weapon: " .. weapon.name)
end
```

## Pointer arrays and iteration

A common pattern for dynamic arrays in C++ uses `T** data; uint32_t size;`. Access the pointer-to-pointer field, then index and dereference:

```lua
local entity_list = world.entity_list

-- entity_list.data is a T** (pointer to pointer)
-- entity_list.data[i] indexes into the pointer array
-- :deref() follows the pointer to get the actual object
for i = 0, entity_list.size - 1 do
    local entity = entity_list.data[i]:deref()
    if entity then
        print(entity.name)
    end
end
```

## Writing values through overlays

Overlay field writes go directly to the target process memory:

```lua
local entity = regenny:overlay()

-- Write primitive fields
entity.health = 100
entity.armor = 50

-- Write through nested struct overlays
local transform = entity.transform
if transform then
    transform.w.x = new_x
    transform.w.y = new_y
    transform.w.z = new_z
end

-- Write pointer values (set a pointer to a new address)
entity.target = some_address
entity.target = nil  -- null the pointer (sets to 0)
```

## Pointer overlay methods

PointerOverlay provides several ways to interact with pointer-typed fields:

```lua
local ptr = entity.some_pointer

-- Get the address of the pointer itself (where the pointer is stored)
local ptr_storage_addr = ptr:address()

-- Get the address the pointer points to
local pointed_to_addr = ptr:ptr()
-- ptr:p() is an alias for ptr:ptr()

-- Dereference the pointer to get the pointed-to value
local value = ptr:deref()
-- ptr:d() and ptr:dereference() are aliases for ptr:deref()

-- Array-style access through pointer arithmetic
local third_element = ptr[2]  -- *(ptr + 2)
```

## Using process read/write with overlay addresses

For operations the overlay system doesn't handle directly, combine overlay addresses with process read/write methods:

```lua
local process = regenny:process()

-- Read raw memory at an overlay field's address
local some_overlay = regenny:overlay()
local raw_value = process:read_uint64(some_overlay.some_field:address())

-- Pointer arithmetic from overlay addresses
local base = some_overlay:address()
local custom_offset_value = process:read_float(base + 0x30)

-- Write typed values at specific addresses
process:write_float(some_overlay.health:address(), 100.0)
process:write_uint32(some_overlay.flags:address(), 0xFF)
```

## Module enumeration

```lua
local process = regenny:process()
for _, mod in ipairs(process:modules()) do
    print(string.format("%s: 0x%X - 0x%X (size: 0x%X)",
        mod.name, mod.start, mod["end"], mod.size))
end
```
