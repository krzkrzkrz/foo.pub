# References and borrowing

A note on references:

* `country` is a `String` and owns the data `"Austria"`
* `ref_one` and `ref_two` can look at the data owned by `country`

No issues here:

```rust
fn main() {
    let country = String::from("Austria");
    let ref_one = &country;
    let ref_two = &country;

    println!("{}", ref_one);
    println!("{}", ref_two);
}
```

However take a look at the next example.

* `return_str()` returns a reference to a `String` that is owned by `country`
* But the `String` called `country` only lives inside the function, and then it dies
* A variable only lives as long as its code block
* After the function returns, `country_ref` would be referring to memory that is already gone

This would return an error:

```rust
fn return_str() -> &String {
    let country = String::from("Austria");
    let country_ref = &country;
    country_ref
}
fn main() {
    let country = return_str();
}
```

