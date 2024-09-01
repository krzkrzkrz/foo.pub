# Enums

* Values can only be one of the variants
* Each variant can have different types and amounts of associated data.

There are 3 types of enums.

## 1. Tuple structs, which are, basically, named tuples
```rust
// A tuple struct
struct Pair(i32, f32);
   ```
 ## 2. The classic C structs
```rust
// A C struct
#[derive(Debug)]
struct Person {
   name: String,
   age: u8,
   x: f32,
   y: f32,
}
   ```
## 3. Unit structs, which are field-less, are useful for generics
```rust
// A unit struct
struct Unit;
```
`enum` in its simplest form:
```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

fn main() {
    let home = IpAddr::V4(127, 0, 0, 1);
    let loopback = IpAddr::V6(String::from("::1"));
}
```
A more complicated structure (with a comparison to a `struct`)
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```
If the above following were to be constructed just using a `struct`, then it would look like:
```rust
struct QuitMessage; // Unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // Tuple struct
struct ChangeColorMessage(i32, i32, i32); // Tuple struct
```
`enums` have the advantage of grouping up several variants into one entity
