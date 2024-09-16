# Errors

* The `error` type is a built-in interface similar to `fmt.Stringer`
* A `nil` error denotes success; a `non-nil` error denotes failure

```go
type error interface {
    Error() string
}
```
**TODO** Needs a simple/good example

