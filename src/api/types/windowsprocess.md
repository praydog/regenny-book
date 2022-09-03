# WindowsProcess

Windows version of the [Process](process.md) class. Inherits from [Process](process.md).

## Methods

### `self:get_typename(obj: number)`

Returns the RTTI name of the given object at the given address.

### `self:allocate_rwx(address: number, size: number)`

Wrapper over [Process.allocate](process.md#selfallocateaddress-number-size-number-flags-number) that allocates a region of memory with the `PAGE_EXECUTE_READWRITE` protection.

### `self:protect_rwx(address: number, size: number)`

Wrapper over [Process.protect](process.md#selfprotectaddress-number-size-number-flags-number) that changes the protection of a region of memory to `PAGE_EXECUTE_READWRITE`.

### `self:create_remote_thread(address: number, param: number)`
