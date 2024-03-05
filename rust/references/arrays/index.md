# Arrays

* Collection of similar types
* You cant append new or remove elements
* Are constructed using a `[]`

```rust
let months: [&str; 12] = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
println!("{}", a[1]); // February
```

Or:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
```

## Setting

```rust
let my_array = ["a"; 5];
println!("{:?}", my_array); // Prints ["a", "a", "a", "a", "a"]
```

## Reading elements

```rust
let array_of_ten = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

let two_to_five = &array_of_ten[2..5]; // 2..5 means from index 2 up to index 5 but not including index 5
let start_at_one = &array_of_ten[1..]; // 1.. means from index 1 until the end.
let end_at_five = &array_of_ten[..5]; // ..5 means from the beginning up to but not including index 5
let everything = &array_of_ten[..]; // Means to slice the whole array: beginning to end

println!("Two to five: {two_to_five:?}"); // Prints: Two to five: [2, 3, 4]
println!("Start at one: {start_at_one:?}"); // Prints: Start at one: [1, 2, 3, 4, 5, 6, 7, 8, 9]
println!("End at five: {end_at_five:?}"); // Prints: End at five: [0, 1, 2, 3, 4]
println!("Everything: {everything:?}"); // Prints: Everything: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Because a range like 2..5 doesn’t include index 5, it's called **exclusive**. But you can also
have an **inclusive** range, which means it includes the last number, too:

```rust
let two_to_five = &array_of_ten[2..=5];
```
