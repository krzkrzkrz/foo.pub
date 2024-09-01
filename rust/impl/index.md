# Impl

* Implement some functionality for a type
* The `impl` keyword is primarily used to define implementations on types
* `trait` implementations are used to implement traits for types, or other traits
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // Method to calculate the area of the rectangle
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect = Rectangle {
        width: 30,
        height: 50,
    };
    println!(
        "The area of the rectangle is {} square pixels.",
        rect.area()
    ); // The area of the rectangle is 1500 square pixels.
}
```

