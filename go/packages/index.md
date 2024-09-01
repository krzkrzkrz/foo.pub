# Packages

Every Go program is made up of packages.

Programs start running in package main.

This program is using the packages with import paths "fmt" and "math/rand".

By convention, the package name is the same as the last element of the import path. For instance, the "math/rand" package comprises files that begin with the statement package rand.

```go
package main

import (
  "fmt"
  "math/rand"
)

func main() {
  fmt.Println("My favorite number is", rand.Intn(10))
}
```

## Imports

This code groups the imports into a parenthesized, **factored** import statement.

```go
import (
  "fmt"
  "math"
)
```

You can also write multiple import statements, like:

```go
import "fmt"
import "math"
```

But it is good style to use the **factored** import statement.

## Exported names

In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the math package.

```go
fmt.Println(math.Pi)
```

