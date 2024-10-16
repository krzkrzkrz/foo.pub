# Iters

* `iter`, `into_iter`  and `iter_mut` all handle the conversion of a collection into an
  iterator in different ways
* `for` loop will apply the `into_iter`

**iter** - This borrows each element of the collection through each iteration. Thus leaving the
collection untouched and available for reuse after the loop

```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter() {
        match name {
            &"Ferris" => println!("There is a rustacean among us!"),
            // TODO ^ Try deleting the & and matching just "Ferris"
            _ => println!("Hello {}", name),
        }
    }

    println!("names: {:?}", names); // "names" can be reused here, since match only borrowed the element
}
```

Output:

```
Hello Bob
Hello Frank
There is a rustacean among us!
names: ["Bob", "Frank", "Ferris"]
```

