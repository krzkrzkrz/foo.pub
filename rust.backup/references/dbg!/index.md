# dbg! macro

* A good alternative to `println!` because it is faster to type and gives more information

```rust
fn main() {
    let my_number = 8;
    dbg!(my_number); // Returns: [src/main.rs:3:5] my_number = 8
}
```

You can place `dbg!` anywhere in your code:

```rust
fn main() {
    let mut my_number = dbg!(9); //  Retuns: [src/main.rs:3:25] my_number = 9

    dbg!(my_number += 10); // Returns: [src/main.rs:3:5] my_number += 10 = ()

    let new_vec = dbg!(vec![8, 9, 10]);
    // Returns:
    // [src/main.rs:4:19] vec![8, 9, 10] = [
    //     8,
    //     9,
    //     10,
    // ]

    let double_vec = dbg!(new_vec.iter().map(|x| x * 2).collect::<Vec<i32>>());
    // Returns:
    // [src/main.rs:5:22] new_vec.iter().map(|x| x * 2).collect::<Vec<i32>>() = [
    //     16,
    //     18,
    //     20,
    // ]

    dbg!(double_vec); // Returns:
                      // [src/main.rs:6:5] double_vec = [
                      //     16,
                      //     18,
                      //     20,
                      // ]
}
```

