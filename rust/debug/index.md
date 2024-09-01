# Debug

Replacing the `{}` placeholder with `{:?}` uses the debug placeholder. Any type
that supports debug printing will print a detailed debugging dump of its
contents, rather than just the value.
```rust
println!("{:?}", name);
```

