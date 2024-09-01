# Methods

* Go does not have classes. However, you can define methods on types
* A method is a function with a special **receiver** argument
* The receiver appears in its own argument list between the `func` keyword and the method name

In this example, the `fullName` method has a **receiver** of type `Person` named `p`:

```go
package main

import "fmt"

// Define a Person struct
type Person struct {
	firstName string
	lastName  string
}

// Method to get the full name of the person
// (p Person) is the "receiver"
func (p Person) fullName() string {
	return p.firstName + " " + p.lastName
}

func main() {
	// Create a new Person
	person := Person{
		firstName: "John",
		lastName:  "Doe",
	}

	// Call the method to get the full name
	fullName := person.fullName()

	fmt.Println("Full Name:", fullName) // Prints: Full Name: John Doe
}
```

## Methods are functions

* A method is just a function with a receiver argument
* Here, `fullName` is written as a regular function with no change in functionality

```go
package main

import "fmt"

// Define a Person struct
type Person struct {
	firstName string
	lastName  string
}

// Method to get the full name of the person
func fullName(p Person) string {
	return p.firstName + " " + p.lastName
}

func main() {
	// Create a new Person
	person := Person{
		firstName: "John",
		lastName:  "Doe",
	}

	// Call the method to get the full name
	fullName := fullName(person)

	fmt.Println("Full Name:", fullName) // Prints: Full Name: John Doe
}
```

## Methods with pointer receivers

* You can declare methods with pointer receivers

```go
package main

import "fmt"

// Define a Person struct
type Person struct {
	firstName string
	lastName  string
}

// Method that has a receiver of type Person named p
func (p Person) fullName() string {
	return p.firstName + " " + p.lastName
}

// Method to change the first name of the person using a pointer receiver
func (p *Person) updateFirstName(newFirstName string) {
	p.firstName = newFirstName
}

func main() {
	// Create a new Person
	person := Person{
		firstName: "John",
		lastName:  "Doe",
	}

	fmt.Println("Full name:", person.fullName()) // Prints: Full Name: John Doe

	// Call the method to change the first name
	person.updateFirstName("Jane")
	fmt.Println("Updated first name:", person.firstName) // Prints: Updated First Name: Jane
}
```





