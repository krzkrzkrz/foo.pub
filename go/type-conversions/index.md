# Type conversions

* The expression `T(v)` converts the value `v` to the type `T`.

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

Or simple:

```go
i := 42
f := float64(i)
u := uint(f)
```

