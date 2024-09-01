# BinaryHeaps

* Keeps the item with the greatest value in the front, but the other items are in any order
* Some languages call this a `priority queue`

```rust
use std::collections::BinaryHeap;

fn main() {
    // Note that these numbers are in order. They won't be in the same order
    // once we put them inside our BinaryHeap, though.
    let many_numbers = vec![0, 5, 10, 15, 20, 25, 30];
    let mut heap = BinaryHeap::new();

    for num in many_numbers {
        heap.push(num);
    }

    println!("First item is largest, others are out of order: {heap:?}");

    // The.pop() method returns Some(number) if a number is there, and None if not. It
    // pops from the front, which is where the item with the greatest value is.
    while let Some(num) = heap.pop() {
        println!("Popped off {num}. Remaining numbers are: {heap:?}");
    }
}
// Prints:
// First item is largest, others are out of order: [30, 15, 25, 0, 10, 5, 20]
// Popped off 30. Remaining numbers are: [25, 15, 20, 0, 10, 5]
// Popped off 25. Remaining numbers are: [20, 15, 5, 0, 10]
// Popped off 20. Remaining numbers are: [15, 10, 5, 0]
// Popped off 15. Remaining numbers are: [10, 0, 5]
// Popped off 10. Remaining numbers are: [5, 0]
// Popped off 5. Remaining numbers are: [0]
// Popped off 0. Remaining numbers are: []
```

