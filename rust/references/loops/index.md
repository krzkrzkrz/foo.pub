# Loops

Rust provides a `loop` keyword to indicate an infinite loop.

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

## Nesting and labels

Loops must be annotated with some `'label`

```rust
#![allow(unreachable_code, unused_labels)]

fn main() {
    'outer: loop {
        println!("Entered the outer loop");

        'inner: loop {
            println!("Entered the inner loop");

            // This would break only the inner loop
            //break;

            // This breaks the outer loop
            break 'outer;
        }

        println!("This point will never be reached");
    }

    println!("Exited the outer loop");
}
```

Output:

```
Entered the outer loop
Entered the inner loop
Exited the outer loop
```

## Returning from loops

Put the value after the `break`, and it will be returned by the loop expression

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

## While loop

The `while` keyword can be used to run a loop while a condition is true.

```rust
fn main() {
    // A counter variable
    let mut n = 1;

    // Loop while `n` is less than 101
    while n < 101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }

        // Increment counter
        n += 1;
    }
}
```

## For loop

The `for in` construct can be used to iterate through an `Iterator` (i.e. `a..b`)

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

Instead of an **exclusive range**, `a..b`.  
Alternatively, `a..=b` can be used for an **inclusive range**:

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100. Instead of 1, 2, ..., 99 in each iteration
    for n in 1..=100 {
        println!("N is: {}", n);
    }
}
```

If you dont need a variable name, you can use `_` to ignore the value:

```rust
fn main() {
    for _ in 0..3 {
        println!("Printing the same thing three times");
    }
}
```

## iter, into_iter, iter_mut

* `iter`, `into_iter`  and `iter_mut` all handle the conversion of a collection into an iterator in different ways
* `for` loop will apply the `into_iter`

**iter** - This borrows each element of the collection through each iteration. Thus leaving the collection  
untouched and available for reuse after the loop


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

**into_iter** - This consumes the collection so that on each iteration the exact data is provided. Once the  
collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop

```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.into_iter() {
        match name {
            "Ferris" => println!("There is a rustacean among us!"),
            _ => println!("Hello {}", name),
        }
    }

    println!("names: {:?}", names); // Returns an error, because "names" was already consumed by match
}
```

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

