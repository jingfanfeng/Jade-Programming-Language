# Jade Overview

## Introduction

Jade is a programming language that prioritizes **clarity, safety, and developer experience**. It is multi-paradigm, statically typed, and compiled, with a focus on object-oriented programming (OOP) and functional programming (FP).

The language is built on a few guiding principles:

- **Explicitness**: nothing happens “magically.”
- **Composition over inheritance**: flexible code reuse without fragile hierarchies.
- **Errors as values**: error handling is part of normal control flow, not hidden.
- **Readable syntax**: concise but structured, without semicolons or one-line cleverness.

## Typing

Jade uses **static typing** for performance and reliability, while supporting union types and `any` for dynamic behavior when needed.

```swift
func greet(name: string | int) {
    switch name {
        string(s) => println(f"Hello, {s}!"),
        int(n) => println(f"Hello, number {n}!"),
    }
}
```

## Error Handling

Jade treats **errors as values**. Unlike exceptions, which create hidden control flow, Jade makes all error cases explicit in the type system.

```swift
func open_file(name: string) -> FileError!File {...}

func main() {
    var result = open_file("input.txt") catch {
        println("Error while opening file")
    }
}
```

This ensures every possible failure is acknowledged and handled.

## Compile-Time vs Runtime

- Compile-time checks catch all determinable errors before execution.
- Panics outside guarded contexts (loops/ifs) are compile-time errors.
- Runtime panics exist but are always explicit.

## Semicolons

Semicolons are removed to reduce clutter. Jade’s parser uses rules similar to Swift:

- **Assignments are statements**, not expressions.
- A line starting with an expression continues the previous line.

```swift
let query = db.table("users")
    .where("name", "=", "Jane Doe")
    .orderBy("created_at", "desc")
    .limit(10)
```

This keeps code clean without ambiguity.

## Composition vs Inheritance

### Why not inheritance?

- Adds complexity (protected members, hierarchies, polymorphism rules).
- Creates brittle, rigid code structures.
- Makes refactoring harder.

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
    use Image

    static func load(path: string) -> Self {...}
    func save(path: string) {...}

}
```

Adding new behavior is simple:

```swift
struct DrawableImage {
    use Image
    func draw_line(p1: Point, p2: Point) {...}
}
```

Composition gives flexibility without the downsides of deep hierarchies.

## Explicitness vs Verbosity

- No implicit conversions or coercions.
- Upcasting requires explicit syntax.
- Function overloading is explicitly labelled.
- Errors are propagated explicitly.

Yes, this can feel verbose — but Jade favors **clarity over conciseness**.

Note that there is a line to this. Writing out the full name for commonly used **keywords** and **names** that can be **easily understood**, for example, do not bring any benefit for the reader and thus are often shortened. For instance, the standard library uses `Rect` instead of `Rectangle`, and keywords such as `public` and `private` are shortened to `pub` and `priv` because it is easy to understand.

Properties can be argued to be implicit behavior. However, there is a line to this. Jade does not use manual memory management, and that could also be argued as implicit. The line is drawn in terms of **readability** and whether hiding the behavior could lead to issues, and the clarity brought by properties outweighs the drawbacks. For instance, take a look at the following code:

```swift
var rect = Rect::new(0, 0, 50, 50)

loop {
    rect.x += 10
    if rect.right >= screen.height {
        rect.right = screen.height
    }
}
```

This code is **clearer** than the alternative:

```swift
loop {
    rect.set_x(rect.x() + 10)
    // First alternative, builtin getter and setter

    if rect.right() >= screen.height() {
        rect.set_right(screen.height)
    }
}
```

It should still be noted that even though properties are allowed, having **side effects** in getters or having **expensive** function calls in properties are **highly discouraged**.

## Unique Features

- **Row polymorphism**: more flexible type reuse than classic OOP inheritance.
- **Pattern-matching overloads**: select function behavior by structure of input.

## Conclusion

Jade aims to be a **practical language for real-world development** that avoids common pitfalls of modern languages. It balances the safety of static typing with enough flexibility to stay ergonomic, all while emphasizing explicit, maintainable code.
