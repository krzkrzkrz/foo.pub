# HashSets

* A `HashSet` is actually just a `HashMap` that only has keys
* The keys of a `HashSet` are not ordered
* Use a `BtreeSet` if you need the keys to be ordered

```rust
use std::collections::HashSet;

fn main() {
    // Create a new HashSet
    let mut fruits = HashSet::new();

    // Insert some elements into the HashSet
    fruits.insert("Apple");
    fruits.insert("Banana");
    fruits.insert("Orange");

    // Check if an element exists in the HashSet
    let search = "Apple";

    match fruits.contains(search) {
        true => println!("Yes, {} is in the set!", search),
        false => println!("No, {} is not in the set.", search),
    }

    // Remove an element from the HashSet
    fruits.remove("Banana");

    // Iterate over the HashSet and print its elements
    println!("Remaining fruits in the set:");

    for fruit in fruits {
        println!("{}", fruit);
    }
}
// Prints
// Yes, Apple is in the set!
// Remaining fruits in the set:
// Orange
// Apple
```
