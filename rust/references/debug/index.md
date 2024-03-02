# Debug

* `{:?}` or `{:#?}`. `{:#?}`, is known as "pretty printing"
* `{}` `Display` print. More types have `Debug` than `Display,` so if a type you want to print can't print with `Display,` you can try `Debug.`
* `{:?}` `Debug` print. If there is too much information on one line, you can try `{:#?}`.
* `{:#?}` `Debug` print, but pretty. Pretty means that each part of a type is printed on its own line to make it easier to read.

Given:

```rust
User { name: "Mr. User", user_number: 101 }
```

`{:?}` will look like this:

```rust
User { name: "Mr. User", user_number: 101
```

`{:#?}` will look more like this, over more lines:

```rust
User {
    name: "Mr. User",
    user_number: 101,
}
```

```rust
fn main() {
    print!("This will not print a new line");
    println!(" so this will be on the same line");
}
```
