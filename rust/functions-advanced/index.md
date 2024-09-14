# Functions: Advanced

* This technique is useful when you want to pass a function you've already defined rather
  than defining a new closure

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
    // ^ and ^ is equivalent to: add_one(arg) + add_one(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);
    println!("The answer is: {}", answer); // Returns: The answer is: 12
}
```

