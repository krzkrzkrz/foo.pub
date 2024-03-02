# Shadowing

* Using let to declare a new variable with the same name as another variable

```rust
fn main() {
    let my_number = 8;
    println!("{}", my_number); // Prints 8

    let my_number = 9.2;
    println!("{}", my_number); // Prints 9.2
}
```

Another example:

```rust
  let x = 1;
  let x = x + 1; // x is 2
  let mut x = x as f32;
  x += 0.5; // x is 2.5
```
