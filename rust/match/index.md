# Match

* Compare a value against a series of patterns and then execute code based on which
  pattern matches
* All possible values must be covered (i.e. using `_ => ...`)

**Example 1:**

```rust
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

fn main() {
    println!("{}", get_string(Some(1))); // Returns "one"
}
```

**Example 2:**

```rust
fn main() {
    let number: i32 = 10;

    match number {
        0..=10 => println!("Number between 0 and 10: {}", number),
        11.. => println!("Number 11 and above: {}", number),
        _ => println!("{:?}", "Number is negative".to_string()),
    } // Prints "Number between 0 and 10: 10"
}
```
