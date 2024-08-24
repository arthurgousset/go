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

## Booleans (from [exercism.org](https://exercism.org/tracks/go/concepts/booleans))

Booleans in Go are represented by the predeclared boolean type `bool`, which values can be either
`true` or `false`. It's a defined type.

```go
var closed bool    // boolean variable 'closed' implicitly initialized with 'false'
speeding := true   // boolean variable 'speeding' initialized with 'true'
hasError := false  // boolean variable 'hasError' initialized with 'false'
```

Go supports three logical operators that can evaluate expressions down to Boolean values, returning
either `true` or `false`.

| Operator    | What it means                                 |
| ----------- | --------------------------------------------- |
| `&&` (and)  | It is true if both statements are true.       |
| `\|\|` (or) | It is true if at least one statement is true. |
| `!` (not)   | It is true only if the statement is false.    |

## Strings (from [exercism.org](https://exercism.org/tracks/go/concepts/strings))

A `string` in Go is an immutable sequence of bytes, which don't necessarily have to represent
characters.

A string literal is defined between double quotes:

```go
const name = "Jane"
```

Strings can be concatenated via the `+` operator:

```go
"Jane" + " " + "Austen"
// => "Jane Austen"
```

Some special characters need to be escaped with a leading backslash, such as `\t` for a tab and `\n`
for a new line in strings.

```go
"How is the weather today?\nIt's sunny"
// =>
// How is the weather today?
// It's sunny
```

The `strings` package contains many useful functions to work on strings. For more information about
string functions, check out the [strings package documentation](https://pkg.go.dev/strings). Here
are some examples:

```go
import "strings"

// strings.ToLower returns the string given as argument with all its characters lowercased
strings.ToLower("MaKEmeLoweRCase")
// => "makemelowercase"

// strings.Repeat returns a string with a substring given as argument repeated many times
strings.Repeat("Go", 3)
// => "GoGoGo"
```

## Comparison (from [exercism.org](https://exercism.org/tracks/go/concepts/comparison))

In Go numbers can be compared using the following relational and equality operators.

| Comparison       | Operator |
| ---------------- | -------- |
| equal            | `==`     |
| not equal        | `!=`     |
| less             | `<`      |
| less or equal    | `<=`     |
| greater          | `>`      |
| greater or equal | `>=`     |

The result of the comparison is always a boolean value, so either `true` or `false`.

```go
a := 3

a != 4 // true
a > 5  // false
```

The comparison operators above can also be used to compare strings. In that case a lexicographical
(dictionary) order is applied. For example:

```Go
	"apple" < "banana"  // true
	"apple" > "banana"  // false
```

## Conditionals If (from [exercism.org](https://exercism.org/tracks/go/concepts/conditionals-if))

Conditionals in Go are similar to conditionals in other languages. The underlying type of any
conditional operation is the `bool` type, which can have the value of `true` or `false`.
Conditionals are often used as flow control mechanisms to check for various conditions.

For checking a particular case an `if` statement can be used, which executes its code if the
underlying condition is `true` like this:

```go
var value string

if value == "val" {
    return "was val"
}
```

In scenarios involving more than one case many `if` statements can be chained together using the
`else if` and `else` statements.

```go
var number int
result := "This number is "

if number > 0 {
    result += "positive"
} else if number < 0 {
    result += "negative"
} else {
    result += "zero"
}
```

If statements can also include a short initialization statement that can be used to initialize one
or more variables for the if statement. For example:

```go
num := 7
if v := 2 * num; v > 10 {
    fmt.Println(v)
} else {
    fmt.Println(num)
}
// Output: 14
```

> Note: any variables created in the initialization statement go out of scope after the end of the
> if statement.

## Packages (from [exercism.org](https://exercism.org/tracks/go/concepts/packages))

In Go an application is organized in packages. A package is a collection of source files located in
the same folder. All source files in a folder must have the same package name at the top of the
file. By convention packages are named to be the same as the folder they are located in.

```go
package greeting
```

Go provides an extensive standard library of packages which you can use in your program using the
`import` keyword. Standard library packages are imported using their name.

```go
package greeting

import "fmt"
```

An imported package is then addressed with the package name:

```go
world := "World"
fmt.Sprintf("Hello %s", world)
```

Go determines if an item can be called by code in other packages through how it is declared. To make
a function, type, variable, constant or struct field externally visible (known as _exported_) the
name must start with a capital letter.

```go
package greeting

// Hello is a public function (callable from other packages).
func Hello(name string) string {
    return "Hello " + name
}

// hello is a private function (not callable from other packages).
func hello(name string) string {
    return "Hello " + name
}
```

## String Formatting (from [exercism.org](https://exercism.org/tracks/go/concepts/string-formatting))

Go provides an in-built package called `fmt` (format package) which offers a variety of functions to
manipulate the format of input and output. The most commonly used function is `Sprintf`, which uses
verbs like `%s` to interpolate values into a string and returns that string.

```go
import "fmt"

food := "taco"
fmt.Sprintf("Bring me a %s", food)
// Returns: Bring me a taco
```

In Go floating point values are conveniently formatted with Sprintf's verbs: `%g` (compact
representation), `%e` (exponent) or `%f` (non exponent). All three verbs allow the field's width and
numeric position to be controlled.

