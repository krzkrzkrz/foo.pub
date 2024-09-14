# Arrays

The type `[n]T` is an array of `n` values of type `T`

The expression declares a variable `a` as an array of ten integers:

```go
var a [10]int
```

An array's length is part of its type, so arrays cannot be resized. This seems limiting,
but don't worry; Go provides a convenient way of working with arrays

```go
func main() {
  var a [2]string
  a[0] = "Hello"
  a[1] = "World"
  // a[10] = "World" // Will result in an error: invalid array index 10 (out of bounds for 2-element array)

  fmt.Println(a[0], a[1]) // Prints: Hello World
  fmt.Println(a)          // Prints: [Hello World]

  primes := [6]int{2, 3, 5, 7, 11, 13}
  fmt.Println(primes)     // Prints: [2 3 5 7 11 13]
}
```

