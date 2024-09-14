# Map

* A `map` maps keys to values
* The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added
* The `make` function returns a map of the given type, initialized and ready for use

```go
package main

import "fmt"

type Coords struct {
	Lat, Long float64
}

var m map[string]Coords

func main() {
	fmt.Println(m) // Returns: map[]

	m = make(map[string]Coords)
	m["Bell Labs"] = Coords{
		40.68433, -74.39967,
	}


	fmt.Println(m["Bell Labs"]) // Returns: {40.68433 -74.39967}
}
```

Another way of writing this:

```go
func main() {
	m := map[string]Coords{
		"Bell Labs": {40.68433, -74.39967},
	}

	fmt.Println(m["Bell Labs"]) // Returns: {40.68433 -74.39967}
}
```

## Map literals

* Map literals are like struct literals, but the keys are required

```go
type Coords struct {
  Lat, Long float64
}

var m = map[string]Coords{
  "Bell Labs": Coords{
    40.68433, -74.39967,
  },
  "Google": Coords{
    37.42202, -122.08408,
  },
}

func main() {
  fmt.Println(m) // Returns: map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]
}
```

If the top-level type is just a type name, you can omit it from the elements of the literal:

```go
var m = map[string]Coords{
  "Bell Labs": {40.68433, -74.39967},
  "Google":    {37.42202, -122.08408},
}
```

## Mutating maps

Insert or update an element in map m:

```go
m[key] = elem
```

Retrieve an element:

```go
elem = m[key]
```

Delete an element:

```go
delete(m, key)
```

Test that a key is present with a two-value assignment:

```go
elem, ok = m[key]
```

If `key` is in `m`, `ok` is `true.` If not, `ok` is `false`.

If `key` is not in the map, then `elem` is the zero value for the map's element type.

```go
package main

import "fmt"

func main() {
	m := make(map[string]int)
	fmt.Println(m) // Prints: map[]

	// Set key/value pairs
	m["Answer"] = 42
	fmt.Println("The value:", m["Answer"]) // Prints: The value: 42

	// Update key/value pairs
	m["Answer"] = 48
	fmt.Println("The value:", m["Answer"]) // Prints: The value: 48

	// Delete key/value pairs
	delete(m, "Answer")
	fmt.Println("The value:", m["Answer"]) // Prints: The value: 0

	// Check if key exists
	v, ok := m["Answer"]
	fmt.Println("The value:", v, "Present?", ok) // Prints: The value: 0 Present? false
}
```

