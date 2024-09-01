# Options

* Rust does not have `nulls`
* Rust has an `enum` that can encode the concept of a value being present or absent

It is defined by the standard library as:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

Dont need to bring it into scope explicitly. You can use `Some` and `None` directly without
the `Option::` prefix:

```rust
let some_number = Some(5);
let some_string = Some("a string");
let absent_number: Option<i32> = None;

println!("{some_number:?}"); // Prints: Some(5)
println!("{some_string:?}"); // Prints: Some("a string")
println!("{absent_number:?}"); // Prints: None
```

