# Tutorial

This tutorial assumes that you already have an understanding of programming concepts. It will simply show you how each feature is written and why it is useful.

## Hello world

```swift
func main() {
    println("Hello, world!")
}
```

## Primitive Data Types

Jade doesn't have a pure concept of primitive data types. While there are basic data types, they can have methods and properties like regular class types. Integer types are:

| Type   | C++ Equivalent        | Description                    |
| :----- | :-------------------- | :----------------------------- |
| `i8`   | `int8_t`              | signed 8-bit integer           |
| `u8`   | `uint8_t`             | unsigned 8-bit integer         |
| `i16`  | `int16_t`             | signed 16-bit integer          |
| `u16`  | `uint16_t`            | unsigned 16-bit integer        |
| `i32`  | `int32_t`             | signed 32-bit integer          |
| `u32`  | `uint32_t`            | unsigned 32-bit integer        |
| `i64`  | `int64_t`             | signed 64-bit integer          |
| `u64`  | `uint64_t`            | unsigned 64-bit integer        |
| `i128` | `__int128`            | signed 128-bit integer         |
| `u128` | `unsigned __int128`   | unsigned 128-bit integer       |
| `int`  | `intptr_t`            | signed pointer sized integer   |
| `uint` | `uintptr_t`, `size_t` | unsigned pointer sized integer |

Floating point types:

| Type   | C++ Equivalent | Description                   |
| :----- | :------------- | :---------------------------- |
| `f16`  | `_Float16`     | 16-bit floating point number  |
| `f32`  | `float`        | 32-bit floating point number  |
| `f64`  | `double`       | 64-bit floating point number  |
| `f80`  | `long double`  | 80-bit floating point number  |
| `f128` | `_Float128`    | 128-bit floating point number |

Other types:

| Type            | C++ Equivalent   | Description                                                 |
| :-------------- | :--------------- | :---------------------------------------------------------- |
| `string`        | `std::u8_string` | stores a utf-8 encoded string                               |
| `bool`          | `bool`           | boolean value: `true` or `false`                            |
| `void`          | `void`           | indicates no value                                          |
| `anytype`       | `auto`           | used in function parameters to accept any type              |
| `any`           | `std::any`       | can be any type                                             |
| `noreturn`      | (none)           | used for `continue`, `break`, `panic` to indicate no return |
| `type`          | (none)           | the type of types                                           |
| `generic_int`   | (none)           | indicates an integer literal                                |
| `generic_float` | (none)           | indicates a float literal                                   |

Note that integers can be arbitrarily sized in Jade. These integer types must be prefixed with an `i` or `u`, and the byte size can span from 1 to 65535.

## Declaring variables

There are three ways of declaring variables.

Use `var` to declare a mutable variable. It can be followed by a type annotation, though it is recommended to use type inference when possible.

```swift
var x: u8 = 1
var y = 2 // by default it is of type 'i32'
var z = function_returning_u32() // automatically of type u32
```

Use `let` to declare an immutable variable. Similarly to `var`, it can be followed by an optional type annotation.

```swift
let x: u8 = 1

// x += 1 -- uncommenting this line will result in a compilation error
```

Use the `:=` operator to declare a mutable variable. It doesn't accept type annotations.

```go
x := 1 // type 'i32'
y := function_returning_u32() // y is of type 'u32'
```

## If statements

`if` statements follow the structure `if bool_exp {conditional code}`. Note that parentheses are not required, but curly braces are. They are used to execute code conditionally.

```swift
var x = 1
if x < 0 {
    println("x is negative")
}
```

`else if` statements follow the same syntax. They are used to conditionally execute code if the previous `if` or `else if` statement didn't execute.

```swift
var x = 1
if x < 0 {
    println("x is negative")
} else if x > 0 {
    println("x is positive")
}
```

`else` statements execute if the previous `if` or `else if` statement didn't execute.

```swift
var x = 1
if x < 0 {
    println("x is negative")
} else if x > 0 {
    println("x is positive")
} else {
    println("x is 0")
}
```

## Loops

### Loop keyword

The most basic loop is the `loop` keyword. It loops indefinitely. You must use `break` to prevent an infinite loop.

```swift
let counter = 0
loop {
    if counter == 8 {
        break
    }
    println("This statement is printed 8 times!")
    counter += 1
}
```

Use the `while` loop to keep executing a series of statements until a condition becomes `false`.

```swift
let counter = 0
while counter < 8 {
    println("This statement is printed 8 times!")
    counter += 1
}
```

`while` loops can optionally have a continue expression. This is executed at the end of every loop cycle.

```swift
let counter = 0
while counter < 8 : counter += 1 {
    println("This statement is printed 8 times!")
}
```

`for` loops in Jade are only range-based; there are no c-style for loops. It can be iterated over any iterator (any type implementing the `Iterator` trait)

Note: the syntax here creates a range from 0 inclusive to 8 exclusive. See **ranges** for more info.

```swift
for counter in 0..8 {
    println("This statement is printed 8 times!")
}
```

Ranged integers
constraints
