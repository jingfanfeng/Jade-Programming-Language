# Jade Programming Language

## Introduction

This programming language hopes to make the life of the developer easy by taking the middle path between lots of different language types, each with their pros and cons.

This README discusses the core aspects of Jade's design philosophy and outlines the reasoning behind it for the complex decisions.

## Programming Paradigm

Jade is a multi-paradigm, statically typed, and compiled language, meaning that it supports multiple programming paradigms. However, it primarily focuses on object-oriented programming (OOP) and functional programming (FP).

## Static vs dynamic typing

Jade utilizes static typing with support for `any` and union types when necessary for dynamic behavior. Static typing allows for more error detection at compile time and better performance.

## Error handling system

The programming language utilizes errors as values with syntactic sugar that'll make it feel like a checked exception in many ways.

### Why not regular exceptions?

Most modern programming languages utilize exceptions for error handling.

```
func open_file(file_name: string) -> File {...} // Builtin function

func main() {
    var file: File = undefined
    try {
        file = open_file("input.txt")
    } except FileNotFoundException {
        println("Error: file not found")
    }
}
```

The main issues with exceptions are:

#### Complex control flow

Recall that a core philosophy of Jade is explicitness. Nothing happens without code being written to explicitly do it. Exceptions are a huge violation to this rule. They bubble up the call stack automatically until it reaches a catch block, which is not explicit at all.

#### Accounting for all errors

An important aspect of writing a robust program is ensuring that every error has been addressed. With exceptions, it is impossible for you to know all possible errors - it is still incredibly difficult with documentation. With errors as values, you are forced to handle every possibility. It is verbose, but it allows for applications that handle errors appropriately at the right times.

### Checked exceptions

Java and Swift have a concept known as checked exceptions. Checked exceptions require an explicit annotation in the function signature that the function will be throwing an exception and must be explicitly handled or propagated. However, in a language that is also focused on functional programming, checked exceptions clutter up function signatures, which become problematic when generics get involved (insert example).

### Errors as values

See the tutorial for how errors as values work in Jade.

## Compile-time vs runtime errors

All errors that can be caught at compile time will be caught at compile time. Any `panic` that isn't placed in an if statement or loop will be addressed at compile time.

## Use of semicolons

The use of semicolons is a very controversial issue among programming languages.

### Why Not Use Semicolons?

Semicolons lead to visual clutter and less concise syntax. We want Jade to be easy to read. In addition, part of Jade's design philosophy is to give the developer some but not too much freedom, and being able to format the code however you want is too much freedom for the developer. There is always the temptation for super terse one-line statements that make the program hard to read.

### How do we address the lack of semicolons?

We will now address how Jade resolves issues that are commonly associated with non-semicolon programming languages.

We will look at a snippet of Jade code for a database query builder as an example:

```
var query = db.table("users")
    .where("name", "=", "Jane Doe")
    .orWhere("email", "=", "jane@example.com")
    .orderBy("created_at", "desc")
    .limit(10)
```

In a semicolon language, one could simply terminate the last line with a semicolon, and there would be no ambiguity in the statement. However, in non-semicolon languages, this would create ambiguity in what the expression means. The code snippet above would normally get interpreted as:

```
var query = db.table("users");
    .where("name", "=", "Jane Doe");
    .orWhere("email", "=", "jane@example.com");
    .orderBy("created_at", "desc");
    .limit(10);
```

Because whitespace is ignored, the lexer can't tell the difference between the two.

There are two ways to solve this parsing dilemma without semicolons:

### Use Indentation

An option that has been considered is to simply use the fact that the next line is indented as a cue to continue the previous line onto the next line. After all, we humans primarily rely on indentation for reading these expressions. However, there is a drawback to this approach. Copying code sections from one area to another with different indentation levels is very inconvenient. A code formatter can't figure out the indentation for the programmer because it relies on that information to parse the code.

### Differentiating statements and expressions

Most programming languages do not differentiate statements and expressions. Statements do not return a value and only execute an action. Expressions may execute an action and evaluate to a value.

#### Variable declarations as statements

