# Range

* The `range` form of the `for` loop iterates over a slice or map

When ranging over a slice, two values are returned for each iteration. The first is the index,
and the second is a copy of the element at that index

```go
package main

import "fmt"

var array = []int{1, 2, 3, 4, 5}

func main() {
	for i, v := range array {
		fmt.Println("Index:", i, "Value:", v+i)
		// Prints:
		// Index: 0 Value: 1
		// Index: 1 Value: 3
		// Index: 2 Value: 5
		// Index: 3 Value: 7
		// Index: 4 Value: 9
	}
}
```

You can skip the `index` or `value` by assigning to `_`

```go
for i, _ := range array
for _, value := range array
```

If you only want the index, you can omit the second variable.

```go
for i := range array
```

