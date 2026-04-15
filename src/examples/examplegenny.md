# Example genny files

* [REFramework](https://github.com/praydog/REFramework/tree/master/reversing)
* [AutomataMP](https://github.com/praydog/AutomataMP/tree/main/reversing)

## Schema patterns

### Basic type definitions

Define primitive types with their sizes and [metadata](../general/metadata.md) for the overlay system:

```c
type uint32_t 4 [[u32]]
type int32_t 4 [[i32]]
type float 4 [[f32]]
type uint8_t 1 [[u8]]
type bool 1 [[u8]]
type char 1
type void 0
```

### Explicit struct sizes

When only a few fields of a struct are known, specify the total struct size after the name. This ensures pointer arithmetic and array strides are correct even with an incomplete field list:

```c
// Full size is 0x1000, but we only know a few fields
struct PlayerController 0x1000 {
    int32_t health @ 0x1A4
    float stamina @ 0x1A8
}

// Empty struct with known size -- used as a placeholder
struct UnknownManager 0x200 {
}
```

### Field offset pinning with @

Use `@ offset` to place a field at a specific byte offset, skipping unknown regions between known fields:

```c
struct Camera {
    Matrix3x4 view_matrix      // starts at offset 0
    Matrix3x4 projection
    float near_z @ 0x68        // pin to offset 0x68, gap before is skipped
    float far_z                // auto-placed right after near_z
    float fov_left
    float fov_right
    uint8_t flags @ 0x88       // jump to offset 0x88
    uint32_t mode @ 0x8C
}
```

### Relative padding with +

Use `+ N` to add N bytes of padding before a field, relative to the previous field's end:

```c
struct PhysicsWorld 0x1000 {
    Vector3 gravity @ 0x170
    float target_delta_time
    float delta_time + 4       // 4 bytes of padding after target_delta_time
}
```

### Forward declarations

Declare a struct with an empty body, then define it fully later. This is common in iterative reverse engineering when types reference each other:

```c
// Forward declarations
struct World {}
struct EntityManager {}

// Full definitions later
struct World 0x8000 {
    EntityManager* entity_manager @ 0x100
}

struct EntityManager 0x500 {
    Entity** entities
    uint32_t capacity
    uint32_t count
}
```

### Namespaces and cross-namespace references

Organize types into namespaces. Reference types across namespaces with dot notation:

```c
namespace engine {
    struct BaseEntity 0x100 {
        int32_t id
        char* name [[utf8*]]
    }
}

namespace game {
    // Inherit from a type in another namespace
    struct Player : engine.BaseEntity 0x200 {
        float health @ 0x1A0
        float armor
    }
}
```

### Multiple inheritance

Structs can inherit from multiple base types, including across namespaces:

```c
struct SharedItem : BaseItem, RefCountObject {
}

struct GenericEntity : ManagedInstance, BaseItem, ComponentContainer 0x100 {
    Matrix3x4 transform
}
```

### Virtual function vtable slots

Document known virtual function slots with their vtable index:

```c
struct BaseItem 0x8 {
    virtual void pad() @ 0
    virtual uint32_t GetHash() @ 1
    virtual char* GetModuleName() @ 2
    virtual uint32_t GetSize() @ 3
    virtual void Destructor() @ 6
}
```

### String pointer metadata

Use `[[utf8*]]` on `char*` fields to tell the overlay system to read them as null-terminated strings:

```c
struct ClassInfo {
    char* class_name @ 0x10 [[utf8*]]
    char* module_name [[utf8*]]
    uint32_t field_count
}
```

For inline character arrays, the same metadata works:

```c
struct FixedString {
    char[64] buffer [[utf8*]]
}
```

### Dynamic array pattern

A common C++ dynamic array layout uses a pointer-to-pointer with capacity and size:

```c
struct EntityList {
    Entity** data
    uint32_t capacity
    uint32_t size
}

struct World 0x8000 {
    EntityList entities @ 0x100
}
```

In Lua, access elements with `world.entities.data[i]:deref()`.

### Nested struct composition

Embed structs by value inside other structs:

```c
struct BoneInfo {
    Matrix3x4 transform
    int32_t parent_index
}

struct BonesHolder {
    BoneInfo* bone_infos
    void* unk1
    void* unk2
    uint32_t bone_count
}

struct MeshObject 0x1000 {
    BonesHolder bones @ 0xE0   // embedded by value at offset 0xE0
}
```


### Template types

Define generic structs with `template <typename T>`. Template parameters act as placeholder types resolved at instantiation:

```c
template <typename T>
struct WeakPtr 0x10 {
    T* data @ 0x8
}

struct Player 0x200 {
    WeakPtr<Entity> entity_ref
}
```

Multiple template parameters work the same way:

```c
template <typename K, typename V>
struct Pair {
    K key
    V value
}
```

Templates can inherit from other structs:

```c
template <typename T>
struct Container : BaseContainer {
    T* items
    int count
}
```

The `+ N` relative padding syntax works inside templates. The delta is preserved across all instantiations:

```c
template <typename T>
struct AlignedValue {
    T value
    int metadata + 4
}
```

When instantiated (e.g. `AlignedValue<float>`), the concrete struct has `float value` followed by 4 bytes of padding before `int metadata`. Cross-namespace types use dot notation in the angle brackets: `Container<engine.Entity>`.