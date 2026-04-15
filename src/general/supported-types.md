# Supported types

## Primitive types

Define primitive types with `type name size [[metadata]]`:

```c
type int 4 [[i32]]
type float 4 [[f32]]
type char 1
type bool 1 [[u8]]
type void 0
```

See [Type metadata](metadata.md) for available metadata values.

## Structs

```c
struct Foo {
    int a
    float b
}

// With explicit size
struct Bar 0x100 {
    int x @ 0x40
}
```

## Classes

```c
class MyClass : Foo {
    int extra_field
}
```

## Enums

```c
enum Direction {
    North = 0,
    South = 1,
    East = 2,
    West = 3,
}
```

## Namespaces

Namespaces can be nested. Types in other namespaces are referenced with dot notation.

```c
namespace engine {
    struct Entity {
        int id
    }

    namespace physics {
        struct RigidBody {
            engine.Entity* owner
        }
    }
}
```

## Pointers

```c
struct Foo {
    int* data
    Foo* next
    char* name [[utf8*]]
}
```

## Arrays

Fixed-size arrays, including multi-dimensional:

```c
struct Foo {
    int[10] values
    float[4][4] matrix
    char[64] name [[utf8*]]
}
```

## Bitfields

```c
struct Date {
    ushort nWeekDay : 3
    ushort nMonthDay : 6
    ushort nMonth : 5
    ushort nYear : 8
}
```

## Function prototypes

```c
struct Foo {
    virtual void Update() @ 0
    virtual int GetHealth() @ 1
    virtual void SetHealth(int value) @ 5
}
```

## Static function prototypes

```c
struct Foo {
    static void Initialize()
    static Foo* GetInstance()
}
```

## Inheritance

Single and multiple inheritance, including across namespaces:

```c
struct Base {
    int id
}

struct Derived : Base {
    float value
}

// Multiple inheritance
struct MultiDerived : Base, OtherBase {
    int extra
}

// Cross-namespace inheritance
struct GameEntity : engine.BaseEntity 0x200 {
    float health
}
```

## Templates

Define template structs with `template <typename T>` before the struct/class keyword:

```c
template <typename T>
struct WeakPtr {
    T* data
    int ref_count
}

// Multiple template parameters
template <typename K, typename V>
struct Pair {
    K key
    V value
}
```

Template parameters can be used anywhere a type is used — as fields, pointers, and arrays:

```c
template <typename T>
struct Container {
    T value
    T* ptr
    T[4] items
    T** indirect
}
```

Use a template by providing concrete type arguments in angle brackets:

```c
struct Player {
    WeakPtr<Entity> target
    Container<float> data
    Pair<int, float> stats
}
```

Cross-namespace type arguments use dot notation:

```c
struct World {
    WeakPtr<engine.Entity> active_entity
    Container<physics.RigidBody> bodies
}
```

Template fields support explicit offsets (`@`) and relative padding (`+`):

```c
template <typename T>
struct TemplatedWrapper 0x100 {
    int header @ 0x0
    T payload @ 0x10
    int footer + 0x8  // 0x8 bytes after the end of T
}
```

> **Note:** Nested template arguments like `Foo<Bar<int>>` are not supported.


## Nesting

Structs, enums and classes can be nested within other structs and classes:

```c
struct Outer {
    struct Inner {
        int x
    }

    Inner value
    Inner* ptr
}
```
