# Modules

* `mod` keyword declares a new module
* By default, functions, types, constants, and modules are private. The `pub` keyword makes an item public and therefore visible outside its namespace.
* The `use` keyword brings modules, or the definitions inside modules, into scope

```console
cargo new communicator --lib // Creates a new module
cd communicator
```

Notice: Cargo generated src/lib.rs instead of src/main.rs

### Sample module (in `src/lib.rs`, multiple modules with the function name `connect`)

* Call `server::connect` or `client::connect` respectively

```rust
mod server {
    fn connect() {}
}

mod client {
    fn connect() {}
}
```


