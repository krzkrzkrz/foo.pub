# Constants and statics

* `const` is for values that don’t change and are created at compile time.
* `static`: A possibly mutable variable with `'static` lifetime. The static lifetime is  
  inferred and does not have to be specified. Accessing or modifying a mutable static variable is `unsafe`.
* Write them with ALL CAPITAL LETTERS and usually outside of `main` so that they can live for the whole program

```rust
// Globals are declared outside all other scopes.
const NUMBER_OF_MONTHS: u32 = 12;
static SEASONS: [&str; 4] = ["Spring", "Summer", "Fall", "Winter"];
```

Almost always, if you can choose between the two, choose `const.` It’s pretty rare that you actually want a  
memory location associated with your constant, and using a `const` allows for optimizations like constant  
propagation not only in your crate but downstream crates.

