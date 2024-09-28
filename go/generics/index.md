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

## Another example

* if you wanted to count how many times a particular value appears in a `[]string` slice and an
  `[]int` slice you would need to write two separate functions — one function for the `[]string`
  type and another for the `[]int`

```go
// Count how many times the value v appears in the slice s.
func countString(v string, s []string) int {
    count := 0
    for _, vs := range s {
        if v == vs {
            count++
        }
    }
    return count
}

func countInt(v int, s []int) int {
    count := 0
    for _, vs := range s {
        if v == vs {
            count++
        }
    }
    return count
}
```

With generics, you can write a single function that works with any type:

```go
func count[T comparable](v T, s []T) int {
    count := 0
    for _, vs := range s {
        if v == vs {
            count++
        }
    }
    return count
}
```
