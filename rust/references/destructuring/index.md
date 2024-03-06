# Destructuring

## Enums and structs

* You can also rename variables as you destructure.
* `let tallinn = City { fields };` lets you create a `struct.`
* `let City { fields } = self;` then destructures it.

```rust
struct City {
    name: String,
    name_before: String,
    population: u32,
    date_founded: u32,
}

impl City {
    fn new(
        name: &str,
        name_before: &str,
        population: u32,
        date_founded: u32
    ) -> Self {
        Self {
            name: String::from(name),
            name_before: String::from(name_before),
            population,
            date_founded,
        }
    }

    fn print_names(&self) {
        let City {
            name: official_name, // Assign a new variable for the name field.
            name_before,
            .. // These two dots tell Rust not to care about the other parameters inside City.
        } = self;
        println!("The city {offical_name} used to be called {name_before}.");
    }
}

fn main() {
    let tallinn = City::new("Tallinn", "Reval", 426_538, 1219);
    tallinn.print_names(); // The city Tallinn used to be called Reval.
}
```

## Match

See [Match](../match/index.md)

