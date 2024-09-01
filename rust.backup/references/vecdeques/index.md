# VecDeques

* `VecDeques` is a `Vec` that is optimized for popping items both off the front and the back

Take for example, the following code will take over a minute to run:


```rust
fn main() {
    let mut my_vec = vec![0; 600_000];
    for _ in 0..600000 {
        my_vec.remove(0); // Remove the first element
    }
}
```

With a `VecDeque`, the same code will run in a fraction of the time:

```rust
use std::collections::VecDeque;

fn main() {
    let mut my_vec = VecDeque::from(vec![0; 600000]);
    for i in 0..600000 {
        my_vec.pop_front();
    }
}
```

