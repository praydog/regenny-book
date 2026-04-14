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

## Valid metadata tokens

The following metadata tokens are recognized by the overlay system for reading and writing values:

| Token | Description |
|-------|-------------|
| `bool` | Boolean (1 byte, nonzero = true) |
| `u8` | Unsigned 8-bit integer |
| `u16` | Unsigned 16-bit integer |
| `u32` | Unsigned 32-bit integer |
| `u64` | Unsigned 64-bit integer |
| `i8` | Signed 8-bit integer |
| `i16` | Signed 16-bit integer |
| `i32` | Signed 32-bit integer |
| `i64` | Signed 64-bit integer |
| `f32` | 32-bit floating point |
| `f64` | 64-bit floating point |
| `utf8*` | Null-terminated UTF-8 string (pointer or inline) |