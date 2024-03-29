# Results

* `Results` is similar to `Option`
* `Option` holds `Some` and `None`; `Results` holds `Ok` and `Err` variants
* `Result` is actually an `enum` (defined by the standard library):

  ```rust
  enum Result<T, E> {
      Ok(T),
      Err(T),
  }
  ```

  `Ok` holds a generic type `T`, and `Err` holds a generic type `E`

* Often, you see `Result` and `Option` being used at the same time

  For example, you might want to get data from a server. First, you use
  a function to connect. The connection might fail, so that's a `Result`.
  And after connecting, there might not be any data. That's an `Option`.

  So the entire operation would be an `Option` inside a `Result`: a `Result<Option<T>>`

## is_ok() and is_err()

`Result<T, E>` means you need to think of what you want to return for `Ok` and what you want to return for `Err`.  
In fact, you can return anything you like. Even returning a `()` in each case is okay:

```rust
fn check_error() -> Result<(), ()> {
    match 1 == 2 {
        true => Ok(()),
        false => Err(()),
    }
}

fn main() {
    if check_error().is_ok() {
        println!("Its Ok!");
    } else {
        println!("Its and error");
    }
}
```

## Result with a string return type

```rust
fn check_if_five(number: i32) -> Result<i32, String> {
    match number {
        5 => Ok(number),
        _ => Err(format!("Sorry, bad number. Expected: 5 Got: {number}")),
    }
}

fn main() {
    for number in 4..=7 {
        println!("{:?}", check_if_five(number));
    }
}
// Returns:
// Err("Sorry, bad number. Expected: 5 Got: 4")
// Ok(5)
// Err("Sorry, bad number. Expected: 5 Got: 6")
// Err("Sorry, bad number. Expected: 5 Got: 7")
```
