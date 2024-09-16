# sync.Mutex

* `channels` are great for communication among goroutines. But what if we just want to make sure
  only one goroutine can access a variable at a time to avoid conflicts
* Essential to avoid race conditions when multiple Goroutines are accessing or modifying shared
  data concurrently
* This concept is called **mutual exclusion**, and the conventional name for the data structure
  that provides it is `mutex`
* Go's standard library provides **mutual exclusion** with `sync.Mutex` and its two methods:

  * `Lock()`
  * `Unlock()`

## Without a Mutex

```go
package main

import (
	"fmt"
	"sync"
)

var counter int // Shared variable

func increment(wg *sync.WaitGroup) {
	// Increment the counter without any synchronization
	for i := 0; i < 1000; i++ {
		counter++
	}
	wg.Done() // Signal that the Goroutine is done
}

func main() {
	var wg sync.WaitGroup

	// Start 5 Goroutines that will increment the counter
	for i := 0; i < 5; i++ {
		wg.Add(1)
		go increment(&wg)
	}

	wg.Wait() // Wait for all Goroutines to finish
	fmt.Println("Final counter value:", counter) // Prints random number: Final counter value: 3216
}
```

## With a Mutex

```go
package main

import (
    "fmt"
    "sync"
)

var (
    counter int       // Shared variable
    mu      sync.Mutex // Mutex to protect the counter
)

func increment(wg *sync.WaitGroup) {
    mu.Lock()          // Lock the mutex before accessing the counter
    counter++
    mu.Unlock()        // Unlock the mutex after the counter is updated
    wg.Done()          // Signal that the Goroutine is done
}

func main() {
    var wg sync.WaitGroup

    // Start 5 Goroutines that will increment the counter
    for i := 0; i < 5; i++ {
        wg.Add(1)
        go increment(&wg)
    }

    wg.Wait() // Wait for all Goroutines to finish
    fmt.Println("Final counter value:", counter) // Final counter value: 5
}
```

