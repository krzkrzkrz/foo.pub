# Interfaces

* An interface type is defined as a set of method signatures

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

## Example

```go
// Define an interface with one method
type Speaker interface {
	Speak() string
}

// Define a type that implements the Speaker interface
type Dog struct{}

func (d Dog) Speak() string {
	return "Woof!"
}

// Another type implementing the Speaker interface
type Cat struct{}

func (c Cat) Speak() string {
	return "Meow!"
}

func main() {
	// Create instances of Dog and Cat
	var speaker Speaker

	speaker = Dog{}
	fmt.Println(speaker.Speak()) // Prints: Woof!

	speaker = Cat{}
	fmt.Println(speaker.Speak()) // Prints: Meow!
}
```
