# Go Journey

[Go](https://go.dev) is a statically typed, compiled high-level programming language designed at Google

## Suggested learning approach

[A tour of go](https://go.dev/welcome/1)

## Install Go

```shell
brew install go
```

## Packages

Every Go program is made up of packages.

Programs start running in package main.

This program is using the packages with import paths "fmt" and "math/rand".

By convention, the package name is the same as the last element of the import path. For instance, the "math/rand" package comprises files that begin with the statement package rand.

```go
package main

import (
  "fmt"
  "math/rand"
)

func main() {
  fmt.Println("My favorite number is", rand.Intn(10))
}
```

## Imports

This code groups the imports into a parenthesized, **factored** import statement.

```go
import (
  "fmt"
  "math"
)
```

You can also write multiple import statements, like:

```go
import "fmt"
import "math"
```

But it is good style to use the **factored** import statement.

## Exported names

In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the math package.

```go
fmt.Println(math.Pi)
```

## Functions

A function can take zero or more arguments.  
In this example, add takes two parameters of type int.

```go
func add(x int, y int) int {
  return x + y
}

func main() {
  fmt.Println(add(42, 13))
}
```

When two or more consecutive named function parameters share a type, you can omit the type from all but the last.

```go
func add(x int, y int) int {
  return x + y
}
```

```go
func add(x, y int) int {
  return x + y
}
```

## Multiple results

A function can return any number of results.  
The swap function returns two strings.

```go
func swap(x, y string) (string, string) {
  return y, x
}

func main() {
  a, b := swap("hello", "world")
  fmt.Println(a, b)
}
```

## Named return values

Go's return values may be named. If so, they are treated as variables defined at the top of the function.  
These names should be used to document the meaning of the return values.  
A return statement without arguments returns the named return values. This is known as a "naked" return.  
Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return y,x
}

func main() {
  fmt.Println(split(17))
}
```

## Variables

The `var` statement declares a list of variables; as in function argument lists, the type is last.  
A var statement can be at package or function level. We see both in this example.

```go
var c, python, java bool

func main() {
  var i int
  fmt.Println(i, c, python, java)
}
```

With initializers:

```go
var i, j int = 1, 2

func main() {
  var c, python, java = true, false, "no!"
  fmt.Println(i, j, c, python, java)
}
```

## Short variable declarations

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

Outside a function, every statement begins with a keyword (var, func, and so on) and so the `:=` construct is not available.

```go
func main() {
  var i, j int = 1, 2
  k := 3 // This is a short variable declaration
  c, python, java := true, false, "no!"

  fmt.Println(i, j, k, c, python, java)
}
```

## Basic types

The `int` `uint`, and `uintptr` types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems.  
When you need an integer value you should use `int` unless you have a specific reason to use a sized or unsigned integer type.

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

The example shows variables of several types, and also that variable declarations may be "factored" into blocks, as with import statements.

```go

var (
  ToBe   bool       = false
  MaxInt uint64     = 1<<64 - 1
  z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
  fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)
  fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)
  fmt.Printf("Type: %T Value: %v\n", z, z)
}
```

# Zero values

Variables declared without an explicit initial value are given their zero value.

The zero value is:

- `0` for numeric types,
- `false` for the boolean type, and
- `""` (the empty string) for strings.

```go
func main() {
  var i int
  var f float64
  var b bool
  var s string
  fmt.Printf("%v %v %v %q\n", i, f, b, s)
}
```

# Type conversions

The expression `T(v)` converts the value `v` to the type `T`.

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

Or simple:

```go
i := 42
f := float64(i)
u := uint(f)
```

# Type inference

When declaring a variable without specifying an explicit type (either by using the := syntax or var = expression syntax),  
the variable's type is inferred from the value on the right hand side.

When the right hand side of the declaration is typed, the new variable is of that same type:

```go
var i int
j := i // j is an int
```

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

# Constants

Constants are declared like variables, but with the `const` keyword.  
Constants can be character, string, boolean, or numeric values.  
Constants cannot be declared using the := syntax.

```go

