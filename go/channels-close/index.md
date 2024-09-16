# Channels: Close

* `close` function is used to close a channel, signaling that no more values will be sent on
  that channel
* Only the `sender` should close a channel, never the `receiver`. Sending on a closed channel
  will cause a panic

```go
package main

import "fmt"

func sendNumbers(ch chan int) {
    for i := 1; i <= 5; i++ {
        ch <- i
    }
    close(ch)  // Close the channel after sending all values
}

func main() {
    ch := make(chan int)

    // Start a Goroutine to send numbers
    go sendNumbers(ch)

    // Receive numbers from the channel until it's closed
    for num := range ch {
        fmt.Println(num) // Prints the received number: 1, 2, 3, 4, 5
    }

    fmt.Println("Channel closed, no more data.")
}
```

