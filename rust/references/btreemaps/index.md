# BTreeMaps

* If you want a `HashMap` that gives you its keys in order
* The signature of a `BTreeMap` is the same as a `HashMap`

```rust
// This is so we can just write BTreeMap instead of std::collections::BTreeMap every time
use std::collections::BTreeMap;

struct City {
    name: String,
    // This will have the year and the population for the year
    population: BTreeMap<i32, i32>,
}

fn main() {
    let mut tallinn = City {
        name: "Tallinn".to_string(),
        // So far the BTreeMap is empty
        population: BTreeMap::new(),
    };

    // Just so we remember, there is no difference between 24_000 and 24000. The _ is just for readability
    tallinn.population.insert(2020, 437_619);
    tallinn.population.insert(1372, 3_250);
    tallinn.population.insert(1851, 24_000);

    // The BTreeMap is BTreeMap<i32, i32>, so it returns two items each time
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