In most programming languages, variable assignments return the value that was assigned to the variable. For instance, take a look at this Python code snippet:

```
x = y = 2
```

Or this one:

```
i = 10
arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
print(arr[i = 5]);
```

In differentiating statements and expressions, the variable assignment operation can't return a value.

Because Jade favors explicitness in programming, the second example is considered bad programming. The programmer should separate these statements to make it easier to identify what the program is doing.

As for the first example, special parsing logic will be added to allow for it without the returning of values in variable assignments.

#### How does this help?

The parsing logic takes inspiration from Swift. In Swift, a line with an expression is automatically assumed to be part of the previous line. A line with a statement indicates a new statement being executed. This provides a happy compromise between the flexibility in semicolon languages and the cleanliness of non-semicolon languages.

## Composition vs Inheritance

This discussion circulates around the debate between inheritance and composition and ultimately outlines why Jade does not support inheritance, instead providing several rich features for composition and interfaces.

### Inheritance

Many programming languages support inheritance, the ability to extend a parent class. It is often credited for promoting code reuse.

However, adding inheritance would lead to a few issues:

#### Language complexity

Jade is focused on being a relatively simple language. Adding inheritance would also add on protected members and polymorphism for base classes, bloating up the complexity of the language. Navigating inheritance hierarchies is quite complex and hard to understand and navigate, especially with it being abused very often to create very deep inheritance hierarchies.

#### Inflexible Structure and Maintenance Challenges

Inheritance is a very inflexible relationship between the parent and child class. The child class must have all of the parent class's fields, methods, and properties. While this may seem fine at first, maintaining and refactoring code becomes a major challenge. The need for different functionality in the child class can break the entire inheritance hierarchy, making it incredibly difficult to refactor code.

Let's take a look at this example Jade code using inheritance:

```
// Abstract structs don't exist in Jade, but inheritance relies on it, so it is here in this example.
abstract struct Image {
    pixels: [][]u32

    pub func flipHorizontal() {...}
    pub func flipVertical() {...}
    pub func resize() {...}

    pub func operator[x, y: int] -> &u32 {
        return &pixels[y][x]
    }

    abstract static func load(file_name: string) -> Self
    abstract func save(file_name: string)
}

struct PngImage: Image {
    override static func load(file_name: string) -> Self {...}

    override func save(file_name: string) -> Self {...}
}
```

Now let's say the user decides to add a `DrawableImage` class.

```
struct DrawableImage {}
```

This code results in an error: the load and save methods aren't implemented. In a typical inheritance programming language, one has two options:

1. Refactor the code, adding a `FileImage` class that inherits from the `Image` class and names the load and save methods. This adds extra complexity to the inheritance hierarchy.
2. Panic for the `load` and `save` methods. Unfortunately, deferring compile-time checks to runtime checks unnecessarily like this is heavily against Jade's design philosophy.

### Composition Perks

Composition is a much more flexible structure - a class contains other classes that have the desired functionality. Let's revisit the previous example:

```
struct Image {
    pixels: [][]u32

    pub func flipHorizontal() {...}
    pub func flipVertical() {...}
    pub func resize() {...}

    pub func operator[x, y: int] -> &Self {
        return &pixels[y][x]
    }
}

interface ImageFile {
    abstr static func load(file_name: string) -> Self
    abstr func save(file_name: string)
}

struct PngImage: ImageFile {
    use Image {}

    pub static func load(image_file: string) -> Self {...}
    pub func save(file_name: string) {...}
}
```

Now, when we update the hierarchy with DrawableImage, it is much simpler.

```
struct DrawableImage {
    use Image {}

    pub func drawLine(x, y: Point) {...}
}
```

With composition, you can have much more flexible hierarchies - you can rename and even delete certain methods. See the tutorial for more details.

### What if I want polymorphism?

Jade supports polymorphism through interfaces and row polymorphism. Interfaces can implement other interfaces and hold default implementations, allowing them to resemble abstract classes.

## Explicitness vs Verbosity

Note on properties
Note on explicit conversion between data types

# Unusual/Rare Features

Row polymorphism, pattern matching overloading

Should I include?
