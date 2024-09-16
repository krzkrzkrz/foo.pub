# Channels: Default

* The `select` statement can have a default case, which runs when none of the other channels are
  ready for communication

```go
package main

import (
	"fmt"
	"time"
)

func sendData(ch chan string) {
	time.Sleep(2 * time.Second)
	ch <- "Data from Goroutine"
}

func main() {
	ch := make(chan string)

	// Start a Goroutine to send data
	go sendData(ch)

	// Add a delay to allow time for the Goroutine to send data
  // Uncomment the following line to see the non-default case in execute
	// time.Sleep(3 * time.Second)

	// Use select with a default case to avoid blocking
	select {
	case msg := <-ch:
		fmt.Println("Received:", msg)
	default:
		// The select statement tries to receive from ch. Since the Goroutine hasn't sent data
		// yet (due to the 2-second delay), the default case executes immediately.
		fmt.Println("No data available yet, doing something else.")
	}

	// Give enough time for the Goroutine to finish before the program exits
	time.Sleep(3 * time.Second)
}
```

