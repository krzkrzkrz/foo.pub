# Structs

* Collection of different types
* Usually, after making a `struct`, you'll start an `impl` block and give it
  some methods. Most of the time, they'll take `&self` or `&mut self` if you need to
  change it.

## Named structs

* Most common struct
* You declare field names and types inside a `{}` code block

Creatina a struct without making variables first:

```rust
struct User {
    username: String,
    email: String,
    active: bool,
    sign_in_count: u64,
}

fn main() {
    let mut user1 = User {
        email: String::from("user1@example.com"),
        username: String::from("user1"),
        active: true,
        sign_in_count: 1,
    };

    user1.email = String::from("user111@example.com");

    let mut user2 = User {
        email: String::from("user2@example.com"),
        username: String::from("user2"),
        ..user1 // The syntax .. specifies that the remaining fields (from user1) be explicitly set on user2
    };

    println!("{:?}", user2);
}
```

Another example, to declare a struct and initialize it with variables of the same name:

```rust
struct Country {
    population: u32,
    capital: String,
    leader_name: String
}

fn main() {
    let population = 500_000;
    let capital = String::from("Elista");
    let leader_name = String::from("Batu Khasikov");

    let kalmykia = Country {
        population,
        capital,
        leader_name,
    };
}
```

## Unit struct

* Unit means "doesn’t have anything" (like the unit type)

```rust
struct FileDirectory;
```

## Tuple structs

* Like structs, but dont have names associated with their fields
* They just have the types of the fields

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

## Sidenote

* A `String` is, in fact, a `struct String`, a `Vec` is a `struct Vec`, and
  there are `impl String` and `impl Vec` blocks, too. There’s nothing magic
  about them, and you’re already starting to see how they work

