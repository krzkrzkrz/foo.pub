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

