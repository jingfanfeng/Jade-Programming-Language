# Jade Programming Language

Jade is a statically typed, compiled, multi-paradigm programming language designed to **make developers’ lives easier without sacrificing clarity or safety**.

It blends **object-oriented programming (OOP)** and **functional programming (FP)**, borrowing the best ideas while avoiding common pitfalls.

The core philosophy:

- **Explicit > Implicit**
- **Composition > Inheritance**
- **Clarity > Cleverness**

---

## Features at a Glance

- ✅ Statically typed with union types for flexibility
- ✅ Errors as values, not exceptions
- ✅ Composition and interfaces instead of inheritance
- ✅ Readable syntax without semicolons
- ✅ Compile-time checks wherever possible

```swift
func main() {
    let result = open_file("input.txt")
    switch result {
        Ok(file)   => println("Opened file!"),
        Err(error) => println("Error: " + error.to_string())
    }
}
```

## Why Jade?

Jade takes the **middle path** between strict, verbose languages and overly permissive ones:

- From Java & C++: structure, performance, static guarantees
- From Rust & Swift: safety and modern ergonomics
- From Go: simplicity and readability

But Jade avoids common pitfalls: no fragile inheritance hierarchies, no hidden exception flow, and no semicolon clutter.

## Documentation

- [Overview & Philosophy](docs/overview.md)

- Getting Started
  (planned)

- Syntax Guide
  (planned)

- Error Handling
  (planned)

- Advanced Features
  (planned)

## Status

Jade is currently in the **design stage**. Syntax and philosophy are being documented before implementation. Contributions, feedback, and design discussions are welcome.
