# Slices

* An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view
  into the elements of an array. In practice, slices are much more common than arrays
* The type `[]T` is a slice with elements of type `T`
  This selects a half-open range which includes the first element, but excludes the last one. Also
  known as an `exclusive range`
* Think of it as `[inclusive:exclusive]T`

The following expression creates a slice which includes elements 1 through 3 of `a`:

```go
a[1:4]
```

For example:

```go
func main() {
	array := [6]int{1, 2, 3, 4, 5}
	var s []int = array[1:4]
	fmt.Println(s) // Prints: [2 3 4]. 5 is not included
}
```

* Slices are like references to arrays
* A slice does not store any data, it just describes a section of an underlying array
* Changing the elements of a slice modifies the corresponding elements of its underlying array

```go
func main() {
  names := [4]string{
    "John",
    "Paul",
    "George",
    "Ringo",
  }
  fmt.Println(names) // Prints: [John Paul George Ringo]

  a := names[0:2]
  b := names[1:3]
  fmt.Println(a, b)  // Prints: [John Paul] [Paul George]

  b[0] = "XXX"
  fmt.Println(a, b)  // Prints: [John XXX] [XXX George]
  fmt.Println(names) // Prints: [John XXX George Ringo]
}
```

## Slice literals

* A slice literal is like an array literal without the length

```go
func main() {
  q := []int{2, 3, 5, 7, 11, 13}
  fmt.Println(q)
}
```

## Slice defaults

For the array

```go
var a [10]int

// These slice expressions are equivalent:

a[0:10]
a[:10]
a[0:]
a[:]
```

## Appending to a slice

* You can append to a slice using the `append()` built-in function

```go
package main

import "fmt"

func main() {
	var s []int
	fmt.Println(s)

	s = append(s, 0)
	s = append(s, 1)
	s = append(s, 2, 3, 4)
	fmt.Println(s) // Prints: [0 1 2 3 4]
}
```

## Slice length and capacity

* A slice has both a length and a capacity
* The length of a slice is the number of elements it contains
* The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice
* The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`

```go
func main() {
  s := []int{2, 3, 5, 7, 11, 13}
  printSlice(s) // Prints: len=6 cap=6 [2 3 5 7 11 13]

  // Slice the slice to give it zero length.
  s = s[:0]
  printSlice(s) // Prints: len=0 cap=6 []

  // Extend its length.
  s = s[:4]
  printSlice(s) // Prints: len=4 cap=6 [2 3 5 7]

  // Drop its first two values.
  s = s[2:]
  printSlice(s) // Prints: len=2 cap=4 [5 7]
}

func printSlice(s []int) {
  fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

# Creating a slice with make

The make function allocates a zeroed array and returns a slice that refers to that array:

```go
a := make([]int, 5)  // len(a)=5
```

To specify a capacity, pass a third argument to make:

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

