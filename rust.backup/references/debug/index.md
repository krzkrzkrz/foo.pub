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

# Tabs and new lines

`\t` and `\n` are escape sequences that represent tabs and new lines, respectively.

```rust
print!("\t Start with a tab\nand move to a new line");
```

Returns:

```
     Start with a tab
and move to a new line
```

# Raw

* Sometimes you end up using too many escape characters and just want Rust to print a
  string as you see it. To do this, you can add `r#` to the beginning and `#` to the end

```rust
fn main() {
    // We had to use \ eight times here—kind of annoying
    println!("He said, \"You can find the file at c:\\files\\my_documents\\file.txt.\" Then I found the file.");

    // Much better!
    println!(r#"He said, "You can find the file at c:\files\my_documents\file.txt." Then I found the file."#);
}
```
