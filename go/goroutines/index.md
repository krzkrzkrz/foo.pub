# Goroutines

* A `goroutine` is a lightweight thread managed by the Go runtime

```go
// Starts a new goroutine running: f(x, y, z)
go f(x, y, z)
```

## A simple example

```go
package main

import (
	"fmt"
	"time"
)

func printNumbers() {
	for i := 1; i <= 5; i++ {
		fmt.Println(i)
		time.Sleep(500 * time.Millisecond) // Sleep for 500 milliseconds
	}
}

func main() {
	// Start a Goroutine to print numbers
	go printNumbers() // Print numbers from 1 to 5

	// Wait for the Goroutine to finish
	time.Sleep(10 * time.Second)

	fmt.Println("Finished!") // Print "Finished!", after 10 seconds
}
```

