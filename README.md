# Jade Programming Language

This programming language hopes to make the life of the developer easy by taking the middle path between lots of different language types, each with their pros and cons.

## Programming Paradigm

Jade is a multi-paradigm language, meaning that it supports multiple programming paradigms. However, it primarily focuses on object-oriented programming (OOP) and functional programming (FP).

## Static vs dynamic typing

Jade utilizes static typing with support for `any` and union types when necessary for dynamic behavior. Static typing allows for more error detection at compile time and better performance.

## Error handling system

The programming language utilizes errors as values with syntactic sugar that'll make it feel like a checked exception in many ways.

## Compile-time vs runtime errors

All errors that can be caught at compile time will be caught at compile time. Any `panic` that isn't placed in an if statement or loop will be addressed at compile time.

## Use of semicolons

The use of semicolons is a very controversial issue among programming languages. This section will discuss the reasoning behind Jade not having semicolons and the workarounds made for it. We will look at a snippet of Jade code for a database query builder as an example:

```
let query = db.table("users")
    .where("name", "=", "Jane Doe")
    .orWhere("email", "=", "jane@example.com")
    .orderBy("created_at", "desc")
    .limit(10)
```

In a semicolon language, one could simply terminate the last line with a semicolon, and there would be no ambiguity in the statement. However, in non-semicolon languages, this would create ambiguity in what the expression means. The code snippet above would normally get interpreted as:

```
db.table("users");
    .where("name", "=", "Jane Doe");
    .orWhere("email", "=", "jane@example.com");
    .orderBy("created_at", "desc");
    .limit(10);
```

Because whitespace is ignored, the lexer can't tell the difference between the two.

Because Jade is focused on being easy to read, semicolons can't exist - they clutter up the code and make it look less neat. In addition, having too much flexibility in formatting gives the programmer too much freedom in writing messy code.

There are two ways to solve this parsing dilemma without semicolons:

### Use Indentation

An option that has been considered is to simply use the fact that the next line is indented as a cue to continue the previous line onto the next line. After all, we humans primarily rely on indentation for reading these expressions. However, there is a drawback to this approach. Copying code sections from one area to another with different indentation levels is very inconvenient. A code formatter can't figure out the indentation for the programmer because it relies on that information to parse the code.

### Differentiating statements and expressions

Most programming languages do not differentiate statements and expressions. Statements do not return a value and only execute an action. Expressions may execute an action and evaluate to a value.

#### Variable declarations

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

This discussion circulates around the debate between inheritance and composition and ultimately outlines why Jade does not support inheritance, instead providing several rich features for composition.

### Inheritance

Many programming languages support inheritance, the ability to extend a parent class. It is often credited for promoting code reuse.

However, adding inheritance would lead to a few issues:

#### Language complexity

Jade is focused on being a relatively simple language. Adding inheritance would also add on protected members and polymorphism for base classes. Navigating inheritance hierarchies is quite complex and hard to understand and navigate, especially with it often being overused to create very deep inheritance hierarchies.

#### Inflexible Structure and Maintenance Challenges

Inheritance is a very inflexible relationship between the parent and child class. The child class must have all of the parent class's fields, methods, and properties. While this may seem fine at first, maintaining and refactoring code becomes a major challenge. The need for different functionality in the child class can break the entire inheritance hierarchy, making it incredibly difficult to refactor code.

Let's take a look at the following Java example. We are building a framework to store, load, and save images.

```
class Image {
    abstract void load(String filename);
    abstract void save(String fileName);
}
```

## Explicitness vs Verbosity

Note on properties
