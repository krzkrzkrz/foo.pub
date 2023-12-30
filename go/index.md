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

# Pointers

Go has pointers. A pointer holds the memory address of a value.

The type `*T` is a pointer to a `T` value. Its zero value is `nil.`

```go
var p *int
```

The `&` operator generates a pointer to its operand.

```go
i := 42
p = &i
```

The `*` operator denotes the pointer's underlying value.

```go
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

This is known as "dereferencing" or "indirecting".

# Structs

A `struct` is a collection of fields.

```go
type Vertex struct {
  X int
  Y int
}

func main() {
  fmt.Println(Vertex{1, 2})
}
```

`Struct fields` are access using a dot

```go
func main() {
  v := Vertex{1, 2}
  v.X = 4
  fmt.Println(v.X)
}
```

# Pointers to structs

Struct fields can be accessed through a struct pointer.  
To access the field `X` of a struct when we have the struct pointer p we could write `(*p).X`. However,
that notation is cumbersome, so the language permits us instead to write just `p.X`, without the explicit dereference.

```go
type Vertex struct {
  X int
  Y int
}

func main() {
  v := Vertex{1, 2}
  p := &v
  p.X = 1e9
  fmt.Println(v)
}
```

# Struct Literals

A struct literal denotes a newly allocated struct value by listing the values of its fields.  
You can list just a subset of fields by using the `Name:` syntax. (And the order of named fields is irrelevant)  
The special prefix `&` returns a pointer to the struct value.

```go
type Vertex struct {
  X, Y int
}

var (
  v1 = Vertex{1, 2}  // has type Vertex
  v2 = Vertex{X: 1}  // Y:0 is implicit
  v3 = Vertex{}      // X:0 and Y:0
  p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
  fmt.Println(v1, p, v2, v3)
}
```

Returns: `{1 2} &{1 2} {1 0} {0 0}`

# Arrays

The type `[n]T` is an array of `n` values of type `T`

The expression declares a variable `a` as an array of ten integers:

```go
var a [10]int
```

An array's length is part of its type, so arrays cannot be resized. This seems limiting, but don't worry; Go provides a convenient way of working with arrays.

```go
func main() {
  var a [2]string
  a[0] = "Hello"
  a[1] = "World"
  fmt.Println(a[0], a[1]) // Prints: Hello World
  fmt.Println(a)          // Prints: [Hello World]

  primes := [6]int{2, 3, 5, 7, 11, 13}
  fmt.Println(primes)     // Prints: [2 3 5 7 11 13]
}
```

# Slices

An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

The type `[]T` is a slice with elements of type `T`

The following expression creates a slice which includes elements 1 through 3 of `a`:

```go
a[1:4]
```

For example:

```go
func main() {
  primes := [6]int{2, 3, 5, 7, 11, 13}
  var s []int = primes[1:4]
  fmt.Println(s) // Prints: [3 5 7]
}
```

Slices are like references to arrays  
A slice does not store any data, it just describes a section of an underlying array  
Changing the elements of a slice modifies the corresponding elements of its underlying array

```go
func main() {
  names := [4]string{
    "John",
    "Paul",
    "George",
    "Ringo",
  }
  fmt.Println(names) // Prints: [John Paul George Ringo]

  a := names[0:2]
  b := names[1:3]
  fmt.Println(a, b)  // Prints: [John Paul] [Paul George]

  b[0] = "XXX"
  fmt.Println(a, b)  // Prints: [John XXX] [XXX George]
  fmt.Println(names) // Prints: [John XXX George Ringo]
}
```

# Slice literals

A slice literal is like an array literal without the length

```go
func main() {
  q := []int{2, 3, 5, 7, 11, 13}
  fmt.Println(q)
}
```

# Slice defaults

For the array

```go
var a [10]int

// These slice expressions are equivalent:

a[0:10]
a[:10]
a[0:]
a[:]
```

# Slice length and capacity

A slice has both a length and a capacity.  
The length of a slice is the number of elements it contains.  
The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.  
The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`.

