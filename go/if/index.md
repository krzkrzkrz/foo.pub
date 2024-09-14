# If

* Go's `if` statements are like its `for` loops; the expression need not be surrounded by
  parentheses `( )` but the braces `{ }` are required.

```go
func sqrt(x float64) string {
  if x < 0 {
    return sqrt(-x) + "i"
  }
  return fmt.Sprint(math.Sqrt(x))
}

func main() {
  fmt.Println(sqrt(2), sqrt(-4))
}
```

## If with a short statement

* Like `for`, the `if` statement can start with a short statement to execute before the condition
* Variables declared by the statement are only in scope until the end of the if

```go
func add(x, y int) int {
  if x := x + 10; x < y {
    return x
  }
  return 0
}

func main() {
  fmt.Println(
    add(3, 20),
    add(3, 10),
  ) // Prints: 13 0
}
```

