# Jade Programming Language

## Table of Contents

- Introduction

- Philosophy

- Typing

- Error Handling

- Compile-Time vs Runtime

- Semicolons

- Composition over Inheritance

- Explicitness vs Verbosity

- Unusual Features

## Introduction

Jade is a statically typed, compiled, multi-paradigm language designed to make the developer’s life easier without sacrificing clarity or safety.

It blends object-oriented programming (OOP) and functional programming (FP), borrowing the best ideas while avoiding common pitfalls.

The core philosophy: explicit > implicit, simple > clever, composition > inheritance.

## Philosophy

Jade’s design balances between competing language design choices:

- Static typing with limited dynamic escape hatches

- Errors as values instead of exceptions

- Composition instead of inheritance

- Readable syntax without semicolons

- Explicit conversions to avoid hidden magic

## Typing

Jade uses static typing for performance and safety, with support for `any` and union types when dynamic flexibility is needed.

```swift
func greet(name: string | int) {
    switch name {
        string(s) => println("String given"),
        int(i) => println("Int given"),
    }
}
```

## Error Handling

Errors are values, not exceptions. This forces programmers to handle failure paths explicitly.

### Why Not Exceptions?

Exceptions create hidden control flow. For example:

```java
// Java-style exception
try {
    file = open("input.txt")
} catch FileNotFound {
    println("File not found")
}
```

The problem: you can’t tell by reading the function signature which errors might occur.

Errors as Values

In Jade, error handling looks like this:

```swift
func open_file(name: string) -> FileError!File {...}

func main() {
    var result = open_file("input.txt") catch |err| {
        println(f"Error: ${err.message}")
    }
}
```

This is explicit and composable, like Rust’s Result type but with syntactic sugar to reduce boilerplate.

## Compile-Time vs Runtime

All errors detectable at compile-time are caught before execution.

Panics outside guarded contexts are compile-time errors.

Runtime panics exist but are always explicit.

## Semicolons

Semicolons are omitted to reduce clutter. Jade uses parsing rules inspired by Swift:

```javascript
var query = db
  .table("users")
  .where("name", "=", "Jane Doe")
  .orWhere("email", "=", "jane@example.com")
  .orderBy("created_at", "desc")
  .limit(10);
```

Assignments are statements, not expressions, preventing tricky one-liners like in Python:

```python
print(arr[i = 5]) # Bad style Jade disallows
```

This keeps code explicit and readable.

## Composition over Inheritance

### Why Not Inheritance?

- Increases complexity (protected members, fragile hierarchies)

- Makes refactoring painful

- Encourages rigid designs

### Composition Example

```swift
struct Image {
    pixels: [][]u32

    func resize() {...}
    func flip_horizontal() {...}
}

interface ImageFile {
    static func load(path: string) -> Self
    func save(path: string)
}

struct PngImage: ImageFile {
    use Image {} // mix in Image methods

    static func load(path: string) -> Self {...}
    func save(path: string) {...}
}
```

Want a DrawableImage? Just compose:

```swift
struct DrawableImage {
    use Image{}

    func draw_line(p1: Point, p2: Point) {...}
}
```

No brittle hierarchies, just flexible reuse.

## Explicitness vs Verbosity

- No implicit conversions between types

- Upcasting is explicit

- Properties require explicit getters/setters

Yes, it’s a little verbose — but it prevents hidden bugs.

## Unusual Features

Row polymorphism for flexible type reuse

Pattern-matching overloads

(Optional: expand if you want to highlight Jade’s uniqueness.)

## Final Thoughts

This README is both an overview and a design rationale. For tutorials, syntax guides, and detailed examples, see the /docs folder (future).
