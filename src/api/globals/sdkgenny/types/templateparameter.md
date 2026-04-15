# sdkgenny.TemplateParameter

Inherits from [Type](type.md).

A placeholder type representing a template parameter (e.g. `T` in `template <typename T>`). Has a size of 0 — it is never emitted directly but is substituted for a concrete type when the template is instantiated.

TemplateParameter types are created via `Struct:template_parameter(name)` or automatically by the genny parser when it encounters a `template <typename ...>` declaration.

```lua
local my_struct = ns:struct("Container")
local T = my_struct:template_parameter("T")

-- T can be used as a field type in the template definition
my_struct:field("value"):type(T)
my_struct:field("ptr"):type(T:ptr())
my_struct:field("items"):type(T:array():count(4))
```

TemplateParameter has no additional methods beyond those inherited from [Type](type.md). It supports the standard `ptr()`, `ref()`, and `array()` factory methods for building compound types from the placeholder.

During code generation, template structs emit a `template<typename T>` prefix in C++ headers. The TemplateParameter itself is skipped — the C++ compiler handles substitution when the template is instantiated.
