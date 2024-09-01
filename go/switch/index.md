# Switch

A switch statement is a shorter way to write a sequence of if - else statements.  
It runs the first case whose value is equal to the condition expression.

```go
func main() {
  fmt.Print("Go runs on ")

  switch os := runtime.GOOS; os {
  case "darwin":
    fmt.Println("OS X.")
  case "linux":
    fmt.Println("Linux.")
  default:
    // freebsd, openbsd, plan9, windows...
    fmt.Printf("%s.\n", os)
  }
}
```

Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

```go
func main() {
  fmt.Println("When's Saturday?")
  today := time.Now().Weekday()

  switch time.Saturday {
  case today + 0:
    fmt.Println("Today.")
  case today + 1:
    fmt.Println("Tomorrow.")
  case today + 2:
    fmt.Println("In two days.")
  default:
    fmt.Println("Too far away.")
  }
}
```

Swith without a condition

```go
func main() {
  t := time.Now()
  switch {
  case t.Hour() < 12:
    fmt.Println("Good morning!")
  case t.Hour() < 17:
    fmt.Println("Good afternoon.")
  default:
    fmt.Println("Good evening.")
  }
}
```

