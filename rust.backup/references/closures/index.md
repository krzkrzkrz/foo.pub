# Closures

* Closures are quick functions that don't need a name
* Sometimes they are called `lambdas` in other languages
* They use `||` instead of `()`

```rust
fn main() {
    let my_closure = |x: i32| println!("{x}");
    my_closure(5); // Prints:
    my_closure(5 + 5); // Prints: 10
}
```

## Longer closures

* Can take variables from their environment that are *outside* the closure

```rust
fn main() {
    let twenty = 20;

    // This closure can be as long as we want, just like a function
    let my_closure = |x: i32| {
        let number = 7;
        let other_number = 10;
        println!(
            "The two numbers are {number} and {other_number}. Also {}",
            number + twenty + x
        ); // The two numbers are 7 and 10. Also 32
    };

    my_closure(5);
}
```

## Closures inside of methods

```rust
fn main() {
    // Here |num| is a closure inside a method
    (1..=3).for_each(|num| println!("{num}"));
    (1..=3).for_each(|num| {
        println!("Got a {num}!");

        if num % 2 == 0 {
            println!("It's even")
        } else {
            println!("It's odd")
        };
        // Prints:
        // 1
        // 2
        // 3
        // Got a 1!
        // It's odd
        // Got a 2!
        // It's even
        // Got a 3!
        // It's odd
    });
}
```

## Examples of std methods that come with closures

Take for example the variations of `.unwrap()`. Nothing fancy here with `.unwrap_or()`:

```rust
fn main() {
    let nothing: Option<i32> = None;
    println!("{}", nothing.unwrap_or(0)); // Prints 0
}
```

However, `unwrap_or_else()` can accept a closure:

```rust
fn main() {
    let my_vec = vec![8, 9, 10];

    // First, we try to get an item at index 3.
    let fourth = my_vec.get(8).unwrap_or_else(|| {
        // If it doesn't work, maybe we have a good reason to look for
        // an item one index back. Inside the closure we can try .get() again!
        // And then return that value if it's found at index 2.
        //
        // index 2, is the value with: 10
        if let Some(val) = my_vec.get(2) {
            val
        } else {
            &0
        }
    });

    // The output is 10 because there was no item at index 8
    println!("{fourth}"); // Prints: 10
}
```

## Closures: Lazy and fast

One interesting thing about `.map()` is that it doesn't do anything unless you use a method like `.collect()`

```rust
fn main() {
    let num_vec = vec![2, 4, 6];

    // Takes num_vec
    let double_vec: Vec<i32> = num_vec
        .iter() // Makes into an iterator
        .map(|num| num * 2) // Multiplies each item by 2 and passes it on
        .collect(); // And collects into a new Vec

    println!("{:?}", double_vec); // Prints: [4, 8, 12]
}
```

Without `.collect()` the code above would not do anything. "Iterators are lazy and do nothing unless consumed" means

```rust
fn main() {
    let num_vec = vec![2, 4, 6];
    num_vec
        .iter() // Transformed into a Iter<i32>
        .enumerate() // Now it is an Enumerate<Iter<i32>>, so it is a type Enumerate of type Iter of i32s
        .map(|(index, num)| format!("Index {index} is {num}")); // Now it is a type Map<Enumerate<Iter<i32>>>, so it is a type Map of type Enumerate of type Iter of i32s.
}
```

## |_| in a closure

* Is a closure that takes no argument

```rust
fn main() {
    let my_vec = vec![8, 9, 10];
    my_vec
        .iter()
        // Without |_|, the compiler would give an error because the closure would expect a parameter
        .for_each(|_| println!("We didn't use the variables at all"));
        // Prints:
        // We didn't use the variables at all
        // We didn't use the variables at all
        // We didn't use the variables at all
}
```

