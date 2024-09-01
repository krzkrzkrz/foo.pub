# While let

* Similar to `if let`, `while let` can make awkward match sequences more tolerable
```rust
fn main() {
    let mut values = vec![1, 2, 3, 4];

    // while let destructures values.pop() into Some(x), and then evaluates the block (i.e. {})
    // pop() returns removes the last element from a vector and returns it, or None if it is empty
    while let Some(x) = values.pop() {
        println!("Popped value: {}", x);
        // Returns:
        //   Popped value: 4
        //   Popped value: 3
        //   Popped value: 2
        //   Popped value: 1
    }
}
```
