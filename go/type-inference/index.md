# Type inference

* When declaring a variable without specifying an explicit type (either by using the `:=` syntax
  or var `=` expression syntax), the variable's type is inferred from the value on the right
  hand side

When the right hand side of the declaration is typed, the new variable is of that same type:

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

