# Variables

The `var` statement declares a list of variables; as in function argument lists, the type is last.  
A var statement can be at package or function level. We see both in this example.

```go
var c, python, java bool

func main() {
  var i int
  fmt.Println(i, c, python, java)
}
```

With initializers:

```go
var i, j int = 1, 2

func main() {
  var c, python, java = true, false, "no!"
  fmt.Println(i, j, c, python, java)
}
```

## Short variable declarations

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

Outside a function, every statement begins with a keyword (var, func, and so on) and so the `:=` construct is not available.

```go
func main() {
  var i, j int = 1, 2
  k := 3 // This is a short variable declaration
  c, python, java := true, false, "no!"

  fmt.Println(i, j, k, c, python, java)
}
```


