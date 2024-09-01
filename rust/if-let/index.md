# If let

* For some use cases, when matching `enums`, `match` is awkward. For example:

```rust
// Make `optional` of type `Option<i32>`
let optional = Some(7);

match optional {
    Some(i) => {
        println!("This is a really long string and `{:?}`", i);
        // ^ Needed 2 indentations just so we could destructure
        // `i` from the option.
    },
    _ => {},
    // ^ Required because `match` is exhaustive. Doesn't it seem
    // like wasted space?
};
```
`if let` is cleaner for this use case and in addition allows various failure options to
be specified:
```rust
fn main() {
    let some_value = Some(7);
    //let some_value: Option<i32> = None; // Try uncommenting this

    // if let destructures some_value into Some(i), and then evaluates the block (i.e. {})
    if let Some(i) = some_value {
        println!("The value is: {}", i);
    } else {
        println!("No value found");
    }
}
```

Output:

```
The value is: 7
```

## let else

* `if let` can also be combined with an else, via `let else`

```rust
fn main() {
    let some_value: Option<i32> = None;

    // if let destructures some_value into Some(i), and then evaluates the block (i.e. {})
    let Some(x) = some_value else {
        println!("No value found"); // Returns: No value found
        return;
    };

    println!("The value is: {}", x); // This line will not be reached
}
```
