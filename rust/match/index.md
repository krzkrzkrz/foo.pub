# Match

* Compare a value against a series of patterns and then execute code based on which
  pattern matches
* All possible values must be covered (i.e. using `_ => ...`)
```rust
fn main() {
    println!("{}", get_string(Some(1))); // Returns "one"
}

fn get_string(number: Option<i32>) -> &'static str {
    match number {
        Some(1) => "one",
        Some(2) => "two",
        Some(3) => "three",
        Some(4) => "four",
        Some(5) => "five",
        _ => "Not in the list",
    }
}
```
