# Type assertions

* Type assertions in Go are used to extract the underlying value of the interface

```go
t := i.(T)
```

* `t` will hold the value of `i` with the type `T` if the assertion is successful
* `i` is an interface variable
* `T` is the type you are asserting i to be
```go
func main() {
	// i is an empty interface (interface{})
  // This assigns the string "Hello" to the variable i
  // Since i is of type interface{}, it can hold any type, and in this case, it holds a string
	var i interface{} = "hello"

	// This is type assertion. It checks whether the value stored in the interface i is
	// of type string
	s := i.(string)
	fmt.Println(s) // Prints: hello

	// The type assertion returns two values:
	//   s: The value of i as a string (if the assertion succeeds).
	//   ok: A boolean (true or false) indicating whether the assertion succeeded.
	s, ok := i.(string)
	fmt.Println(s, ok) // Prints: hello true

  // If the value stored in i is not a string, ok will be false
	f, ok := i.(float64)
	fmt.Println(f, ok) // Prints: 0 false

  // This will panic because the value stored in i is a string, not a float64
	//  f = i.(float64)
	// fmt.Println(f)
}
```

