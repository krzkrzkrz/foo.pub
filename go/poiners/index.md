# Pointers

* A pointer holds the memory address of a value
* The `&` operator generates a pointer to its operand.
* The `*` operator denotes the pointer's underlying value.

The type `*T` is a pointer to a `T` value. Its zero value is `nil.`

```go
var p *int
```

An example:

```go
func main() {
	var p *int

	i := 42
	p = &i

	// Read i through the pointer p
	fmt.Println(*p) // Prints: 42

	// Set i through the pointer p
	*p = 21 // This is known as "dereferencing" or "indirecting"
	fmt.Println(*p) // Prints: 21
}
```

