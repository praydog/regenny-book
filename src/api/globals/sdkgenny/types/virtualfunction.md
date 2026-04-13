# sdkgenny.VirtualFunction

Inherits from [Function](function.md).

A virtual member function. Adds vtable index tracking on top of the base [Function](function.md) methods.

## Methods

### `self:vtable_index()` / `self:vtable_index(index: number)`

Gets or sets the vtable slot index for this virtual function. Returns `self` when setting.