const Pi = 3.14

func main() {
  const World = "世界"
  fmt.Println("Hello", World)
  fmt.Println("Happy", Pi, "Day")

  const Truth = true
  fmt.Println("Go rules?", Truth)
}
```

# Numeric constants

Numeric constants are high-precision values.

```go
const (
  // Create a huge number by shifting a 1 bit left 100 places.
  // In other words, the binary number that is 1 followed by 100 zeroes.
  Big = 1 << 100
  // Shift it right again 99 places, so we end up with 1<<1, or 2.
  Small = Big >> 99
)
```

# For loop

Go has only one looping construct, the `for` loop.  
The basic for loop has three components separated by semicolons:

- The `init` statement: executed before the first iteration
- The `condition expression`: evaluated before every iteration
- The `post` statement: executed at the end of every iteration

```go
func main() {
  sum := 0
  for i := 0; i < 10; i++ {
    sum += i
  }
  fmt.Println(sum)
}
```

The `init` and `post` are optional:

```go
func main() {
  sum := 1
  for sum < 1000 {
    sum += sum
  }
  fmt.Println(sum)
}
```

**Forever**: If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.

```go
func main() {
  for {
  }
}
```

# If statements

Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses `( )` but the braces `{ }` are required.

```go
func sqrt(x float64) string {
  if x < 0 {
    return sqrt(-x) + "i"
  }
  return fmt.Sprint(math.Sqrt(x))
}

func main() {
  fmt.Println(sqrt(2), sqrt(-4))
}
```

# If with a short statement

Like for, the if statement can start with a short statement to execute before the condition.  
Variables declared by the statement are only in scope until the end of the if.

```go
func add(x, y int) int {
  if x := x + 10; x < y {
    return x
  }
  return 0
}

func main() {
  fmt.Println(
    add(3, 20),
    add(3, 10),
  )
}
```

Returns: `13 0`

# Switch statements

A switch statement is a shorter way to write a sequence of if - else statements.  
It runs the first case whose value is equal to the condition expression.

```go
func main() {
  fmt.Print("Go runs on ")
  switch os := runtime.GOOS; os {
  case "darwin":
    fmt.Println("OS X.")
  case "linux":
    fmt.Println("Linux.")
  default:
    // freebsd, openbsd, plan9, windows...
    fmt.Printf("%s.\n", os)
  }
}
```

Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

```go
func main() {
  fmt.Println("When's Saturday?")
  today := time.Now().Weekday()
  switch time.Saturday {
  case today + 0:
    fmt.Println("Today.")
  case today + 1:
    fmt.Println("Tomorrow.")
  case today + 2:
    fmt.Println("In two days.")
  default:
    fmt.Println("Too far away.")
  }
}
```

Swith without a condition

```go
func main() {
  t := time.Now()
  switch {
  case t.Hour() < 12:
    fmt.Println("Good morning!")
  case t.Hour() < 17:
    fmt.Println("Good afternoon.")
  default:
    fmt.Println("Good evening.")
  }
}
```

# Defer

A defer statement defers the execution of a function until the surrounding function returns.  
The deferred call's arguments are evaluated immediately, but the function call is not executed  
until the surrounding function returns.

```go
func main() {
  defer fmt.Println("world")
  fmt.Println("hello")
}
```

Returns:

```
hello
world
```

Stacking defers: Deferred function calls are pushed onto a stack. When a function returns,  
its deferred calls are executed in last-in-first-out order.

```go
func main() {
  fmt.Println("counting")

  for i := 0; i < 10; i++ {
    defer fmt.Println(i)
  }

  fmt.Println("done")
}
```

Returns:

```
counting
done
9
8
7
6
5
4
3
2
1
0
```



TODO continue from here https://go.dev/tour/moretypes/1

