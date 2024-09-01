# Function values

* Functions are values too. They can be passed around just like other values
* Function values may be used as function arguments and return values

```go
package main

import "fmt"

// Function that takes two integers and returns their sum
func add(a, b int) int {
	return a + b
}

// Function that takes two integers and returns their difference
func subtract(a, b int) int {
	return a - b
}

func main() {
	// Assigning functions to variables
	operation1 := add
	operation2 := subtract

	// Using the variables which hold functions
	result1 := operation1(5, 3) // This will call the add function
	result2 := operation2(8, 2) // This will call the subtract function

	fmt.Println("Result of addition:", result1)    // Prints: Result of addition: 8
	fmt.Println("Result of subtraction:", result2) // Prints: Result of subtraction: 6

	// Passing functions as arguments to another function
	result3 := performOperation(10, 4, add)     // Passing the add function
	result4 := performOperation(7, 2, subtract) // Passing the subtract function

	fmt.Println("Result of custom addition operation:", result3)    // Prints: Result of custom addition operation: 14
	fmt.Println("Result of custom subtraction operation:", result4) // Prints: Result of custom subtraction operation: 5
}

// Function that takes two integers and a function as arguments,
// and applies the function to the integers
func performOperation(a, b int, operation func(int, int) int) int {
	return operation(a, b)
}
```


