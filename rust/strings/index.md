# Strings

* In Rust, strings are a more complicated data structure, than is apparent in other
  programming languages

## String concatenation
```rust
let hello = "hello";
let world = "world";

println!("{} {}", hello, world);
```
## Strings and characters
```rust
// This is a string with length of 1
"a"

// This is a string with length of 3
"abc"

// This is a single character
'a'

// An empty string
""

// Error: Must contain 1 character
'abc'

// Error: Must contain 1 character
''
```
### More on strings
```rust
fn main() {
    let mut s1 = "foo".to_string(); // Creates a String
    s1.push_str("bar"); // Appends bar

    // Create a String from a string literal
    let mut s2 = String::from("initial contents");

    // Creates a new, empty String
    let mut s3 = String::new();

    // Creates a reference to a String (&str)
    // Returns error: no method named push_str found for type &str in the current scope
    let mut s4 = "foo";
    s4.push_str("bar");

    // Above must be converted to a String first, before using push_str
    let s5 = "foo";
    let mut s6 = s5.to_string();
    s6.push_str("ba"); // Appends ba
    s6.push('r'); // Appends a single character
    println!("{}", s6); // Returns "foobar"
}
```

