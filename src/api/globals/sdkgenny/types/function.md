# sdkgenny.Function

Inherits from [Object](object.md).

A member function declaration with return type, parameters, and an optional body.

## Methods

### `self:returns()` / `self:returns(type: sdkgenny.Type)`

Gets or sets the return type. Returns `self` when setting.

### `self:procedure()` / `self:procedure(body: string)`

Gets or sets the function body as a code string. Returns `self` when setting.

### `self:dependencies()`

Returns the list of types this function explicitly depends on.

### `self:depends_on(type: sdkgenny.Type)`

Declares that this function depends on the given type. Returns `self` for chaining.

### `self:defined()` / `self:defined(flag: boolean)`

Gets or sets whether this function has an implementation body. Returns `self` when setting.
