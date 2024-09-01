# Strings

* Rust has two main types of strings: `String` and `&str`

## &str

* A `&str` is a simple string
* Commonly pronounced `string slice` or `ref-stir`
* When you write `let my_ variable = "Hello, world!"`. Rust can see
  where it starts and where it ends
* Typically stored on the stack
* The data it refers to (the string itself) is stored in the heap.
  So, while the reference (&str) is stored on the stack, the data
  it points to is stored on the heap. This design allows for efficient
  memory management and manipulation of strings.

## String

* A `String` is easy to grow, shrink, mutate, and so on
* It may be a bit slower, but it has more functionality
* A `String` is a pointer with data on the heap

## Conversions

Possible ways to convert `&str` into a `String`:

1. **String::from()**

```rust
String::from("Adrian Fahrenheit Țepeș");
```

2. **to_string()**

```rust
"Adrian Fahrenheit Țepeș".to_string();
```
3. **format!()**

```rust
format!("Adrian Fahrenheit Țepeș");
```

4. **into()**

* `.into()`, is a bit different because `.into()` isn’t for making a string; it's for
  converting from one type into another type.
* Some types can easily convert to and from another type using `From::` and `.into()`
* From is clearer because you already know the types: you know that `String::from("Some str")`
  is a `String` from a `&str`. But with `.into()` sometimes the compiler doesn't know:

5. In methods

* If you have a `String` and pass a reference to it (a `&String`), Rust will convert it to a `&str` when you need it.
* This is thanks to a trait called `Deref`

```rust
fn prints_str(my_str: &str) {
    println!("{my_str}"); // Prints: I am a string
}

fn main() {
    let my_string = String::from("I am a string");
    prints_str(&my_string);
}
```
