# Tuples

* Grouping together some number of other values with a variety of types
* Tuples are mainly used as a sort of minimal-drama struct type. i.e. to store width and height
* Tuples allow constants as indices, like `t.4`. You cant write `t.i` or `t[i]` to get the `i`'th element
* Are constructed using a `()`

```rust
let tup: (&str, i32, f64, u8) = ("foo", 500, 6.4, 1);

// Or
let tup = ("foo", 500, 6.4, 1);
let (x, y, z, w) = tup;

println!("{}", tup.0) // Returns foo
```
