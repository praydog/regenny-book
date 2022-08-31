# Process

Class referring to the currently attached process in regenny.

## Methods

### `self:protect(address: number, size: number, flags: number)`

Modifies the protection of a region of memory in the process.

On Windows, the `flags` are the same as the `flNewProtect` flags in [VirtualProtectEx](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotectex).

Returns the old protection flags of the region.

### `self:allocate(address: number, size: number, flags: number)`

Allocates a region of memory in the process.

`address` is optional, set it to 0 to let the OS choose the address.

On Windows, the `flags` are the same as the `flProtect` flags in [VirtualAllocEx](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex).

### `self:get_module_within(address: number)`

Returns the [ProcessModule](processmodule.md) that contains the given address.

### `self:get_module(name: string)`

Returns the [ProcessModule](processmodule.md) with the given name.

### `self:modules()`

Returns a list of [ProcessModule](processmodule.md)s in the process.

### `self:allocations()`

Returns a list of [ProcessAllocation](processallocation.md)s in the process.

### `self:read_int8(address: number)`

Reads a `int8_t` from the process memory at the given address.

### `self:read_int16(address: number)`

Reads a `int16_t` from the process memory at the given address.

### `self:read_int32(address: number)`

Reads a `int32_t` from the process memory at the given address.

### `self:read_int64(address: number)`

Reads a `int64_t` from the process memory at the given address.

### `self:read_uint8(address: number)`

Reads a `uint8_t` from the process memory at the given address.

### `self:read_uint16(address: number)`

Reads a `uint16_t` from the process memory at the given address.

### `self:read_uint32(address: number)`

Reads a `uint32_t` from the process memory at the given address.

### `self:read_uint64(address: number)`

Reads a `uint64_t` from the process memory at the given address.

### `self:read_float(address: number)`

Reads a `float` from the process memory at the given address.

### `self:read_double(address: number)`

Reads a `double` from the process memory at the given address.

### `self:read_string(address: number)`

Reads a `char*` from the process memory at the given address.

Size is automatically deduced using a `strlen`-like algorithm.

### `self:write_int8(address: number, value: number)`

Writes a `int8_t` to the process memory at the given address.

### `self:write_int16(address: number, value: number)`

Writes a `int16_t` to the process memory at the given address.

### `self:write_int32(address: number, value: number)`

Writes a `int32_t` to the process memory at the given address.

### `self:write_int64(address: number, value: number)`

Writes a `int64_t` to the process memory at the given address.

### `self:write_uint8(address: number, value: number)`

Writes a `uint8_t` to the process memory at the given address.

### `self:write_uint16(address: number, value: number)`

Writes a `uint16_t` to the process memory at the given address.

### `self:write_uint32(address: number, value: number)`

Writes a `uint32_t` to the process memory at the given address.

### `self:write_uint64(address: number, value: number)`

Writes a `uint64_t` to the process memory at the given address.

### `self:write_float(address: number, value: number)`

Writes a `float` to the process memory at the given address.

### `self:write_double(address: number, value: number)`

Writes a `double` to the process memory at the given address.