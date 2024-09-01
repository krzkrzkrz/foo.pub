# Enums

* Values can only be one of the variants
* Each variant can have different types and amounts of associated data
* Usually, after making a `enum`, you'll start an `impl` block and give it
  some methods. Most of the time, they'll take `&self` or `&mut self` if you need to
  change it.
* An enum is usually about having only one choice, for example:

  ```rust
  enum Status {
      Active,
      Inactive,
      Disabled,
  }
  ```

# Simplest form:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

fn main() {
    let home = IpAddr::V4(127, 0, 0, 1);
    let loopback = IpAddr::V6(String::from("::1"));
}
```

A more complicated structure (with a comparison to a `struct`):

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

If the above following were to be constructed just using a `struct`, then it would look like:

```rust
struct QuitMessage; // Unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // Tuple struct
struct ChangeColorMessage(i32, i32, i32); // Tuple struct
```

`enums` have the advantage of grouping up several variants into one entity

## Giving functions to an enum

```rust
enum ThingsInTheSky {
    Sun,
    Stars,
}

// This function is pretty simple: it takes a number to represent the hour of the day and returns
// a ThingsInTheSky based on that. You can see the Sun between 6 and 18 o’clock; otherwise,
// you can see Stars.
fn create_skystate(time: i32) -> ThingsInTheSky {
    match time {
        6..=18 => ThingsInTheSky::Sun,
        _ => ThingsInTheSky::Stars,
    }
}

// This second function takes a reference to a ThingsInTheSky and prints a message depending on
// which variant of ThingsInTheSky it is.ThingsInTheSky::Sun => println!("I can see the sun!"),
fn check_skystate(state: &ThingsInTheSky) {
    match state {
        ThingsInTheSky::Sun => println!("I can see the sun!"),
        ThingsInTheSky::Stars => println!("I can see the stars!"),
    }
}

fn main() {
    let time = 8; // It's 8 oclock
    let skystate = create_skystate(time); // Returns a ThingsInTheSky
    check_skystate(&skystate); // Prints: I can see the sun!
}
```

Enums can also hold data:

```rust
enum ThingsInTheSky {
    Sun(String),
    Stars(String),
}


fn create_skystate(time: i32) -> ThingsInTheSky {
    // Now that the enum variants hold a String, you have to provide a String,
    // too, when creating ThingsInTheSky.
    match time {
        6..=18 => ThingsInTheSky::Sun(String::from("I can see the sun!")),
             _ => ThingsInTheSky::Stars(String::from("I can see the stars!")),
    }
}

// Now, when we match on our reference to ThingsInTheSky, we have access to the
// data inside (in this case, a String). Note that we can give the inner String
// any name we want here: description, n, or anything else
fn check_skystate(state: &ThingsInTheSky) {
    match state {
        ThingsInTheSky::Sun(description) => println!("{description}"),
        ThingsInTheSky::Stars(n) => println!("{n}")
    }
}

fn main() {
    let time = 8;
    let skystate = create_skystate(time);
    check_skystate(&skystate); // Prints: I can see the sun!
}
```

## "use" keyword

* You can also "import" an enum, so you don’t have to type so much

```rust
enum Mood {
    Happy,
    Sleepy,
    NotBad,
    Angry,
}

fn match_mood(mood: &Mood) -> i32 {
    let happiness_level = match mood {
        // Here we type Mood::everytime:
        Mood::Happy => 10,
        Mood::Sleepy => 6,
        Mood::NotBad => 7,
        Mood::Angry => 2,
    };
    happiness_level
}
```

Much better:

```rust
fn match_mood(mood: &Mood) -> i32 {
    // This imports every variant inside the Mood enum. Using * is the same as
    // writing use Mood::Happy; then use Mood::Sleepy; and so on for each varian
    use Mood::*;

    let happiness_level = match mood {
        Happy => 10,
        Sleepy => 6,
        NotBad => 7,
        Angry => 2,
    };
    happiness_level
}
```

## Casting enums into integers

* If an enum doesn't contain any data, then its variants can be cast into an integer
* Each enum starts with `0`

```rust

enum Season {
    Spring, // If this was Spring(String) or something it wouldn't work.
    Summer,
    Autumn,
    Winter,
}
fn main() {
    use Season::*;

    let four_seasons = vec![Spring, Summer, Autumn, Winter];
    for season in four_seasons {
        println!("{}", season as u32);
    }
} // Prints: 0 1 2 3
```
