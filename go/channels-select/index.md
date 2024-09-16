# Channels: Select

* `select` statement allows you to wait on multiple channel operations
* Its similar to a `switch` statement, but it's used specifically with channels

```go
package main

import (
    "fmt"
    "time"
)

func sendMessage(ch chan string) {
    time.Sleep(2 * time.Second)
    ch <- "Hello from Goroutine!"
}

func sendAnotherMessage(ch chan string) {
    time.Sleep(2 * time.Second)
    ch <- "Another message from Goroutine!"
}

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    // Start Goroutines to send messages
    go sendMessage(ch1)
    go sendAnotherMessage(ch2)

    // Use select to wait for any channel to receive data
    select {
    case msg1 := <-ch1:
        fmt.Println("Received:", msg1)
    case msg2 := <-ch2:
        fmt.Println("Received:", msg2)
    } // Will either print msg1 or msg2, whichever is received first
}
```
