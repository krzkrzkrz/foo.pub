# Type switches

* A type switch checks the underlying type of an interface value and allows you to perform
  different actions depending on the type

```go
func main() {
	var i interface{} = 42 // Change this to different types for testing

	switch v := i.(type) {
	case int:
		fmt.Printf("i is an int: %d\n", v)
	case string:
		fmt.Printf("i is a string: %s\n", v)
	case bool:
		fmt.Printf("i is a bool: %t\n", v)
	default:
		fmt.Printf("i is of an unknown type\n")
	} // Prints: i is an int: 42
}
```

