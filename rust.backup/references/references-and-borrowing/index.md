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

## References and the dot operator

The following code would return an error, because `i32` and `&i32` are different types.  
Its like comparing `i32 == &i32`

```rust
fn main() {
    let my_number = 9;
    let reference = &my_number;
    println!("{}", my_number == reference); // Returns an error
}
```

However, the following code would work, since `reference` is *dereferenced* to an `i32` and then compared to `my_number`:

```rust
fn main() {
    let my_number = 9;
    let reference = &my_number;
    println!("{}", my_number == *reference); // Returns true
}
```

Understanding above. Now take the following example:

```rust
fn main() {
    let my_name = "Billy".to_string();
    let single_ref = &my_name;
    let double_ref = &&my_name; // A double reference
    println!("{}", single_ref.is_empty()); // Returns false
    println!("{}", double_ref.is_empty()); // Returns false
}
```

The method `.is_empty()` is for the `String` type, but we called it on a `&String`.  
This works, because the `.` operator will *dereference* for you until it reaches the original type

Meaning, when you use the `.` operator on a reference, you dont need to worry about `*`