```go

func main() {
  s := []int{2, 3, 5, 7, 11, 13}
  printSlice(s) // Prints: len=6 cap=6 [2 3 5 7 11 13]

  // Slice the slice to give it zero length.
  s = s[:0]
  printSlice(s) // Prints: len=0 cap=6 []

  // Extend its length.
  s = s[:4]
  printSlice(s) // Prints: len=4 cap=6 [2 3 5 7]

  // Drop its first two values.
  s = s[2:]
  printSlice(s) // Prints: len=2 cap=4 [5 7]
}

func printSlice(s []int) {
  fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

# Creating a slice with make

The make function allocates a zeroed array and returns a slice that refers to that array:

```go
a := make([]int, 5)  // len(a)=5
```

To specify a capacity, pass a third argument to make:

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

# Range

The `range` form of the `for` loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
  for i, v := range pow {
    fmt.Printf("2**%d = %d\n", i, v)
  }
}
```

Returns:

```
2**0 = 1
2**1 = 2
2**2 = 4
2**3 = 8
2**4 = 16
2**5 = 32
2**6 = 64
2**7 = 128
```

You can skip the `index` or value by assigning to `_`

```go
for i, _ := range pow
for _, value := range pow
```

If you only want the index, you can omit the second variable.

```go
for i := range pow
```

# Maps

A `map` maps keys to values.

The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added.  
The make function returns a map of the given type, initialized and ready for use.

```go
type Vertex struct {
  Lat, Long float64
}

var m map[string]Vertex

func main() {
  m = make(map[string]Vertex)
  m["Bell Labs"] = Vertex{
    40.68433, -74.39967,
  }
  fmt.Println(m["Bell Labs"]) // Returns: {40.68433 -74.39967}
}
```

# Map literals

Map literals are like struct literals, but the keys are required

```go
type Vertex struct {
  Lat, Long float64
}

var m = map[string]Vertex{
  "Bell Labs": Vertex{
    40.68433, -74.39967,
  },
  "Google": Vertex{
    37.42202, -122.08408,
  },
}

func main() {
  fmt.Println(m) // Returns: map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]
}
```

If the top-level type is just a type name, you can omit it from the elements of the literal:

```go
var m = map[string]Vertex{
  "Bell Labs": {40.68433, -74.39967},
  "Google":    {37.42202, -122.08408},
}
```

# Mutating maps

Insert or update an element in map m:

```go
m[key] = elem
```

Retrieve an element:

```go
elem = m[key]
```

Delete an element:

```go
delete(m, key)
```

Test that a key is present with a two-value assignment:

```go
elem, ok = m[key]
```

If `key` is in `m`, `ok` is `true.` If not, `ok` is `false`.

If `key` is not in the map, then `elem` is the zero value for the map's element type.

```go
func main() {
  m := make(map[string]int)

  m["Answer"] = 42
  fmt.Println("The value:", m["Answer"]) // Prints: The value: 42

  m["Answer"] = 48
  fmt.Println("The value:", m["Answer"]) // Prints: The value: 48

  delete(m, "Answer")
  fmt.Println("The value:", m["Answer"]) // Prints: The value: 0

  v, ok := m["Answer"]
  fmt.Println("The value:", v, "Present?", ok) // Prints: The value: 0 Present? false
}
```

# Function values

Functions are values too. They can be passed around just like other values.  
Function values may be used as function arguments and return values.

# Methods

Go does not have classes. However, you can define methods on types.  
A method is a function with a special **receiver** argument.  
The receiver appears in its own argument list between the `func` keyword and the method name.

In this example, the `Abs` method has a receiver of type `Vertex` named `v`:

```go
type Vertex struct {
  X, Y float64
}

// (v Vertex) is the receiver
func (v Vertex) Abs() float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
  v := Vertex{3, 4}
  fmt.Println(v.Abs())
}
```

# Methods are functions

> Remember: a method is just a function with a receiver argument.

Here's Abs written as a regular function with no change in functionality.

```go
type Vertex struct {
  X, Y float64
}

func Abs(v Vertex) float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
  v := Vertex{3, 4}
  fmt.Println(Abs(v))
}
```

# Interfaces

An interface type is defined as a set of method signatures.

```go
type Abser interface {
  Abs() float64
}
```

Under the hood, interface values can be thought of as a tuple of a value and a concrete type:

```go
(value, type)
```

Calling a method on an interface value executes the method of the same name on its underlying type

# Type switches

A type switch is a construct that permits several type assertions in series.

```go
switch v := i.(type) {
case T:
  // Here v has type T
case S:
  // Here v has type S
default:
  // No match; here v has the same type as i
}
```



































TODO continue from here https://go.dev/tour/concurrency/1

