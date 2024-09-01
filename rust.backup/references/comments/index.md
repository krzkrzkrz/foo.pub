# Comments

* `//` and `/* */` are used for comments
* `///` is used for documentation comments
* Running `cargo doc --open` will generate documentation for your project and open it in your browser

For example:

```rust
// If you see /// (three slashes), that’s a “doc comment” (documentation comment)
// A doc comment can be automatically made into documentation for your code. Documentation
// is used to explain how code works, usually for other people to read, but it can be good
// for you, too, so you won’t forget. All the information on documentation pages like
// http://doc.rust-lang.org/std/index.html is made with doc comments
// For example:
//
/// Converts a string slice in a given base to an integer. Leading and
/// trailing whitespace represent an error.
fn main() {
    // Rust programs start with fn main()
    // You put the code inside a block. It starts with { and ends with }
    let some_number = 100; // We can write as much as we want here and the compiler won't look at it }

    // To the compiler, let some_number/*: i16*/ = 100; looks like let some_number = 100;
    let some_number/*: i16*/ = 100;
```
