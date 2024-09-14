# Naming conventions

* **Lowercase function** names are used for unexported (private) functions, which can only be
  accessed within the same package
* **Uppercase function** names are used for exported (public) functions, which can be accessed
  from other packages

## Functions

* **CamelCase** is preferred for multi-word function names, with no underscores
* **Verb-based naming**: Function names should typically be verbs because they perform actions
* **Acronyms**: Acronyms in Go functions should follow CamelCase conventions
  (ie., `HTTPServer` instead of `HttpServer`)

```go
// Private function
func calculateSum(a, b int) int {
    return a + b
}

// Public function
func CalculateSum(a, b int) int {
    return a + b
}
```

## Contstants

```go
// Only visible to the local file
const localFileConstant string = "Constant Value with limited scope"

// Exportable constant
const GlobalConstant string = "Everyone can use this"
```
