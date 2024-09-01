# Options

* You use `Option` when something might or might not exist
* When a value exists, it is `Some(value)` and when it doesn't, it is `None`
* `Option` is actually an `enum` (defined by the standard library):

  ```rust
  enum Option<T> {
      None,
      Some(T),
  }
  ```

## An example

* We define an `optional_number` variable of type `Option<i32>`
* We then use a `match` statement to match against the `optional_number`
* When `optional_number` is `Some(value)`, we print out the value contained within it
* When `optional_number` is `None`, we print out a message indicating that there is no value

```rust
fn main() {
    // Define an Option that can hold an integer value
    let optional_number: Option<i32> = Some(5);

    // Match the optional_number to handle both Some and None cases
    match optional_number {
        // Do something with the value if it's Some
        Some(value) => {
            println!("The value is: {}", value);
        }
        // Handle the case where the value is None
        None => {
            println!("There is no value!");
        }
    }
}
```

## is_some() and is_none()

```rust
fn main() {
    // Define an Option that can hold an integer value
    let optional_number: Option<i32> = Some(5);

    if optional_number.is_some() {
        println!("The value is: {}", optional_number.unwrap());
    }

    if optional_number.is_none() {
        println!("There is no value!");
    }
}
```
