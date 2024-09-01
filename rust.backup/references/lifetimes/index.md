# Lifetimes

* `Lifetime` is a way to specify the scope for which a reference is valid
* Usually, you don't need to specify `lifetimes` in your code, but sometimes the compiler
  needs a bit of help and will ask you to tell it how long something should last
* This extra help is called a `lifetime annotation`, which means "extra lifetime information"
* If you don't want to think too much about `lifetime` annotations yet, you can mostly avoid them
  by using owned data as much as possible

## A note on String and &str in relevance to lifetimes


**String**

* Also known as a `string literal`
* They last for the whole program because they are written directly into the binary
* They have the type `&'static str`. The `'` means its lifetime, and string literals have a
  lifetime called `static`

**&str**

* Also known as a `borrowed string`
* This is the regular `&str` form without a `'static` lifetime

## Lifetimes in functions

This works:

```rust
fn return_a_string() -> String {
    let string: String = "I am a string".to_string();
    string
}

fn main() {
    let my_string = return_a_string();
    println!("{my_string}"); // Prints: I am a String
}
```

This doesn't work (because the lifetime of `&str` is too short):

```rust
fn return_a_str() -> &str {
    // Needs a lifetime for &str because &str is a reference
    // &str only lives in this function and is dropped when the function ends
    let str: &str = "I am a string";
    str
}

fn main() {
    let my_str = return_a_str();
    println!("{my_string}"); // Prints: I am a String
}
```

To make this work, you need to specify a lifetime:

```rust
// fn returns_str() -> &'static str tells Rust: "Don't worry; we will only return a string literal"
// String literals live for the whole program, so the compiler is now happy with this
fn return_a_str() -> &'static str {
    let str: &str = "I am a string";
    str
}

fn main() {
    let my_str = return_a_str();
    println!("{my_str}"); // Prints: I am a String
}
```

## Lifetimes in structs

Instead of a `String`. Let's say you wanted to pass a reference to a struct (i.e. to `name`)  
This wouldn't work since `name` is a reference (i.e. `&str`). Rust needs a lifetime for `&str`  
because `&str` is a reference

```rust
#[derive(Debug)]
struct City {
    name: &str,
    date_founded: u32,
}

fn main() {
    let my_city = City {
        name: "Ichinomiya",
        date_founded: 1921,
    };

    println!("{} was founded in {}", my_city.name, my_city.date_founded); // Prints: Ichinomiya was founded in 1921
}
```

Changing `name: &str` to `name: &'static str` would work. However, note that now we can only
take string literals, not references to something else.

```rust
#[derive(Debug)]
struct City {
    name: &'static str,
    date_founded: u32,
}
```

At this point, `name` lives long enough, but the struct `City` doesnt. We can fix this:

```rust
#[derive(Debug)]
// This means that it will only take an input for name if it lives as long as `City`
// You can write anything instead of 'name, even 'a, 'b, 'c, etc
// One good tip is that changing the lifetime to a human-readable name, in this case, we named the
// lifetime after the struct's attribute that it corresponds to
struct City<'name> {
    name: &'name str,
    date_founded: u32,
}
```

**Note**: If you get a `&'a str` in a function, you can just turn it into a `String` and put that
on your `struct`! Rust is extremely fast, even when you do this

## Similarities to traits

When you write `T: Display`, it means "Please only take `T` if it has the trait `Display`". It does not mean "I am giving the trait `Display` to `T`"

```rust
use std::fmt::Display;

fn prints<T: Display>(input: T) {
    println!("T is {input}");
}
```

The same is true for lifetimes. Take a close look at `'a` here:

```rust
#[derive(Debug)]
struct City<'a> {
    name: &'a str,
    date_founded: u32,
}
```

The `'a` means "Please only take an input for name if it lives at least as long as `City`".
It does not mean, "This will make the input for name live as long as `City`"

# Anonymous lifetime

* An indicator that references are being used

```rust
struct Adventurer<'a> {
    name: &'a str,
    hit_points: u32,
}

// Instead of writing: impl<'a> Adventurer, you can use an anonymous lifetime:
impl Adventurer<'_> {
    fn take_damage(&mut self) {
        self.hit_points -= 20;
        println!("{} has {} hit points left!", self.name, self.hit_points);
    }
}
```

