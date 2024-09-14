# Stringers

* `Stringer` interface from the `fmt` package is used to define how a type should be printed
  as a string
* If a type implements this interface, the fmt package (e.g., `fmt.Println`) will call the
  `String()` method when printing the type

It contains a single method:

```go
type Stringer interface {
    String() string
}
```

## Example

```go
// Define a struct
type Person struct {
    Name string
    Age  int
}

// Implement the Stringer interface for Person
// The Person struct implements the Stringer interface by providing a String() method
func (p Person) String() string {
    return fmt.Sprintf("%s is %d years old", p.Name, p.Age)
}

func main() {
    // Create an instance of Person
    person := Person{Name: "Alice", Age: 30}

    // When fmt.Println(person) is called, Go automatically calls the String() method to get
    // the string representation of person and prints: Alice is 30 years old
    fmt.Println(person) // Output: Alice is 30 years old
}
```

