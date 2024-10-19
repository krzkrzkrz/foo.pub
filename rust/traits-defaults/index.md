# Traits: Defaults

* The `Default` trait is used to define a default value for a type

```rust
#[derive(Debug)]
struct Person {
    first_name: String,
    last_name: String,
    age: u8,
    location: String,
}

impl Default for Person {
    fn default() -> Self {
        Person {
            first_name: "John".to_string(),
            last_name: "Doe".to_string(),
            age: 21,
            location: "World".to_string(),
        }
    }
}

fn main() {
    // String::default() returns an empty string
    println!("Default string {}", String::default()); // Prints: "Default string: "
                                                      //
    let p1 = Person::default();
    println!("{:#?}", p1); // Prints: Person { first_name: "John", last_name: "Doe", age: 21, location: "World" }

    // Override one of the default values. In this case, we override first_name
    let p2 = Person {
        first_name: "Shannon".to_string(),
        ..Default::default()
    };
    println!("{:#?}", p2); // Prints: Person { first_name: "Shannon", last_name: "Doe", age: 21, location: "World" }
}
```

## Setting defaults on types

* The `Default` trait can be used to set defaults on types
* In the example. The default trait is implemented for the `nickname`

```rust
#[derive(Debug)]
struct Nickname(String);

impl Default for Nickname {
    fn default() -> Self {
        Nickname("Foobar".to_string())
    }
}

#[derive(Debug)]
struct Person {
    first_name: String,
    nickname: Nickname,
}

impl Default for Person {
    fn default() -> Self {
        Person {
            first_name: "John".to_string(),
            nickname: Nickname::default(),
        }
    }
}

fn main() {
    let p1 = Person {
        first_name: "Joe".to_string(),
        nickname: Nickname::default(),
    };
    println!("{:#?}", p1); // Prints: Person { first_name: "Joe", nickname: Nickname("Foobar") }

    let p2 = Person::default();
    println!("{:#?}", p2); // Prints:  Person { first_name: "John", nickname: Nickname("Foobar") }
}
```

