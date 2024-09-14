# Functions

```rust
fn main() {
    println!("Hello, world!");  // Returns Hello, world!
    println!("{}", add_one(5)); // Returns 6
    println!("{:?}", foobar(5)); // Returns 6
}

// Type annotated
// Expects a 32 bit unsigned (negative or positive number)
// Returns a 32 bit unsigned integer
fn add_one(x: u32) -> u32 {
    x + 1
}

// Functions that don't return a value, actually return the unit type `()`
// In which case, the return type can be omitted from the signature
// In its simplest form, can be re-written inseas as: fn foobar(_x: u32) {
fn foobar(x: u32) -> () {
    println!("foobar");
}
```

