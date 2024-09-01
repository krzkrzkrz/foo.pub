# Loops

* Rust provides a `loop` keyword to indicate an infinite loop
* The `break` statement can be used to exit a loop at anytime
* The `continue` statement can be used to skip the rest of the iteration
```rust
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");

    // Infinite loop
    loop {
        count += 1;

        if count == 3 {
            println!("three");

            // Skip the rest of this iteration
            continue;
        }

        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");

            // Exit this loop
            break;
        }
    }
}
```
Outputs:
```
Let's count until infinity!
1
2
three
4
5
OK, that's enough
```
## Returning from loops

* Put the value after the `break`, and it will be returned by the loop expression
```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("Exited the loop, counter: {}", result); // Exited the loop, counter: 20
}
```

