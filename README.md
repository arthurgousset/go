# Go (Cheat Sheet)

> [!TIP]  
> Most of the concepts and text below are taken from the Go track on
> [exercism.org](https://exercism.org/tracks/go). The material is licensed under a MIT license,
> which is included in this repository as well.

## Installation

Official instructions: [go.dev/doc/install](https://go.dev/doc/install).

Instead of using the official instructions, I used `brew` to install Go on my Mac.

```sh
$ brew install golang
```

## Basics (from [exercism.org](https://exercism.org/tracks/go/concepts/basics))

### Introduction

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

## Numbers (from [exercism.org](https://exercism.org/tracks/go/concepts/numbers))

Go contains basic numeric types that can represent sets of either integer or floating-point values.
There are different types depending on the size of value you require and the architecture of the
computer where the application is running (e.g. 32-bit and 64-bit).

For the sake of this exercise you will only be dealing with:

- `int`: e.g. `0`, `255`, `2147483647`. A signed integer that is at least 32 bits in size (value
  range of: -2147483648 through 2147483647). But this will depend on the systems architecture. Most
  modern computers are 64 bit, therefore `int` will be 64 bits in size (value rate of:
  -9223372036854775808 through 9223372036854775807).

- `float64`: e.g. `0.0`, `3.14`. Contains the set of all 64-bit floating-point numbers.

- `uint`: e.g. `0`, `255`. An unsigned integer that is the same size as `int` (value range of: 0
  through 4294967295 for 32 bits and 0 through 18446744073709551615 for 64 bits)

Numbers can be converted to other numeric types through Type Conversion.

## Arithmetic Operators (from [exercism.org](https://exercism.org/tracks/go/concepts/arithmetic-operators))

Go supports many standard arithmetic operators:

| Operator | Example        |
| -------- | -------------- |
| `+`      | `4 + 6 == 10`  |
| `-`      | `15 - 10 == 5` |
| `*`      | `2 * 3 == 6`   |
| `/`      | `13 / 3 == 4`  |
| `%`      | `13 % 3 == 1`  |

For integer division, the remainder is dropped (e.g. `5 / 2 == 2`).

Go has shorthand assignment for the operators above (e.g. `a += 5` is short for `a = a + 5`). Go
also supports the increment and decrement statements `++` and `--` (e.g. `a++`).

### Converting between types

Converting between types is done via a function with the name of the type to convert to. For
example:

```go
var x int = 42 // x has type int
f := float64(x) // f has type float64 (ie. 42.0)
var y float64 = 11.9 // y has type float64
i := int(y) // i has type int (ie. 11)
```

### Arithmetic operations on different types

In many languages you can perform arithmetic operations on different types of variables, but in Go
this gives an error. For example:

```go
var x int = 42

// this line produces an error
value := float32(2.0) * x // invalid operation: mismatched types float32 and int

// you must convert int type to float32 before performing arithmetic operation
value := float32(2.0) * float32(x)
```
