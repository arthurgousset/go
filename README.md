# Go (Cheat Sheet)

## Basics (from [exercism.org](https://exercism.org/tracks/go/concepts/basics))

### Installation

Official instructions: [go.dev/doc/install](https://go.dev/doc/install).

Instead of using the official instructions, I used `brew` to install Go on my Mac.

```sh
$ brew install golang
```

### Introduction

> [!TIP]  
> Most of the concepts and text below are taken from the Go track on
> [exercism.org](https://exercism.org/tracks/go). The material is licensed under a MIT license,
> which is included in this repository as well.

[Go](https://golang.org) is a statically typed, compiled programming language. This exercise
introduces three major language features: Packages, Functions, and Variables.

### Packages

Go applications are organized in packages. A package is a collection of source files located in the
same directory. All source files in a directory must share the same package name. When a package is
imported, only entities (functions, types, variables, constants) whose names start with a capital
letter can be used / accessed. The recommended style of naming in Go is that identifiers will be
named using `camelCase`, except for those meant to be accessible across packages which should be
`PascalCase`.

```go
package lasagna
```

### Variables

Go is statically-typed, which means all variables
[must have a defined type](https://en.wikipedia.org/wiki/Type_system) at compile-time.

Variables can be defined by explicitly specifying a type:

```go
var explicit int // Explicitly typed
```

You can also use an initializer, and the compiler will assign the variable type to match the type of
the initializer.

```go
implicit := 10   // Implicitly typed as an int
```

Once declared, variables can be assigned values using the `=` operator. Once declared, a variable's
type can never change.

```go
count := 1 // Assign initial value
count = 2  // Update to new value

count = false // This throws a compiler error due to assigning a non `int` type
```

### Constants

Constants hold a piece of data just like variables, but their value cannot change during the
execution of the program.

Constants are defined using the `const` keyword and can be numbers, characters, strings or booleans:

```go
const Age = 21 // Defines a numeric constant 'Age' with the value of 21
```

### Functions

Go functions accept zero or more parameters. Parameters must be explicitly typed, there is no type
inference.

Values are returned from functions using the `return` keyword.

A function is invoked by specifying the function name and passing arguments for each of the
function's parameters.

Note that Go supports two types of comments. Single line comments are preceded by `//` and multiline
comments are inserted between `/*` and `*/`.

```go
package greeting

// Hello is a public function.
func Hello (name string) string {
    return hi(name)
}

// hi is a private function.
func hi (name string) string {
    return "hi " + name
}
```

## Comments (from [exercism.org](https://exercism.org/tracks/go/exercises/weather-forecast))

In the previous exercise, we saw that there are two ways to write comments in Go: single-line
comments that are preceded by `//`, and multiline comment blocks that are wrapped with `/*` and
`*/`.

### Documentation comments

In Go, comments play an important role in documenting code. They are used by the `godoc` command,
which extracts these comments to create documentation about Go packages. A documentation comment
should be a complete sentence that starts with the name of the thing being described and ends with a
period.

Comments should precede packages as well as exported identifiers, for example exported functions,
methods, package variables, constants, and structs, which you will learn more about in the next
exercises.

A package-level variable can look like this:

```go
// TemperatureCelsius represents a certain temperature in degrees Celsius.
var TemperatureCelsius float64
```

### Package comments

Package comments should be written directly before a package clause (`package x`) and begin with
`Package x ...` like this:

```go
// Package kelvin provides tools to convert
// temperatures to and from Kelvin.
package kelvin
```

### Function comments

A function comment should be written directly before the function declaration. It should be a full
sentence that starts with the function name. For example, an exported comment for the function
`Calculate` should take the form `Calculate ...`. It should also explain what arguments the function
takes, what it does with them, and what its return values mean, ending in a period):

```go
// CelsiusFreezingTemp returns an integer value equal to the temperature at which water freezes in degrees Celsius.
func CelsiusFreezingTemp() int {
	return 0
}
```
