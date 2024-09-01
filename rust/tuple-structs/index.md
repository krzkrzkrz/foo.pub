# Tuple structs

* Like structs, but dont have names associated with their fields
* They just have the types of the fields

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);

println!("{}, {}", black.0, origin.0); // Returns: 0, 0
```

