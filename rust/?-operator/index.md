# ? operator

* The `?` operator will take in a `Result`,
* If `Ok`, then the result (unwrapped) will be returned
* If `Err`, error will `return`

```rust
fn get_number(number: i32) -> Result<i32, String> {
    match number {
        _ if number >= 0 => Ok(number),
        _ => Err("Number is negative".to_string()),
    }
}

fn main() -> Result<(), String> {
    let number = get_number(-5)?;

    // These lines are never reached
    println!("Number: {}", number);
    Ok(())
}
```
