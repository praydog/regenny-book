# Address
ReGenny parses addresses in a few different ways. Here you can find some examples.

For these examples, I'm going to be using numbers in the hexidecimal format (base 16), but decimal numbers (base 10) are also supported.

## Absolute addresses
Inputting an absolute address will display whatever is at that address in the current virtual address space.

For example, on Windows if your process is based at `0x180000000`, then you can input the address `0x180000000` to see the PE (MZ) header and `0x180001000` to see the start of code (or whatever else is after the PE header, assuming the PE header is `0x1000` bytes in size).

## Relative addresses
Relative addresses can also be used. The syntax looks something like this: `<module_name>+offset->offset->offset`.

Module names can contain full paths or partial paths, as long as it ends with the input path.
For example, say we want to get the base address of a module loaded as `C:\Windows\System32\kernel32.dll`, we could do one of the following:
* `<kernel32.dll>+0x0` (Partial ending)
* `<System32\kernel32.dll>` (Partial ending)
* `<C:\Windows\System32\kernel32.dll>+0x0` (Full path)

On Windows, Addresses are relative to a loaded DLL's (Dynamic-Link Library) name or EXE (Executable) name of the attached app (basically, any loaded module, including itself).

For example, to get the start of code, after the PE header, you can enter something like:
`<app.exe>+0x1000`.

It also supports dereferencing (denoted by `->`), like so: `<app.exe>+0x5000->0x24`. This will add `0x5000` to `app.exe`'s base address, dereference the result and then read out bytes starting from `0x24` after the resulting dereference.