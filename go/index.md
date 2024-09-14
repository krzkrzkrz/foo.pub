# Go Journey

[Go](https://go.dev) is a statically typed, compiled high-level programming language designed at Google

## Suggested learning approach

* [A tour of go](https://go.dev/tour/welcome/1)

* [Naming conventions](naming-conventions/index.md)
* [Installation](installation/index.md)
* [Creating a new project](creating-a-new-project/index.md)
* [Packages](packages/index.md)
* [Functions](functions/index.md)
* [Variables](variables/index.md)
* [Basic types](basic-types/index.md)
* [Zero values](zero-values/index.md)
* [Type conversions](type-conversions/index.md)
* [Type inference](type-inference/index.md)
* [Constants](constants/index.md)
* [For loop](for-loop/index.md)
* [If](if/index.md)
* [Else](else/index.md)
* [Switch](switch/index.md)
* [Defer](defer/index.md)
* [Poiners](poiners/index.md)
* [Structs](structs/index.md)
* [Arrays](arrays/index.md)
* [Slices](slices/index.md)
* [Range](range/index.md)
* [Map](map/index.md)
* [Function values](function-values/index.md)
* [Function closures](function-closures/index.md)
* [Methods](methods/index.md)

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

