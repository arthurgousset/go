# Go (Cheat Sheet)

> [!TIP]  
> Most of the concepts and text below are taken from the Go track on
> [exercism.org](https://exercism.org/tracks/go). The material is licensed under a MIT license,
> which is included in this repository as well.

Other resources:

1. [yourbasic.org](https://yourbasic.org/) by [prof. Stefan Nilsson](https://yourbasic.org/about/)
   > Code should be correct, clear and efficient. Prefer simple. Avoid clever.

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

## Functions (from [exercism.org](https://exercism.org/tracks/go/concepts/functions))

A function allows you to group code into a reusable unit. It consists of the `func` keyword, the
name of the function, and a comma-separated list of zero or more parameters and types in round
brackets.

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

#x## Function Parameters

All parameters must be explicitly typed; there is no type inference for parameters. There are no
default values for parameters so all function parameters are required.

```go
import "fmt"

// No parameters
func PrintHello() {
    fmt.Println("Hello")
}

// Two parameters
func PrintGreetingName(greeting string, name string) {
  fmt.Println(greeting + " " + name)
}
```

Parameters of the same type can be declared together, followed by a single type declaration.

```go
import "fmt"

func PrintGreetingName(greeting, name string) {
  fmt.Println(greeting + " " + name)
}
```

### Parameters vs. Arguments

Let's quickly cover two terms that are often confused together: `parameters` and `arguments`.
Function parameters are the names defined in the function's signature, such as `greeting` and `name`
in the function `PrintGreetingName` above. Function arguments are the concrete values passed to the
function parameters when we invoke the function. For instance, in the example below, `"Hello"` and
`"Katrina"` are the arguments passed to the `greeting` and `name` parameters:

```go
PrintGreetingName("Hello", "Katrina")
```

### Return Values

The function parameters are followed by zero or more return values which must also be explicitly
typed. Single return values are left bare, multiple return values are wrapped in parenthesis. Values
are returned to the calling code from functions using the `return` keyword. There can be multiple
`return` statements in a function. The execution of the function ends as soon as it hits one of
those `return` statements. If multiple values are to be returned from a function, they are comma
separated.

```go
func Hello(name string) string {
  return "Hello " + name
}

func HelloAndGoodbye(name string) (string, string) {
  return "Hello " + name, "Goodbye " + name
}
```

### Invoking Functions

Invoking a function is done by specifying the function name and passing arguments for each of the
function's parameters in parenthesis.

```go
import "fmt"

// No parameters, no return value
func PrintHello() {
    fmt.Println("Hello")
}
// Called like this:
PrintHello()

// One parameter, one return value
func Hello(name string) string {
  return "Hello " + name
}
// Called like this:
greeting := Hello("Dave")

// Multiple parameters, multiple return values
func SumAndMultiply(a, b int) (int, int) {
    return a+b, a*b
}
// Called like this:
aplusb, atimesb := SumAndMultiply(a, b)
```

### Named Return Values and Naked Return

As well as parameters, return values can optionally be named. If named return values are used, a
`return` statement without arguments will return those values. This is known as a 'naked' return.

```go
func SumAndMultiplyThenMinus(a, b, c int) (sum, mult int) {
    sum, mult = a+b, a*b
    sum -= c
    mult -= c
    return
}
```

### Pass by Value vs. Pass by Reference

It is also important to clarify the concept of passing by value and passing by reference.

First, let's clarify passing by value. In the example below, when we pass the variable `val` to the
function `MultiplyByTwo`, we passed a copy of `val`. Because of this, `newVal` has the updated value
`4` but the original variable `val` is still `2`. Behind the scene, Go essentially makes a copy of
the original value so that only this copy, a.k.a. `v`, is modified by the function.

```go
val := 2
func MultiplyByTwo(v int) int {
    v = v * 2
    return v
}
newVal := MultiplyByTwo(val)
// newval is 4, val is still 2 because only a copy of its value was passed into the function
```

Strictly speaking, all arguments are passed by value in Go, i.e. a copy is made of the value or data
provided to the function. But if you don't want to make a copy of the data that is passed to a
function and want to change the data in the function, then you should use `pointers` as arguments,
a.k.a. pass by reference.

### Pointers

We use a `pointer` to achieve passing by reference. By passing `pointer` arguments into a function,
we could modify the underlying data passed into the function instead of only operating on a copy of
the data.

For now, it is sufficient to know that pointer types can be recognized by the `*` in front of the
type in the function signature.

```go
func HandlePointers(x, y *int) {
    // Some logic to handle integer values referenced by pointers x and y
}
```

If the concept of `pointer` is confusing, no worries. We have a dedicated section later in the
syllabus to help you master pointers.

### Exceptions

Note that `slices` and `maps` are exceptions to the above-mentioned rule. When we pass a `slice` or
a `map` as arguments into a function, they are treated as pointer types even though there is no
explicit `*` in the type. This means that if we pass a slice or map into a function and modify its
underlying data, the changes will be reflected on the original slice or map.

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

## Floating-point numbers (from [exercism.org](https://exercism.org/tracks/go/concepts/floating-point-numbers))

A floating-point number is a number with zero or more digits behind the decimal separator. Examples
are `-2.4`, `0.1`, `3.14`, `16.984025` and `1024.0`.

Different floating-point types can store different numbers of digits after the digit separator -
this is referred to as its precision.

Go has two floating-point types:

- `float32`: 32 bits (~6-9 digits precision).
- `float64`: 64 bits (~15-17 digits precision). This is the default floating-point type.

As can be seen, both types can store a different number of digits. This means that trying to store
PI in a `float32` will only store the first 6 to 9 digits (with the last digit being rounded).

By default, Go will use `float64` for floating-point numbers, unless the floating-point number is:

1. assigned to a variable with type `float32`, or
2. returned from a function with return type `float32`, or
3. passed as an argument to the `float32()` function.

The `math` package contains many helpful mathematical functions.

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

Other flags:

```
'+'	always print a sign for numeric values;
	guarantee ASCII-only output for %q (%+q)
'-'	pad with spaces on the right rather than the left (left-justify the field)
'#'	alternate format: add leading 0b for binary (%#b), 0 for octal (%#o),
	0x or 0X for hex (%#x or %#X); suppress 0x for %p (%#p);
	for %q, print a raw (backquoted) string if \[strconv.CanBackquote\]
	returns true;
	always print a decimal point for %e, %E, %f, %F, %g and %G;
	do not remove trailing zeros for %g and %G;
	write e.g. U+0078 'x' if the character is printable for %U (%#U)
' '	(space) leave a space for elided sign in numbers (% d);
	put spaces between bytes printing strings or slices in hex (% x, % X)
'0'	pad with leading zeros rather than spaces;
	for numbers, this moves the padding after the sign
```

## Slices (from [exercism.org](https://exercism.org/tracks/go/concepts/slices))

Slices in Go are similar to lists or arrays in other languages. They hold several elements of a
specific type (or interface).

Slices in Go are based on arrays. Arrays have a fixed size. A slice, on the other hand, is a
dynamically-sized, flexible view of the elements of an array.

A slice is written like `[]T` with `T` being the type of the elements in the slice:

```go
var empty []int                 // an empty slice
withData := []int{0,1,2,3,4,5}  // a slice pre-filled with some data
```

You can get or set an element at a given zero-based index using the square-bracket notation:

```go
withData[1] = 5
x := withData[1] // x is now 5
```

You can create a new slice from an existing slice by getting a range of elements. Once again using
square-bracket notation, but specifying both a starting (inclusive) and ending (exclusive) index. If
you don't specify a starting index, it defaults to 0. If you don't specify an ending index, it
defaults to the length of the slice.

```go
newSlice := withData[2:4]
// => []int{2,3}
newSlice := withData[:2]
// => []int{0,1}
newSlice := withData[2:]
// => []int{2,3,4,5}
newSlice := withData[:]
// => []int{0,1,2,3,4,5}
```

You can add elements to a slice using the `append` function. Below we append `4` and `2` to the `a`
slice.

```go
a := []int{1, 3}
a = append(a, 4, 2)
// => []int{1,3,4,2}
```

`append` always returns a new slice, and when we just want to append elements to an existing slice,
it's common to reassign it back to the slice variable we pass as the first argument as we did above.

`append` can also be used to merge two slices:

```go
nextSlice := []int{100,101,102}
newSlice  := append(withData, nextSlice...)
// => []int{0,1,2,3,4,5,100,101,102}
```

### Copy slices

Use `make([]T, <length>)` to create a slice of type `T` and length `<length>`.

```go
arr := []int{1, 2, 3}
tmp := make([]int, len(arr))
copy(tmp, arr)

fmt.Println(tmp)
// => [1 2 3]
fmt.Println(arr)
// => [1 2 3]
```

> The
> built-in [`copy(dst, src)`](http://golang.org/pkg/builtin/#copy) copies `min(len(dst), len(src))` elements.
> So if your `dst` is empty (`len(dst) == 0`), nothing will be copied.
> Try `tmp := make([]int, len(arr))` ([Go Playground](https://play.golang.org/p/xwISeGLzxb)):

Source: [stackoverflow.com](https://stackoverflow.com/a/30182622)

## Variadic Functions (from [exercism.org](https://exercism.org/tracks/go/concepts/variadic-functions))

Usually, functions in Go accept only a fixed number of arguments. However, it is also possible to
write variadic functions in Go.

A variadic function is a function that accepts a variable number of arguments.

If the type of the last parameter in a function definition is prefixed by ellipsis `...`, then the
function can accept any number of arguments for that parameter.

```go
func find(a int, b ...int) {
    // ...
}
```

In the above function, parameter `b` is variadic and we can pass 0 or more arguments to `b`.

```go
find(5, 6)
find(5, 6, 7)
find(5)
```

> [!NOTE]  
> The variadic parameter must be the last parameter of the function.

The way variadic functions work is by converting the variable number of arguments to a slice of the
type of the variadic parameter.

Here is an example of an implementation of a variadic function.

```go
func find(num int, nums ...int) {
    fmt.Printf("type of nums is %T\n", nums)

    for i, v := range nums {
        if v == num {
            fmt.Println(num, "found at index", i, "in", nums)
            return
        }
    }

    fmt.Println(num, "not found in ", nums)
}

func main() {
    find(89, 90, 91, 95)
    // =>
    // type of nums is []int
    // 89 not found in  [90 91 95]

    find(45, 56, 67, 45, 90, 109)
    // =>
    // type of nums is []int
    // 45 found at index 2 in [56 67 45 90 109]

    find(87)
    // =>
    // type of nums is []int
    // 87 not found in  []
}
```

In line `find(89, 90, 91, 95)` of the program above, the variable number of arguments to the find
function are `90`, `91` and `95`. The `find` function expects a variadic int parameter after `num`.
Hence these three arguments will be converted by the compiler to a slice of type `int`
`[]int{90, 91, 95}` and then it will be passed to the find function as `nums`.

Sometimes you already have a slice and want to pass that to a variadic function. This can be
achieved by passing the slice followed by `...`. That will tell the compiler to use the slice as is
inside the variadic function. The step described above where a slice is created will simply be
omitted in this case.

```go
list := []int{1, 2, 3}
find(1, list...) // "find" defined as shown above
```

## Conditionals Switch (from [exercism.org](https://exercism.org/tracks/go/concepts/conditionals-switch))

Like other languages, Go also provides a `switch` statement. Switch statements are a shorter way to
write long `if ... else if` statements. To make a switch, we start by using the keyword `switch`
followed by a value or expression. We then declare each one of the conditions with the `case`
keyword. We can also declare a `default` case, that will run when none of the previous `case`
conditions matched:

```go
operatingSystem := "windows"

switch operatingSystem {
case "windows":
    // do something if the operating system is windows
case "linux":
    // do something if the operating system is linux
case "macos":
    // do something if the operating system is macos
default:
    // do something if the operating system is none of the above
}
```

One interesting thing about switch statements, is that the value after the `switch` keyword can be
omitted, and we can have boolean conditions for each `case`:

```go
age := 21

switch {
case age > 20 && age < 30:
    // do something if age is between 20 and 30
case age == 10:
    // do something if age is equal to 10
default:
    // do something else for every other case
}
```

## Structs (from [exercism.org](https://exercism.org/tracks/go/concepts/structs))

In Go, a `struct` is a sequence of named elements called _fields_, each field has a name and type.
The name of a field must be unique within the struct. Structs can be compared with _classes_ in the
Object-Oriented Programming paradigm.

### Defining a struct

You create a new struct by using the `type` and `struct` keywords, and explicitly define the name
and type of the fields. For example, here we define `Shape` struct, that holds the name of a shape
and its size:

```go
type Shape struct {
    name string
    size int
}
```

Field names in structs follow the Go convention - fields whose name starts with a lower case letter
are only visible to code in the same package, whereas those whose name starts with an upper case
letter are visible in other packages.

### Creating instances of a struct

Once you have defined the `struct`, you need to create a new instance defining the fields using
their field name in any order:

```go
s := Shape {
    name: "Square",
    size: 25,
}
```

To read or modify instance fields, use the `.` notation:

```go
// Update the name and the size
s.name = "Circle"
s.size = 35
fmt.Printf("name: %s size: %d\n", s.name, s.size)
// Output: name: Circle size: 35
```

Fields that don't have an initial value assigned, will have their zero value. For example:

```go
s := Shape{name: "Circle"}
fmt.Printf("name: %s size: %d\n", s.name, s.size)
// Output: name: Circle size: 0
```

You can create an instance of a `struct` without using the field names, as long as you define the
fields _in order_:

```go
s := Shape {
	"Oval",
	20,
}
```

However, this syntax is considered _brittle code_ since it can break when a field is added,
especially when the new field is of a different type. In the following example we add an extra field
to `Shape`:

```go
type Shape struct {
	name        string
	description string // new field 'description' added
	size        int
}

s := Shape{
    "Circle",
    20,
}
// Since the second field of the struct is now a string and not an int,
// the compiler will throw an error when compiling the program:
// Output: cannot use 20 (type untyped int) as type string in field value
// Output: too few values in Shape{...}
```

### "New" functions

Sometimes it can be nice to have functions that help us create struct instances. By convention,
these functions are usually called `New` or have their names starting with `New`, but since they are
just regular functions, you can give them any name you want. They might remind you of constructors
in other languages, but in Go they are just regular functions.

In the following example, one of these `New` functions is used to create a new instance of `Shape`
and automatically set a default value for the `size` of the shape:

```go
func NewShape(name string) Shape {
	return Shape{
		name: name,
		size: 100, //default-value for size is 100
	}
}

NewShape("Triangle")
// => Shape { name: "Triangle", size: 100 }

```

Using `New` functions can have the following advantages:

- validation of the given values
- handling of default-values
- since `New` functions are often declared in the same package of the structs they initialize, they
  can initialize even private fields of the struct

## For Loops (from [exercism.org](https://exercism.org/tracks/go/concepts/for-loops))

### General syntax

The for loop is one of the most commonly used statements to repeatedly execute some logic. In Go it
consists of the `for` keyword, a header and a code block that contains the body of the loop wrapped
in curly brackets. The header consists of 3 components separated by semicolons `;`: init, condition
and post.

```go
for init; condition; post {
  // loop body - code that is executed repeatedly as long as the condition is true
}
```

- The **init** component is some code that runs only once before the loop starts.
- The **condition** component must be some expression that evaluates to a boolean and controls when
  the loop should stop. The code inside the loop will run as long as this condition evaluates to
  true. As soon as this expression evaluates to false, no more iterations of the loop will run.
- The **post** component is some code that will run at the end of each iteration.

**Note:** Unlike other languages, there are no parentheses `()` surrounding the three components of
the header. In fact, inserting such parenthesis is a compilation error. However, the braces `{ }`
surrounding the loop body are always required.

### For Loops - An example

The init component usually sets up a counter variable, the condition checks whether the loop should
be continued or stopped and the post component usually increments the counter at the end of each
repetition.

```go
for i := 1; i < 10; i++ {
  fmt.Println(i)
}
```

This loop will print the numbers from `1` to `9` (including `9`). Defining the step is often done
using an increment or decrement statement, as shown in the example above.

## While loops

Source: [programiz.com](https://www.programiz.com/golang/while-loop)

Unlike other programming languages, Go doesn't have a dedicated keyword for a while loop. However,
we can use the `for` loop to perform the functionality of a while loop.

Syntax of Go while loop:

```go
for condition {
  // code block
}
```

Here, the loop evaluates the `condition`. If the condition is:

- `true` - statements inside the loop are executed and `condition` is evaluated again
- `false` - the loop terminates

## Randomness (from [exercism.org](https://exercism.org/tracks/go/concepts/randomness))

Package [math/rand][mathrand] provides support for generating pseudo-random numbers.

Here is how to generate a random integer between `0` and `99`:

```go
n := rand.Intn(100) // n is a random int, 0 <= n < 100
```

Function `rand.Float64` returns a random floating point number between `0.0` and `1.0`:

```go
f := rand.Float64() // f is a random float64, 0.0 <= f < 1.0
```

There is also support for shuffling a slice (or other data structures):

```go
x := []string{"a", "b", "c", "d", "e"}
// shuffling the slice put its elements into a random order
rand.Shuffle(len(x), func(i, j int) {
	x[i], x[j] = x[j], x[i]
})
```

### Seeds

The number sequences generated by package `math/rand` are not truly random. Given a specific "seed"
value, the results are entirely deterministic.

In Go 1.20+ the seed is automatically picked at random so you will see a different sequence of
random numbers each time you run your program.

In prior versions of Go, the seed was `1` by default. So to get different sequences for various runs
of the program, you had to manually seed the random number generator, e.g. with the current time,
before retrieving any random numbers.

```go
rand.Seed(time.Now().UnixNano())
```

[mathrand]: https://pkg.go.dev/math/rand

## Maps (for [exercism.org](https://exercism.org/tracks/go/concepts/maps))

In Go, `map` is a built-in data type that maps keys to values. In other programming languages, you
might be familiar with the concept of `map` as a dictionary, hash table, key/value store or an
associative array.

Syntactically, `map` looks like this:

```go
map[type]type
```

It is also important to know that each key is unique, meaning that assigning the same key twice will
overwrite the value of the corresponding key.

To create a map, you can do:

```go
  // With map literal
  foo := map[string]int{}
```

or

```go
  // or with make function
  foo := make(map[string]int)
```

Here are some operations that you can do with a map

```go
  // Add a value in a map with the `=` operator:
  foo["bar"] = 42
  // Here we update the element of `bar`
  foo["bar"] = 73
  // To retrieve a map value, you can use
  baz := foo["bar"]
  // To delete an item from a map, you can use
  delete(foo, "bar")
```

If you try to retrieve the value for a key which does not exist in the map, it will return the zero
value of the value type. This can confuse you, especially if the default value of your `ElementType`
(for example, 0 for an int), is a valid value. To check whether a key exists in your map, you can
use

```go
  value, exists := foo["baz"]
  // If the key "baz" does not exist,
  // value: 0; exists: false
```

## Time (from [exercism.org](https://exercism.org/tracks/go/concepts/time))

A [`Time`][time] in Go is a type describing a moment in time. The date and time information can be
accessed, compared, and manipulated through its methods, but there are also some functions called on
the `time` package itself. The current date and time can be retrieved through the [`time.Now`][now]
function.

The [`time.Parse`][parse] function parses strings into values of type `Time`. Go has a special way
of how you define the layout you expect for the parsing. You need to write an example of the layout
using the values from this special timestamp: **`Mon Jan 2 15:04:05 -0700 MST 2006`**.

For example:

```go
import "time"

func parseTime() time.Time {
    date := "Tue, 09/22/1995, 13:00"
    layout := "Mon, 01/02/2006, 15:04"

    t, err := time.Parse(layout,date) // time.Time, error
}

// => 1995-09-22 13:00:00 +0000 UTC
```

The [`Time.Format()`][format] method returns a string representation of time. Just as with the
`Parse` function, the target layout is again defined via an example that uses the values from the
special timestamp.

For Example:

```go
import (
    "fmt"
    "time"
)

func main() {
    t := time.Date(1995,time.September,22,13,0,0,0,time.UTC)
    formattedTime := t.Format("Mon, 01/02/2006, 15:04") // string
    fmt.Println(formattedTime)
}

// => Fri, 09/22/1995, 13:00
```

### Layout Options

> [!NOTE]  
> All custom layouts must be defined using the values of this special timestamp:
> **`Mon Jan 2 15:04:05 -0700 MST 2006`** For example:
>
> ```go
> layout := "1/2/2006 15:04:05"
> layout := "2006-January-2"
> ```

For a custom layout use combination of these options. In Go predefined date and timestamp [format
constants][const] are also available.

| Time        | Options                                        |
| ----------- | ---------------------------------------------- |
| Year        | 2006 ; 06                                      |
| Month       | Jan ; January ; 01 ; 1                         |
| Day         | 02 ; 2 ; \_2 (For preceding 0)                 |
| Weekday     | Mon ; Monday                                   |
| Hour        | 15 ( 24 hour time format ) ; 3 ; 03 (AM or PM) |
| Minute      | 04 ; 4                                         |
| Second      | 05 ; 5                                         |
| AM/PM Mark  | PM                                             |
| Day of Year | 002 ; \_\_2                                    |

The `time.Time` type has various methods for accessing a particular time. e.g. Hour :
[`Time.Hour()`][hour] , Month : [`Time.Month()`][month]. More on how this works can be found in the
[official documentation][time].

The [`time`][time] includes another type, [`Duration`][duration], representing elapsed time, plus
support for locations/time zones, timers, and other related functionality that will be covered in
another concept.

[time]: https://golang.org/pkg/time/#Time
[now]: https://golang.org/pkg/time/#Now
[const]: https://pkg.go.dev/time#pkg-constants
[format]: https://pkg.go.dev/time#Time.Format
[hour]: https://pkg.go.dev/time#Time.Hour
[month]: https://pkg.go.dev/time/#Time.Month
[duration]: https://pkg.go.dev/time#Duration
[parse]: https://golang.org/pkg/time/#Parse
[article]: https://www.pauladamsmith.com/blog/2011/05/go_time.html

## Range Iteration (from [exercism.org](https://exercism.org/tracks/go/concepts/range-iteration))

In Go, you can iterate over a `slice` using `for` and an index, or you can use `range`. `range` also
allows you to iterate over a `map`.

Every iteration returns two values: the index/key and a copy of the element at that index/key.

### Iterate over a slice

Easy as pie, loops over a slice, ordered as expected.

```go
arr := []int{10, 20, 30}
for i, x := range arr {
  fmt.Println(i, x)
}
// outputs:
// 0, 10
// 1, 20
// 2, 30
```

### Iterate over a map

Iterating over a map raises a new problem. The order is now random.

```go
hash := map[int]int{9: 10, 99: 20, 999: 30}
for k, v := range hash {
  fmt.Println(k, v)
}
// outputs, for example:
// 99 20
// 999 30
// 9 10
```

> [!NOTE]  
> It may seem the above output is incorrect, as one would expect the first key/value pair on the
> declaration of the map `9 10` to be the first one printed and not the last. However, maps are
> unordered by nature - there isn't a first or last key/value pair. Because of that, when iterating
> over the entries of a map, the order by which entries will be visited will be random and not
> follow any specific pattern. This means the above output is possible but might differ from what
> you get if you try to run this yourself. To learn more about this see
> [Go Language Spec: range clause](https://go.dev/ref/spec#RangeClause).

### Iteration omitting key or value

In Go an unused variable will raise an error at build time. Sometimes you only need the value, as
per the first example:

```go
arr := []int{10, 20, 30}
for i, x := range arr {
  fmt.Println(x)
}
// Go build failed: i declared but not used
```

You can replace the `i` with `_` which tells the compiler we don't use that value:

```go
arr := []int{10, 20, 30}
for _, x := range arr {
  fmt.Println(x)
}
// outputs:
// 10
// 20
// 30
```

If you want to only print the index, you can replace the `x` with `_`, or simply omit the
declaration:

```go
arr := []int{10, 20, 30}
// for i, _ := range arr {
for i := range arr {
  fmt.Println(i)
}
// outputs:
// 0
// 1
// 2
```

## Type Definitions (from [exercism.org](https://exercism.org/tracks/go/concepts/type-definitions))

We've already seen struct types; to recap a `struct` is a sequence of named elements
called *fields*, each field having a name and type. The name of a field must be unique within the
struct. Structs can be compared with the *class* in the Object Oriented Programming paradigm.

You create a new struct by using the `type` and `struct` keywords, then explicitly define the name
and type of the fields as shown in the example below.

```
type StructName struct{
    field1 fieldType1
    field2 fieldType2
}
```

### Non-struct types

You've previously seen defining struct types. It is also possible to define non-struct types which
you can use as an alias for a built-in type declaration, and you can define receiver functions on
them to extend them in the same way as struct types.

```go
type Name string
func SayHello(n Name) {
  fmt.Printf("Hello %s\n", n)
}
n := Name("Fred")
SayHello(n)
// Output: Hello Fred
```

You can also define non-struct types composed of arrays and maps.

```go
type Names []string
func SayHello(n Names) {
  for _, name := range n {
    fmt.Printf("Hello %s\n", name)
  }
}
n := Names([]string{"Fred", "Bill"})
SayHello(n)
// Output:
// Hello Fred
// Hello Bill
```

## Pointers (from exercism.org)

Like many other languages, Go has pointers. If you're new to pointers, they can feel a little
mysterious but once you get used to them, they're quite straight-forward. They're a crucial part of
Go, so take some time to really understand them.

Before digging into the details, it's worth understanding the use of pointers. Pointers are a way to
share memory with other parts of our program, which is useful for two major reasons:

1. When we have large amounts of data, making copies to pass between functions is very inefficient.
   By passing the memory location of where the data is stored instead, we can dramatically reduce
   the resource-footprint of our programs.
2. By passing pointers between functions, we can access and modify the single copy of the data
   directly, meaning that any changes made by one function are immediately visible to other parts of
   the program when the function ends.

### Variables, Memory, and Pointers

Let's say we have a regular integer variable `a`:

```go
var a int
```

When we declare a variable, Go has to find a place in memory to store its value. This is largely
abstracted from us — when we need to fetch the value stored in that piece of memory, we can just
refer to it by the variable name.

For instance, when we write `a + 2`, we are effectively fetching the value stored in the memory
associated with the variable `a` and adding 2 to it.

Similarly, when we need to change the value in the piece of memory of `a`, we can use the variable
name to do an assignment:

```go
a = 3
```

The piece of memory that is associated with `a` will now be storing the value `3`.

While variables allow us to refer to values in memory, sometimes it's useful to know the **memory
address** to which the variable is pointing. **Pointers** hold the memory addresses of those values.
You declare a variable with a pointer type by prefixing the underlying type with an asterisk:

```go
var p *int // 'p' contains the memory address of an integer
```

Here we declare a variable `p` of type "pointer to int" (`*int`). This means that `p` will hold the
memory address of an integer. The zero value of pointers is `nil` because a `nil` pointer holds no
memory address.

### Getting a pointer to a variable

To find the memory address of the value of a variable, we can use the `&` operator. For example, if
we want to find and store the memory address of variable `a` in the pointer `p`, we can do the
following:

```go
var a int
a = 2

var p *int
p = &a // the variable 'p' contains the memory address of 'a'
```

### Accessing the value via a pointer (dereferencing)

When we have a pointer, we might want to know the value stored in the memory address the pointer
represents. We can do this using the `*` operator:

```go
var a int
a = 2

var p *int
p = &a // the variable 'p' contains the memory address of 'a'

var b int
b = *p // b == 2
```

The operation `*p` fetches the value stored at the memory address stored in `p`. This operation is
often called "dereferencing".

We can also use the dereference operator to assign a new value to the memory address referenced by
the pointer:

```go
var a int        // declare int variable 'a'
a = 2            // assign 'a' the value of 2

var pa *int
pa = &a          // 'pa' now contains to the memory address of 'a'
*pa = *pa + 2    // increment by 2 the value at memory address 'pa'

fmt.Println(a)   // Output: 4
                 // 'a' will have the new value that was changed via the pointer!
```

Assigning to `*pa` will change the value stored at the memory address `pa` holds. Since `pa` holds
the memory address of `a`, by assigning to `*pa` we are effectively changing the value of `a`!

A note of caution however: always check if a pointer is not `nil` before dereferencing.
Dereferencing a `nil` pointer will make the program crash at runtime!

```go
var p *int // p is nil initially
fmt.Println(*p)
// panic: runtime error: invalid memory address or nil pointer dereference
```

### Pointers to structs

So far we've only seen pointers to primitive values. We can also create pointers for structs:

```go
type Person struct {
    Name string
    Age  int
}

var peter Person
peter = Person{Name: "Peter", Age: 22}

var p *Person
p = &peter
```

We could have also created a new `Person` and immediately stored a pointer to it:

```go
var p *Person
p = &Person{Name: "Peter", Age: 22}
```

When we have a pointer to a struct, we don't need to dereference the pointer before accessing one of
the fields:

```go
var p *Person
p = &Person{Name: "Peter", Age: 22}

fmt.Println(p.Name) // Output: "Peter"
                    // Go automatically dereferences 'p' to allow
                    // access to the 'Name' field
```

## Slices and maps are already pointers

Slices and maps are special types because they already have pointers in their implementation. This
means that more often than not, we don't need to create pointers for these types to share the memory
address for their values. Imagine we have a function that increments the value of a key in a map:

```go
func incrementPeterAge(m map[string]int) {
	m["Peter"] += 1
}
```

If we create a map and call this function, the changes the function made to the map persist after
the function ended. This is a similar behavior we get if we were using a pointer, but note how on
this example we are not using any referencing/dereferencing or any of the pointer syntax:

```go
ages := map[string]int{
  "Peter": 21
}
incrementPeterAge(ages)
fmt.Println(ages)
// Output: map[Peter:22]
// The changes the function 'incrementPeterAge' made to the map are visible after the function ends!
```

The same applies when changing an existing item in a slice.

However, actions that return a new slice like `append` are a special case and **might not** modify
the slice outside of the function. This is due to the way slices work internally, but we won't cover
this in detail in this exercise, as this is a more advanced topic. If you are really curious you can
read more about this in [Go Blog: Mechanics of 'append'][mechanics-of-append]

[mechanics-of-append]: https://go.dev/blog/slices
