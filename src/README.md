This is the [regenny](https://github.com/cursey/regenny) wiki.

# What is regenny?
[regenny](https://github.com/cursey/regenny) is a reverse engineering tool to interactively reconstruct structures and generate usable C++ header files. Header file generation is done by the sister project [sdkgenny](https://github.com/cursey/sdkgenny).

For those familiar with other tools, it is similar to ReClass.NET. It is written in C++.

Structures are defined in plaintext `.gennyfile` rather than manually inserting types into a tree view. This allows for the structures to be version controlled, shared between users, and are human readable from a normal text editor.

It has a Lua scripting interface to easily read and manipulate the structures externally without compiling a separate program. This allows for easy testing, quick prototyping, or even writing a full blown tool in Lua. The [Lua bindings](https://github.com/praydog/luagenny) can be used in a separate Lua installation/embedding, they are not tied to regenny.

Precompiled builds can currently be downloaded from the [Actions](https://github.com/cursey/regenny/actions) page.

# What is a gennyfile?
gennyfiles are defined in a C/C++-like syntax. They provide the definitions of the types to be viewed in the GUI, generated, and/or reflected upon by the scripting API.

Structures can be given an explicit size, and fields within the structures can be given explicit offsets without manual padding, and out of order.

Simple C structures can sometimes be directly pasted into a gennyfile.

## Example
```c
// Explicitly defined types.
// Format is type {type_name} {type_size} [[{metadata}]]
// the metadata determines how the type is displayed in the GUI and read by the Lua API.
type int 4 [[i32]]
type float 4 [[f32]]
type char 1

// Forward declared structure.
struct Bar{};

// Simple structure. Size is explicitly defined. It doesn't need to be, though.
struct Foo 0x100 {
    int a @ 4 // Explicit offset
    char* b @ 8 [[utf8*]]
    int c // implicit offset, it's 0x10, inferred from the previous member
    int d @ 0x70
    float e @ 0x80 
    Bar* bar @ 0x90 // Pointer to a forward declared structure
};

struct Bar {
    Foo* parent
    int b
    int c
};
```

![rexample](https://user-images.githubusercontent.com/2909949/187513097-32107759-4f23-4ac8-8985-e2debf2a09ec.png)


### The corresponding generated C++ structure
Foo.hpp
```cpp
#pragma once
struct Bar;
#pragma pack(push, 1)
struct Foo {
    char pad_0[0x4];
    int a; // 0x4
    // Metadata: utf8*
    char* b; // 0x8
    int c; // 0x10
    char pad_14[0x5c];
    int d; // 0x70
    char pad_74[0xc];
    float e; // 0x80
    char pad_84[0xc];
    Bar* bar; // 0x90
    char pad_98[0x68];
}; // Size: 0x100
#pragma pack(pop)
```

Bar.hpp
```cpp
#pragma once
struct Foo;
#pragma pack(push, 1)
struct Bar {
    Foo* parent; // 0x0
    int b; // 0x8
    int c; // 0xc
}; // Size: 0x10
#pragma pack(pop)
```

### Accessing this structure from Lua

Before using this script, the structure in the GUI has to be set to `Foo`, regenny must be attached to a process, and the address of the structure must be set in the GUI.

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

In addition to simply reading/modifying structures, the Lua API allows for more advanced features such as:

* Performing reflection on the structure types
* Reading and writing to the process memory
* Allocating memory in the process
* Parsing another gennyfile separately
* Generating C++ header files from a new gennyfile or SDK instance
