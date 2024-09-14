# Functions: Associated functions

* To call this associated function, we use the `::` syntax with the struct name
* Define functions within `impl` blocks that don't take `self` as a parameter

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
    println!("{:#?}", sq); // Returns: Rectangle { width: 3, height: 3 }
}
```

