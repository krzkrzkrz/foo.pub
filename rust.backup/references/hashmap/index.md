# HashMap

* A collection made out of *keys* and *values*
* An example of a `key` and a `value` is `email` and `my_email@address.com`
* The keys of a HashMap are not ordered

```rust
// This is so we can just write HashMap instead of std::collections::HashMap every time
use std::collections::HashMap;

struct City {
    name: String,
    // This will have the year and the population for the year
    population: HashMap<i32, i32>,
}

fn main() {
    let mut tallinn = City {
        name: "Tallinn".to_string(),
        // So far the HashMap is empty
        population: HashMap::new(),
    };

    // Just so we remember, there is no difference between 24_000 and 24000. The _ is just for readability
    tallinn.population.insert(2020, 437_619);
    tallinn.population.insert(1372, 3_250);
    tallinn.population.insert(1851, 24_000);

    // The HashMap is HashMap<i32, i32>, so it returns two items each time
    for (year, population) in tallinn.population {
        println!(
            "In {year}, {} had a population of {population}",
            tallinn.name
        );
    }
    // Prints:
    // In 1851, Tallinn had a population of 24000
    // In 1372, Tallinn had a population of 3250
    // In 2020, Tallinn had a population of 437619
}
```
