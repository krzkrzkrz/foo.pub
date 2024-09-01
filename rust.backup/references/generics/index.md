# Generics

* Generics are a way to reduce the need to write repetitive code and instead delegate this task
  to the compiler while also making the code more flexible. Many languages support some way to
  do this, even though they might call it something different
* The simplest and most common use of generics is for type parameters
* Generic type parameters" are typically represented as `<T>`
* In Rust, "generic" also describes **anything that accepts one or more generic type parameters** `<T>`

## Simple concept

It would be annoying to have to write a function for every type we want to use:

```rust
// i32, i16, u8 are concrete types
fn return_i32(number: i32) -> i32 {  }
fn return_i16(number: i16) -> i16 {  }
fn return_u8(number: u8) -> u8 {  }
// and so on
```

Instead, you can use generics for this:

```rust
// T is a generic type. Meaning, T can be of any type
// T can be named anything, but T is a convention
//
// The following can also be read as:
// The function return_item is generic over type T
fn return_item<T>(item: T) -> T {
    println!("Here is your item.");
    item
}

fn main() {
    let item = return_item(5);
}
```

## Anatomy

If written like the following:

```rust
fn return_item(item: MyType) -> MyType {
```

Then `MyType` is concrete, not generic. The compiler is looking
for something called `MyType` and can't find it.

To tell the compiler that `MyType` is generic, we need to write it inside the angle brackets:

```rust
fn return_item<MyType>(item: MyType) -> MyType {
```

The compiler reads this as:

* `fn` - This is a function
* `<MyType>` - Ah, the function is generic! There will be some type in it that the
  programmer wants to call MyType
* `item: MyType` - The function takes a variable called item, which will be of the
  type MyType that the programmer declared inside the angle brackets
* `-> MyType` - The function will return a value of the type `MyType`

# Trait bounds

* Trait bounds tells the compiler to only allow types to be substituted here if they match the methods signature
  which must be implemented by all the types which implement the trait

For example. Some types are `Copy`, `Clone`, `Debug`, `Display`, etc. With `Debug`, we can print with `{:?}`.

The following will not work (compiler error). The function `print_item()` needs `T` to have Debug to print item,
but is `T` a type with `Debug`? Maybe not. Maybe it doesn't have `#[derive(Debug)]` - who knows?

```rust
fn print_item<T>(item: T) {
    println!("Here is your item: {item:?}");
}

fn main() {
    print_item(5);
}
```

Define the function as accepting any type `T` that implements `Debug`:

```rust
use std::fmt::Debug;

// Without derive(Debug) here, the compiler will complain because
// print_item() requires T to implement Debug
#[derive(Debug)]
struct Animal {
    name: String,
    age: u8,
}

fn print_item<T: Debug>(item: T) {
    println!("Here is your item: {item:?}");
}

fn main() {
    let charlie = Animal {
        name: "Charlie".to_string(),
        age: 1,
    };

    let number = 55;

    print_item(charlie); // Prints: Here is your item: Animal { name: "Charlie", age: 1 }
    print_item(number); // Prints: Here is your item: 55
}
```

## Using where

Sometimes, we need more than one generic type in a generic function, for example:

```rust
use std::fmt::Display;
use std::cmp::PartialOrd;

fn compare_and_display<T: Display, U: Display + PartialOrd>(statement: T, input_1: U, input_2: U) {
    println!("{statement}! Is {input_1} greater than {input_2}? {}", input_1 > input_2);
}

fn main() {
    compare_and_display("Listen up!", 9, 8); // Prints: Listen up! Is 9 greater than 8? true
}
```

Reads as:

* The function name is `compare_and_display()`
* The first type is `T`, and it is generic. It must be a type that can print with `{}`
* The next type is `U`, and it is generic. It must be a type that can print with `{}`
  Also, it must be a type that can be compared (so that it can use `>`, `<`, and `==`)

To make generic functions easier to read, we can also use the keyword `where`:

```rust
// Now the part after compare_and_display only has <T, U>, which is a lot cleaner to read
fn compare_and_display<T, U>(statement: T, num_1: U, num_2: U)
// Then we use the where keyword and indicate the traits needed on the following lines
where
    T: Display,
    U: Display + PartialOrd,
{
    println!(
        "{statement}! Is {num_1} greater than {num_2}? {}",
        num_1 > num_2
    );
}
```

## In `struct`'s

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```


##  In `enum`'s

```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## In methods

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```

