# Defer

A defer statement defers the execution of a function until the surrounding function returns.  
The deferred call's arguments are evaluated immediately, but the function call is not executed  
until the surrounding function returns.

```go
func main() {
  defer fmt.Println("world")
  fmt.Println("hello")
}
```

Returns:

```
hello
world
```

Stacking defers: Deferred function calls are pushed onto a stack. When a function returns,  
its deferred calls are executed in last-in-first-out order.

```go
func main() {
  fmt.Println("counting")

  for i := 0; i < 10; i++ {
    defer fmt.Println(i)
  }

  fmt.Println("done")
}
```

Returns:

```
counting
done
9
8
7
6
5
4
3
2
1
0
```