```go
import "fmt"

number := 4.3242
fmt.Sprintf("%.2f", number)
// Returns: 4.32
```

You can find a full list of available verbs in the [format package documentation][fmt-docs].

`fmt` contains other functions for working with strings, such as `Println` which simply prints the
arguments it receives to the console and `Printf` which formats the input in the same way as
`Sprintf` before printing it.

[fmt-docs]: https://pkg.go.dev/fmt

### Format 'verbs'

Source: [pkg.go.dev/fmt](https://pkg.go.dev/fmt)

General:

```
%v	the value in a default format
	when printing structs, the plus flag (%+v) adds field names
%#v	a Go-syntax representation of the value
	(floating-point infinities and NaNs print as ±Inf and NaN)
%T	a Go-syntax representation of the type of the value
%%	a literal percent sign; consumes no value
```

Boolean:

```
%t	the word true or false
```

Integer:

```
%b	base 2
%c	the character represented by the corresponding Unicode code point
%d	base 10
%o	base 8
%O	base 8 with 0o prefix
%q	a single-quoted character literal safely escaped with Go syntax.
%x	base 16, with lower-case letters for a-f
%X	base 16, with upper-case letters for A-F
%U	Unicode format: U+1234; same as "U+%04X"
```

Floating-point and complex constituents:

```
%b	decimalless scientific notation with exponent a power of two,
	in the manner of strconv.FormatFloat with the 'b' format,
	e.g. -123456p-78
%e	scientific notation, e.g. -1.234456e+78
%E	scientific notation, e.g. -1.234456E+78
%f	decimal point but no exponent, e.g. 123.456
%F	synonym for %f
%g	%e for large exponents, %f otherwise. Precision is discussed below.
%G	%E for large exponents, %F otherwise
%x	hexadecimal notation (with decimal power of two exponent), e.g. -0x1.23abcp+20
%X	upper-case hexadecimal notation, e.g. -0X1.23ABCP+20
```

String and slice of bytes (treated equivalently with these verbs):

```
%s	the uninterpreted bytes of the string or slice
%q	a double-quoted string safely escaped with Go syntax
%x	base 16, lower-case, two characters per byte
%X	base 16, upper-case, two characters per byte
```

Slice:

```
%p	address of 0th element in base 16 notation, with leading 0x
```

Pointer:

```
%p	base 16 notation, with leading 0x
The %b, %d, %o, %x and %X verbs also work with pointers,
formatting the value exactly as if it were an integer.
```

The default format for %v is:

```
bool:                    %t
int, int8 etc.:          %d
uint, uint8 etc.:        %d, %#x if printed with %#v
float32, complex64, etc: %g
string:                  %s
chan:                    %p
pointer:                 %p
```

For compound objects, the elements are printed using these rules, recursively, laid out like this:

```
struct:             {field0 field1 ...}
array, slice:       \[elem0 elem1 ...\]
maps:               map\[key1:value1 key2:value2 ...\]
pointer to above:   &{}, &\[\], &map\[\]
```

Width is specified by an optional decimal number immediately preceding the verb. If absent, the
width is whatever is necessary to represent the value. Precision is specified after the (optional)
width by a period followed by a decimal number. If no period is present, a default precision is
used. A period with no following number specifies a precision of zero. Examples:

```
%f     default width, default precision
%9f    width 9, default precision
%.2f   default width, precision 2
%9.2f  width 9, precision 2
%9.f   width 9, precision 0
```

Width and precision are measured in units of Unicode code points, that is, runes. (This differs from
C's printf where the units are always measured in bytes.) Either or both of the flags may be
replaced with the character '\*', causing their values to be obtained from the next operand
(preceding the one to format), which must be of type int.
