# Constants

Two different types of `constants`:

1. `const`: An unchangeable value (the common case)
2. `static`: A possibly mutable variable with `'static` lifetime. The static lifetime is  
   inferred and does not have to be specified. Accessing or modifying a mutable static
   variable is `unsafe`

```rust
// Globals are declared outside all other scopes.
const THRESHOLD: i32 = 10;
static LANGUAGE: &str = "Rust";
```

Almost always, if you can choose between the two, choose `const.` It’s pretty rare that you
actually want a memory location associated with your constant, and using a `const` allows for
optimizations like constant propagation not only in your crate but downstream crates.

