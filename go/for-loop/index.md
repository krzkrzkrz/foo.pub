# For loop

Go has only one looping construct, the `for` loop.  
The basic for loop has three components separated by semicolons:

- The `init` statement: executed before the first iteration
- The `condition expression`: evaluated before every iteration
- The `post` statement: executed at the end of every iteration

```go
func main() {
  sum := 0
  for i := 0; i < 10; i++ {
    sum += i
  }
  fmt.Println(sum)
}
```

The `init` and `post` are optional:

```go
func main() {
  sum := 1
  for sum < 1000 {
    sum += sum
  }
  fmt.Println(sum)
}
```

**Forever**: If you omit the loop condition it loops forever, so an infinite loop
is compactlyexpressed.

```go
func main() {
  for {
  }
}
```

