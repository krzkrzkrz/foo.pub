# inspect

* Similar to `dbg!`, but it is used in iterators

```rust
fn main() {
    let new_vec = vec![8, 9, 10];

    let double_vec = new_vec
        .iter()
        .inspect(|first_item| println!("The item is: {first_item}"))
        .map(|x| x * 2)
        .inspect(|next_item| println!("Then it is: {next_item}"))
        .collect::<Vec<i32>>();

    // Prints:
    // The item is: 8
    // Then it is: 16
    // The item is: 9
    // Then it is: 18
    // The item is: 10
    // Then it is: 20
}
```
