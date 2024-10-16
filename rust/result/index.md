# Result

* Functions that have a positive and negative path typically return a `Result` type

**Example: 1**

```rust
fn main() {
    let ok_result: Result<i32, ()> = Ok(42);
    let err_result: Result<(), String> = Err("Error".to_string());

    println!("{:?}", ok_result); // Prints: Ok(42)
    println!("{:?}", err_result); // Prints: Err("Error")
}
```

**Example: 2**

```rust
fn is_adult(age: i32) -> Result<String, String> {
    match age {
        _ if age >= 18 => Ok("Is an adult".to_string()),
        _ => Err("Not an adult".to_string()),
    }
}

fn main() {
    let age: i32 = 21;

    match is_adult(age) {
        Ok(message) => println!("{}", message),
        Err(err) => println!("{}", err),
    } // Prints: Is an adult
}
```

## map_err

* The `map_err` method is used to change the error type of a `Result`

```rust
fn main() {
    let result: Result<String, String> = Ok("good".to_string());
    let new_result = result.map_err(|err| err.to_uppercase());
    println!("{:?}", new_result); // Prints: Ok("good")

    let result: Result<i32, String> = Err("error".to_string());
    let new_result = result.map_err(|err| err.to_uppercase());
    println!("{:?}", new_result); // Prints: Err("ERROR")
}

