# Iters: Iter mut

**iter_mut** - This mutably borrows each element of the collection, allowing for the collection to be  
modified in place

```rust
fn main() {
    let mut names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }

    println!("names: {:?}", names);
}
```

Output:

```
names: ["Hello", "Hello", "There is a rustacean among us!"]
```

