# Generics

* Write functions and types that can work with any data type
* This is done using **type parameters**

```go
// A generic function to swap two values of different types
func Swap[T1, T2 any](a T1, b T2) (T2, T1) {
	return b, a
}

func main() {
	// Swap an int and a string
	val1, val2 := 1, "abc"
	fmt.Println("Before swap:", val1, val2) // Prints: Before swap: 1 abc

	swapped_val1, swapped_val2 := Swap(val2, val2)
	fmt.Println("After swap:", swapped_val1, swapped_val2) // Prints: After swap: abc 1
}
```
