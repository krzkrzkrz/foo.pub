# Channels

* Channels are used to communicate between Goroutines safely
* Allow you to send and receive data between Goroutines, making it easier to synchronize
  their execution
* Are not exclusively used with Goroutines, but they are primarily designed to facilitate
  communication and synchronization between Goroutines
* Channels are **blocking** by nature. When sending or receiving on a channel, the operation
  waits until the other side (either the sender or receiver) is ready. To avoid this, channels
  are typically used with Goroutines to enable concurrent sending and receiving

```go
package main

import (
    "fmt"
    "time"
)

func sendData(ch chan string) {
    time.Sleep(1 * time.Second) // Simulate some work
    ch <- "Hello from Goroutine!" // Send data to the channel
}

func main() {
    // Create a channel of type string
    ch := make(chan string)

    // Start a Goroutine to send data
    go sendData(ch)

    // Receive data from the channel
    message := <-ch

    fmt.Println(message)  // Prints: Hello from Goroutine!
}
```

