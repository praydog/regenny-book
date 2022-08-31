# Type metadata

Types and variables can have a metadata attached to them. This metadata is used to determine how the type is displayed in the GUI and read by the Lua API.

For types, the format is `type {type_name} {size} [[{metadata}]]`.

For variables, the format is `{type_name} {variable_name} [[{metadata}]]`.

Example:

```c
type int 4 [[i32]] // i32 metadata, will be displayed as a signed 32-bit integer in the GUI
type float 4 [[f32]] // f32 metadata, will be displayed as a 32-bit float in the GUI
type char 1 // no metadata

struct Foo {
    // will be displayed as a signed 32-bit integer in the GUI, implicit metadata
    int a;
    // utf8* metadata, will be displayed as a utf8 string in the GUI
    // must be explicitly specified, as the type is a pointer to 1 byte
    char* b [[utf8*]]
    // f32 metadata, will be displayed as a 32-bit float in the GUI
    float c
    // metadata does not need to be specified for this pointer
    int* d
};
```