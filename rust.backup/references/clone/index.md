# Clone

Some types dont have the `Copy` trait, but they have the `Clone` trait.  
An example of such a type is `String`.

The documentation for `String`: https://doc.rust-lang.org/std/string/struct.String.html

* `String` does not have a `Copy` trait, instead it has a `Clone` trait
* `Clone` is similar to `Copy` but usually needs more memory. Also, you have
  to call it with `.clone()`. It won't clone just by itself in the same
  way that `Copy` types copy themselves all on their own

The following will return an error (type `std::string::String`, which does not implement the `Copy` trait):

```rust
fn prints_country(country_name: String) {
    println!("{country_name}");
}

fn main() {
    let country = String::from("Kiribati");
    prints_country(country);
    prints_country(country);
}
```

This works using `clone()`:

```rust
// This function takes ownership of a String.
fn get_length(input: String) {
    println!("It's {} words long.",
    // Here we split to count the number of words. (Whitespace
    // means the space between words.)
    input.split_whitespace().count());
}

fn main() {
    let mut my_string = String::new();
    for _ in 0..50 {
        my_string.push_str("Here are some more words ");
        get_length(my_string.clone());
    }
}
```

That's 50 clones. Here it is using a reference instead, which is better.  
If the String is very large, .clone() can use a lot of memory  
Instead of 50 clones, it's zero:

```rust
fn get_length(input: &String) {
    println!("It's {} words long.", input.split_whitespace().count());
}

fn main() {
    let mut my_string = String::new();
    for _ in 0..50 {
        my_string.push_str("Here are some more words ");
        get_length(&my_string);
    }
}
```

> **Rule of thumb** with references and functions: If you can use an immutable reference,
> go with that. You won't have to worry about a function taking ownership of some
> data. The function will simply take a look at it and be done


