# HashMaps

* A collection made out of *keys* and *values*
* An example of a `key` and a `value` is `email` and `my_email@address.com`
* The keys of a `HashMap` are not ordered
* Use a `BtreeMap` if you need the keys to be ordered

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

## .get()

* Use `.get()` if you are not sure if a key exists
* `.get()` returns an `Option` type

```rust
use std::collections::HashMap;

fn main() {
    let canadian_cities = vec!["Calgary", "Vancouver", "Gimli"];
    let german_cities = vec!["Karlsruhe", "Bad Doberan", "Bielefeld"];

    let mut city_hashmap = HashMap::new();

    for city in canadian_cities {
        city_hashmap.insert(city, "Canada");
    }
    for city in german_cities {
        city_hashmap.insert(city, "Germany");
    }

    println!("{:?}", city_hashmap["Bielefeld"]); // Prints "Germany"
    println!("{:?}", city_hashmap.get("Bielefeld")); // Prints "Some("Germany")"
    println!("{:?}", city_hashmap.get("asdf")); // Prints "None"

    // Will panic
    // println!("{:?}", city_hashmap["asdf"]);
}
```

## .insert()

* If a `HashMap` already has a key when you try to put it in, using `.insert()` will overwrite its value

```rust
use std::collections::HashMap;

fn main() {
    let mut book_hashmap = HashMap::new();
    book_hashmap.insert(1, "L'Allemagne Moderne");
    book_hashmap.insert(1, "Le Petit Prince");
    book_hashmap.insert(1, "섀도우 오브 유어 스마일");
    book_hashmap.insert(1, "Eye of the World");
    println!("{:?}", book_hashmap.get(&1)); // Prints: "Some("Eye of the World")"
}
```

The following will first use `.get()` to check if a key exists, and if it doesn't, it will use `.insert()` to add it:

```rust
use std::collections::HashMap;

fn main() {
    let mut book_hashmap = HashMap::new();
    let key = 1;

    book_hashmap.insert(1, "L'Allemagne Moderne");

    match book_hashmap.get(&key) {
        Some(val) => println!("Key {key} has a value already: {val}"),
        None => {
            book_hashmap.insert(key, "Le Petit Prince");
        }
    }
    println!("{:?}", book_hashmap.get(&1));
}
```

## .entry()

* Use `.entry()` creates a view into a single entry in a map, which may either be vacant or occupied.
* Use `.or_insert()` to insert a default value if there is no key
* `or_insert()` returns a mutable reference to the value

```rust
use std::collections::HashMap;

fn main() {
    let book_collection = vec![
        "L'Allemagne Moderne",
        "Le Petit Prince",
        "Eye of the World",
        "Eye of the World", //  Duplicated on purpose
    ];

    let mut book_hashmap: HashMap<&str, i32> = HashMap::new();

    for book in book_collection {
        println!("{:?}", book_hashmap.entry(book));
        // Prints:
        // Entry(VacantEntry("L'Allemagne Moderne"))
        // Entry(VacantEntry("Le Petit Prince"))
        // Entry(VacantEntry("Eye of the World"))
        // Entry(OccupiedEntry { key: "Eye of the World", value: 0, .. })

        // The variable return_value is a mutable reference. If nothing is there, it will be 0
        let return_value = book_hashmap.entry(book).or_insert(0);

        // Now return_value is at least 1. And if there was another book, the number it returns will now be increased by 1.
        *return_value += 1;
    }

    for (book, number) in book_hashmap {
        println!("{book}, {number}");
        // Prints:
        // Eye of the World, 2
        // Le Petit Prince, 1
        // L'Allemagne Moderne, 1
    }
}
```

## or_insert()

* You can also do things with `.or_insert()`, such as insert a `Vec` and then push a value onto it

```rust
use std::collections::HashMap;

fn main() {
    let data = vec![
        ("male", 9),
        ("female", 5),
        ("male", 0),
        ("female", 6),
        ("female", 5),
        ("male", 10),
    ];

    let mut survey_hash = HashMap::new();

    for item in data {
        survey_hash.entry(item.0).or_insert(Vec::new()).push(item.1);
    }

    for (male_or_female, numbers) in survey_hash {
        println!("{male_or_female}: {numbers:?}");
        // Prints:
        // female: [5, 6, 5]
        // male: [9, 0, 10]
    }
}
```
