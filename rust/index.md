# Rust Journey

[Rust](https://www.rust-lang.org) A systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.

## Suggested learning approach

A [recommended approach](https://twitter.com/AndreaPessino/status/1042120425415700480). In summary:

Rust is a practical solution to concrete problems that have hindered progress in software development for the last two decades.
It is a leap forward in potential performance, scalability and productivity.

When experienced coders try out a new language the process usually goes something like this: skim the docs as quickly as possible,
go over some samples until one has got the gist, write some code; the expectation is that details and deep concepts wi
reveal themselves with use and that writing code is going to be the quickest way to familiarize oneself with the new tool.
If you are learning Python, Java, JS, C#, Go, etc. this approach works well, mostly because these languages are basically all
the same. If, on the other hand, you approach Rust like this you will probably be frustrated. There is nothing especially difficult
about Rust (no more than C/C++, at least), but Rust combines novel concepts and refined ideas from other languages into a surprisingly
innovative whole. Its design is cohesive, purposeful and visionary.

It is different enough to require actual studying.

At a glance Rust looks like a C++ cousin, but it really is not, and the resemblance might actually prove misleading for newcomers.
This has nothing to do with ability or experience you just need to put in the time to acquire some new skills, otherwise before long
you'll end up on Twitter complaining about how you are "fighting the borrow checker".

So, here are a few suggestion on how to approach the language to ensure you have a greatest grasp in minimal time:

1. First one is obvious: read the official [RUST PROGRAMMING LANGUAGE](https://doc.rust-lang.org/stable/book). This is the best introductory
text you can get very easy to read, perfectly paced. Read the whole thing beginning to end, run and study the code samples, even when
they seem trivial.

IMPORTANT: when you are done reading this book you might think I get it now. But really, you dont. The official book does not go
nearly deep enough. Its a great intro but after this you will be smack in the middle of the "got the gist phase". You've got some
more work to do.

2. Read [RUST BY EXAMPLE](https://doc.rust-lang.org/rust-by-example). This is a great series of code snippets illustrating
most of the language components. All examples are short, can be run directly (and even modified) in the books pages and they
each illustrate a specific aspect of Rust programming. This will not take long study all of them, make sure you modify and play
with each one, don't move on until you fully understand each chapter. After going through this, you'll be in a good spot. Then
it'll be time to bring it all home.

3. Read [PROGRAMMING RUST](https://www.amazon.com/Programming-Rust-Fast-Systems-Development-ebook).
This is a fantastic book and the perfect finishing touch in your Rust education. It goes deeper than the previous two,
and having the others under your belt beforehand will make it more effective and much easier to go through. You don't really
need to finish the book before moving on, just go over the first 6 to 8 chapters and then start Step 4 while continuing to
read the remaining chapters.

4. This is where you start writing your own code. A few more bits of advice:

- Use as basic a starting point as you can

- Avoid binding to/integrating any existing non-Rust code, yours or otherwise

- Try to write Rust code as idiomatic as you can. This is important in any language, but it is imperative in Rust

- Follow the recommended coding standard, use the formatting and linting tools Rust provides (rustfmt and clippy, for example). Correct
the warnings, do not ignore them. Do things the Rust way it's the best way to start and you can always diverge later on when you
have enough experience to adequately assess the implications.

- Stop thinking in terms of the patterns you are used to. Do not try to replicate object oriented idioms with Rust. None of
these abstractions are necessary or especially beneficial, and while they have their place the early exploration of a
language that does not specifically support them is not it. Start (or go back to) thinking in terms of data layout and
interface design, instead of reducing problems to fixed patterns

- And finally: give it time. It will take a while, but you will find that Rust was well worth the investment

## Install rustup

```shell
curl https://sh.rustup.rs -sSf | sh
```

## Uninstall Rust and rustup

```shell
rustup self uninstall
```

## New [project](project)

```console
cargo new foobar
```

## Updating Rust

```console
rustup update
```

## Comments

```rust
// A comment here

let hello = "hello"; // Inline comment
```

### Debug

Replacing the `{}` placeholder with `{:?}` uses the debug placeholder. Any type
that supports debug printing will print a detailed debugging dump of its
contents, rather than just the value.

```rust
println!("{:?}", name);
```

## Data types

* Four primary scalar types: integers, floating-point numbers, booleans, and characters

### Integer types

* Signed integers are positive or negative numbers
* Unsigned are only positive numbers
* Integers default to `i32`

| Length  | Signed            | Unsigned        |
|---------|-------------------|-----------------|
| 8-bit   | i8 (-128 to 127)  | u8 (0 to 255)   |
| 16-bit  | i16               | u16             |
| 32-bit  | i32               | u32             |
| 64-bit  | i64               | u64             |
| 128-bit | i128              | u128            |
| arch    | isize             | usize           |

### Floating-point types

* Floating-point numbers are represented according to the IEEE-754 standard
* The `f32` type is a single-precision float, and `f64` has double precision
* Floats default to `f64`

```rust
let x = 2.0; // f64
let y: f32 = 3.0; // f32
```

### Boolean types

* `true` or `false`
* The Boolean type in Rust is specified using `bool`

```rust
let t = true;

// With explicit type annotation
let likely_false: bool = false;
```

### Character types

* Characters are specified with a single `'`
* Can represent a lot more than just ASCII

```rust
  let c = 'z';
  let heart_eyed_cat = '😻';
```

## Variables

* Variables by default, are immutable (i.e. not subject to change)

```rust
let x = 5;
x = 6; // Returns an error

let mut x = 5;
x = 6; // No error
```

## Strings

* In Rust, strings are a more complicated data structure, than is apparent in other programming languages

### String concatenation

```rust
let hello = "hello";
let world = "world";

println!("{} {}", hello, world);
```

### Strings and characters

```rust
// This is a string with length of 1
"a"

// This is a string with length of 3
"abc"

// This is a single character
'a'

// An empty string
""

// Error: Must contain 1 character
'abc'

// Error: Must contain 1 character
''
```

### More on strings

```rust
fn main() {
    let mut s1 = "foo".to_string(); // Creates a String
    s1.push_str("bar"); // Appends bar

    // Create a String from a string literal
    let mut s2 = String::from("initial contents");

    // Creates a new, empty String
    let mut s3 = String::new();

    // Creates a reference to a String (&str)
    // Returns error: no method named push_str found for type &str in the current scope
    let mut s4 = "foo";
    s4.push_str("bar");

    // Above must be converted to a String first, before using push_str
    let s5 = "foo";
    let mut s6 = s5.to_string();
    s6.push_str("ba"); // Appends ba
    s6.push('r'); // Appends a single character
    println!("{}", s6); // Returns "foobar"
}
```

### Concatenating strings

Using the `+` operator to combine `String`s

```rust
let s1 = String::from("Hello ");
let s2 = String::from("amazing ");
let s3 = String::from("world!");
let s4 = s1 + &s2 + &s3; // s1 has been moved here and can no longer be used
println!("{}", s4); // Returns "Hello amazing world!""
```

The reason `s1` is no longer valid after the addition and the reason we used a reference to `s2` has to do with the signature of the method `add`

Which looks something like:

```rust
fn add(self, s: &str) -> String { ... }
```

### `format!`

* The `format!` macro works in the same way as `println!`
* Instead of printing the output to the screen, it returns a `String` with the contents
* The version of the code using `format!` is much easier to read and doesn't take ownership of any of its parameters

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}", s1, s2, s3);
```

## Conditions

```rust
let is_one = 1 == 1;

println!("{}", is_one); // true
```

## Tuples

* Grouping together some number of other values with a variety of types
* Tuples are mainly used as a sort of minimal-drama struct type. i.e. to store width and height
* Tuples allow constants as indices, like `t.4`. You cant write `t.i` or `t[i]` to get the `i`'th element
* Are constructed using a `()`

```rust
let tup: (&str, i32, f64, u8) = ("foo", 500, 6.4, 1);

// Or
let tup = ("foo", 500, 6.4, 1);
let (x, y, z, w) = tup;

println!("{}", tup.0) // Returns foo
```

## Arrays

* Collection of similar types
* You cant append new or remove elements
* Are constructed using a `[]`

```rust
let months: [&str; 12] = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
println!("{}", a[1]); // February
```

// Or
```rust
let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
```

## Structs

* Collection of different types

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
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

## Tuple structs

* Like structs, but dont have names associated with their fields
* They just have the types of the fields

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

## Enums

* Values can only be one of the variants
* Each variant can have different types and amounts of associated data.

3 types of enums:

1. `Tuple structs`, which are, basically, named tuples.

   ```rust
   // A tuple struct
   struct Pair(i32, f32);
   ```

2. The classic `C structs`

   ```rust
   // A C struct
   #[derive(Debug)]
   struct Person {
       name: String,
       age: u8,
       x: f32,
       y: f32,
   }
   ```

3. `Unit structs`, which are field-less, are useful for generics.

   ```rust
   // A unit struct
   struct Unit;
   ```

`enum` in its simplest form:

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

A more complicated structure (with a comparison to a `struct`)

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

## Type aliases

If you use a type alias, you can refer to each enum variant via its alias.  
This might be useful if the enum's name is too long or too generic, and you want to rename it.

```rust
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

// Creates a type alias
type Operations = VeryVerboseEnumOfThingsToDoWithNumbers;

fn main() {
    // We can refer to each variant via its alias, not its long and inconvenient
    // name.
    let x = Operations::Add;
}
```

The most common place you'll see this is in impl blocks using the `Self` alias.

```rust
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

impl VeryVerboseEnumOfThingsToDoWithNumbers {
    fn run(&self, x: i32, y: i32) -> i32 {
        match self {
            Self::Add => x + y,
            Self::Subtract => x - y,
        }
    }
}
```

## Constants

Two different types of `constants`:

1. `const`: An unchangeable value (the common case).
2. `static`: A possibly mutable variable with `'static` lifetime. The static lifetime is  
   inferred and does not have to be specified. Accessing or modifying a mutable static variable is `unsafe`.

```rust
// Globals are declared outside all other scopes.
static LANGUAGE: &str = "Rust";
const THRESHOLD: i32 = 10;
```

Almost always, if you can choose between the two, choose `const.` It’s pretty rare that you actually want a  
memory location associated with your constant, and using a `const` allows for optimizations like constant  
propagation not only in your crate but downstream crates.

## Option type

* Rust does not have `nulls`
* Rust has an enum that can encode the concept of a value being present or absent

It is defined by the standard library as:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

Dont need to bring it into scope explicitly. You can use `Some` and `None` directly without the `Option::` prefix:

```rust
let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
```

## Match

* Compare a value against a series of patterns and then execute code based on which pattern matches
* All possible values must be covered (i.e. using `_ => ...`)

```rust
fn main() {
    println!("{}", get_string(Some(1))); // Returns "one"
}

fn get_string(number: Option<i32>) -> &'static str {
    match number {
        Some(1) => "one",
        Some(2) => "two",
        Some(3) => "three",
        Some(4) => "four",
        Some(5) => "five",
        _ => "Not in the list",
    }
}
```

### Match can destructe

A `match` block can destructure items in a variety of ways.

**Tuples**

```rust
fn main() {
    let triple = (0, -2, 3);

    println!("Tell me about {:?}", triple);

    // `..` can be used to ignore the rest of the tuple
    match triple {
        (0, y, z) => println!("First is `0`, `y` is {:?}, and `z` is {:?}", y, z), // Destructure the second and third elements
        (1, ..)  => println!("First is `1` and the rest doesn't matter"),
        (.., 2)  => println!("last is `2` and the rest doesn't matter"),
        (3, .., 4)  => println!("First is `3`, last is `4`, and the rest doesn't matter"),
        _      => println!("It doesn't matter what they are"), // `_` means don't bind the value to a variable
    }
}
```

Output:

```
Tell me about (0, -2, 3)
First is `0`, `y` is -2, and `z` is 3
```

**Arrays**

```rust
fn main() {
    // Try changing the values in the array, or make it a slice!
    let array = [1, -2, 6];

    match array {
        // Binds the second and the third elements to the respective variables
        [0, second, third] =>
            println!("array[0] = 0, array[1] = {}, array[2] = {}", second, third),

        // Single values can be ignored with _
        [1, _, third] => println!(
            "array[0] = 1, array[2] = {} and array[1] was ignored",
            third
        ),

        // You can also bind some and ignore the rest
        [-1, second, ..] => println!(
            "array[0] = -1, array[1] = {} and all the other ones were ignored",
            second
        ),
        // The code below would not compile
        // [-1, second] => ...

        // Or store them in another array/slice (the type depends on
        // that of the value that is being matched against)
        [3, second, tail @ ..] => println!(
            "array[0] = 3, array[1] = {} and the other elements were {:?}",
            second, tail
        ),

        // Combining these patterns, we can, for example, bind the first and
        // last values, and store the rest of them in a single array
        [first, middle @ .., last] => println!(
            "array[0] = {}, middle = {:?}, array[2] = {}",
            first, middle, last
        ),
    }
}
```

Output:

```
array[0] = 1, array[2] = 6 and array[1] was ignored
```

**Enums**

```rust
// `allow` required to silence warnings because only
// one variant is used.
#[allow(dead_code)]
enum Color {
    // These 3 are specified solely by their name.
    Red,
    Blue,
    Green,
    // These likewise tie `u32` tuples to different names: color models.
    RGB(u32, u32, u32),
    HSV(u32, u32, u32),
    HSL(u32, u32, u32),
    CMY(u32, u32, u32),
    CMYK(u32, u32, u32, u32),
}

fn main() {
    let color = Color::RGB(122, 17, 40);

    println!("What color is it?");

    match color {
        Color::Red   => println!("The color is Red!"),
        Color::Blue  => println!("The color is Blue!"),
        Color::Green => println!("The color is Green!"),
        Color::RGB(r, g, b) => println!("Red: {}, green: {}, and blue: {}!", r, g, b),
        Color::HSV(h, s, v) => println!("Hue: {}, saturation: {}, value: {}!", h, s, v),
        Color::HSL(h, s, l) => println!("Hue: {}, saturation: {}, lightness: {}!", h, s, l),
        Color::CMY(c, m, y) => println!("Cyan: {}, magenta: {}, yellow: {}!", c, m, y),
        Color::CMYK(c, m, y, k) => println!("Cyan: {}, magenta: {}, yellow: {}, key (black): {}!", c, m, y, k),
        // Don't need another arm (i.e. _) because all variants have been examined
    }
}
```

Output:

```
What color is it?
Red: 122, green: 17, and blue: 40!
```

**Pointers / ref**

```rust
fn main() {
    // Assign a reference of type `i32`. The `&` signifies there
    // is a reference being assigned.
    let reference = &4;

    match reference {
        // If `reference` is pattern matched against `&val`, it results
        // in a comparison like:
        // `&i32`
        // `&val`
        // ^ We see that if the matching `&`s are dropped, then the `i32`
        // should be assigned to `val`.
        &val => println!("Got a value via destructuring: {:?}", val),
    }

    // To avoid the `&`, you dereference before matching.
    match *reference {
        val => println!("Got a value via dereferencing: {:?}", val),
    }

    // What if you don't start with a reference? `reference` was a `&`
    // because the right side was already a reference. This is not
    // a reference because the right side is not one.
    let _not_a_reference = 3;

    // Rust provides `ref` for exactly this purpose. It modifies the
    // assignment so that a reference is created for the element; this
    // reference is assigned.
    let ref _is_a_reference = 3;

    // Accordingly, by defining 2 values without references, references
    // can be retrieved via `ref` and `ref mut`.
    let value = 77;
    let mut mut_value = 6;

    // Use `ref` keyword to create a reference.
    match value {
        ref r => println!("Got a reference to a value: {:?}", r),
    }

    // Use `ref mut` similarly.
    match mut_value {
        ref mut m => {
            // Got a reference. Gotta dereference it before we can
            // add anything to it.
            *m += 10;
            println!("We added 10. `mut_value`: {:?}", m);
        },
    }
}
```

Output:

```
Got a value via destructuring: 4
Got a value via dereferencing: 4
Got a reference to a value: 77
We added 10. `mut_value`: 16
```

**Structs**

```rust
fn main() {
    struct Foo {
        x: (u32, u32),
        y: u32,
    }

    // Try changing the values in the struct to see what happens
    let foo = Foo { x: (1, 2), y: 3 };

    match foo {
        Foo { x: (1, b), y } => println!("First of x is 1, b = {},  y = {} ", b, y),

        // you can destructure structs and rename the variables,
        // the order is not important
        Foo { y: 2, x: i } => println!("y is 2, i = {:?}", i),

        // and you can also ignore some variables:
        Foo { y, .. } => println!("y = {}, we don't care about x", y),
        // this will give an error: pattern does not mention field `x`
        //Foo { y } => println!("y = {}", y),
    }

    let faa = Foo { x: (1, 2), y: 3 };

    // You do not need a match block to destructure structs:
    let Foo { x : x0, y: y0 } = faa;
    println!("Outside: x0 = {x0:?}, y0 = {y0}");
}
```

Output:

```
First of x is 1, b = 2,  y = 3 
Outside: x0 = (1, 2), y0 = 3
```

### Match guards

A `match guard` can be added to filter the arm

```rust
#[allow(dead_code)]
enum Temperature {
    Celsius(i32),
    Fahrenheit(i32),
}

fn main() {
    let temperature = Temperature::Celsius(35);

    match temperature {
        Temperature::Celsius(t) if t > 30 => println!("{}C is above 30 Celsius", t),
        // The `if condition` part ^ is a guard
        Temperature::Celsius(t) => println!("{}C is below 30 Celsius", t),
        Temperature::Fahrenheit(t) if t > 86 => println!("{}F is above 86 Fahrenheit", t),
        Temperature::Fahrenheit(t) => println!("{}F is below 86 Fahrenheit", t),
        // Don't need another arm (i.e. _) because all variants have been examined
    }
}
```

### Binding

Indirectly accessing a variable makes it impossible to branch and use that variable without re-binding.  
`match` provides the `@` sigil for binding values to names

```rust
// A function `age` which returns a `u32`.
fn age() -> u32 {
    15
}

fn main() {
    println!("Tell me what type of person you are");

    match age() {
        0             => println!("I haven't celebrated my first birthday yet"),
        // Could `match` 1 ..= 12 directly but then what age
        // would the child be? Instead, bind to `n` for the
        // sequence of 1 ..= 12. Now the age can be reported.
        n @ 1  ..= 12 => println!("I'm a child of age {:?}", n),
        n @ 13 ..= 19 => println!("I'm a teen of age {:?}", n),
        // Nothing bound. Return the result.
        n             => println!("I'm an old person of age {:?}", n),
    }
}
```

### if-let

For some use cases, when matching `enums`, `match` is awkward. For example:

```rust
// Make `optional` of type `Option<i32>`
let optional = Some(7);

match optional {
    Some(i) => {
        println!("This is a really long string and `{:?}`", i);
        // ^ Needed 2 indentations just so we could destructure
        // `i` from the option.
    },
    _ => {},
    // ^ Required because `match` is exhaustive. Doesn't it seem
    // like wasted space?
};
```

`if let` is cleaner for this use case and in addition allows various failure options to be specified:

```rust
fn main() {
    // All have type `Option<i32>`
    let number = Some(7);
    let letter: Option<i32> = None;
    let emoticon: Option<i32> = None;

    // The `if let` construct reads: "if `let` destructures `number` into
    // `Some(i)`, evaluate the block (`{}`).
    if let Some(i) = number {
        println!("Matched {:?}!", i);
    }

    // If you need to specify a failure, use an else:
    if let Some(i) = letter {
        println!("Matched {:?}!", i);
    } else {
        // Destructure failed. Change to the failure case.
        println!("Didn't match a number. Let's go with a letter!");
    }

    // Provide an altered failing condition.
    let i_like_letters = false;

    if let Some(i) = emoticon {
        println!("Matched {:?}!", i);
    // Destructure failed. Evaluate an `else if` condition to see if the
    // alternate failure branch should be taken:
    } else if i_like_letters {
        println!("Didn't match a number. Let's go with a letter!");
    } else {
        // The condition evaluated false. This branch is the default:
        println!("I don't like letters. Let's go with an emoticon :)!");
    }
}
```

Output:

```
Matched 7!
Didn't match a number. Let's go with a letter!
I don't like letters. Let's go with an emoticon :)!
```

**Take note**, `if let` can also be combined with an else, via `let else`

### while-let

Similar to `if let`, `while let` can make awkward match sequences more tolerable. Consider the following sequence that increments `i`

```rust
// Make `optional` of type `Option<i32>`
let mut optional = Some(0);

// Repeatedly try this test.
loop {
    match optional {
        // If `optional` destructures, evaluate the block.
        Some(i) => {
            if i > 9 {
                println!("Greater than 9, quit!");
                optional = None;
            } else {
                println!("`i` is `{:?}`. Try again.", i);
                optional = Some(i + 1);
            }
            // ^ Requires 3 indentations!
        },
        // Quit the loop when the destructure fails:
        _ => { break; }
        // ^ Why should this be required? There must be a better way!
    }
}
```

Using `while let` makes this sequence much nicer

```rust
// Make `optional` of type `Option<i32>`
let mut optional = Some(0);

// This reads: "while `let` destructures `optional` into
// `Some(i)`, evaluate the block (`{}`). Else `break`.
while let Some(i) = optional {
    if i > 9 {
        println!("Greater than 9, quit!");
        optional = None;
    } else {
        println!("`i` is `{:?}`. Try again.", i);
        optional = Some(i + 1);
    }
    // ^ Less rightward drift and doesn't require
    // explicitly handling the failing case.
}
// ^ `if let` had additional optional `else`/`else if`
// clauses. `while let` does not have these.
```

## Loops

Rust provides a `loop` keyword to indicate an infinite loop.

* The `break` statement can be used to exit a loop at anytime
* The `continue` statement can be used to skip the rest of the iteration

```rust
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");

    // Infinite loop
    loop {
        count += 1;

        if count == 3 {
            println!("three");

            // Skip the rest of this iteration
            continue;
        }

        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");

            // Exit this loop
            break;
        }
    }
}
```

Outputs:

```
Let's count until infinity!
1
2
three
4
5
OK, that's enough
```

### Nesting and labels

Loops must be annotated with some `'label`

```rust
#![allow(unreachable_code, unused_labels)]

fn main() {
    'outer: loop {
        println!("Entered the outer loop");

        'inner: loop {
            println!("Entered the inner loop");

            // This would break only the inner loop
            //break;

            // This breaks the outer loop
            break 'outer;
        }

        println!("This point will never be reached");
    }

    println!("Exited the outer loop");
}
```

Output:

```
Entered the outer loop
Entered the inner loop
Exited the outer loop
```

### Returning from loops

Put the value after the `break`, and it will be returned by the loop expression

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("Exited the loop, counter: {}", result); // Exited the loop, counter: 20
}
```

## While loop

The `while` keyword can be used to run a loop while a condition is true.

```rust
fn main() {
    // A counter variable
    let mut n = 1;

    // Loop while `n` is less than 101
    while n < 101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }

        // Increment counter
        n += 1;
    }
}
```

## For loop

The `for in` construct can be used to iterate through an `Iterator` (i.e. `a..b`)

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

Alternatively, `a..=b` can be used for a range that is inclusive on both ends. The above can be written as:

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..5..=100 {
      // ...
    }
}
```

## iter, into_iter, iter_mut

* `iter`, `into_iter`  and `iter_mut` all handle the conversion of a collection into an iterator in different ways
* `for` loop will apply the `into_iter`

**iter** - This borrows each element of the collection through each iteration. Thus leaving the collection  
untouched and available for reuse after the loop


```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter() {
        match name {
            &"Ferris" => println!("There is a rustacean among us!"),
            // TODO ^ Try deleting the & and matching just "Ferris"
            _ => println!("Hello {}", name),
        }
    }

    println!("names: {:?}", names); // "names" can be reused here, since match only borrowed the element
}
```

Output:

```
Hello Bob
Hello Frank
There is a rustacean among us!
names: ["Bob", "Frank", "Ferris"]
```

**into_iter** - This consumes the collection so that on each iteration the exact data is provided. Once the  
collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop

```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.into_iter() {
        match name {
            "Ferris" => println!("There is a rustacean among us!"),
            _ => println!("Hello {}", name),
        }
    }

    println!("names: {:?}", names); // Returns an error, because "names" was already consumed by match
}
```

**iter_mut** - This mutably borrows each element of the collection, allowing for the collection to be  
modified in place

```rust
fn main() {
    let mut names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }

    println!("names: {:?}", names);
}
```

Output:

```
names: ["Hello", "Hello", "There is a rustacean among us!"]
```

## Functions

```rust
fn main() {
    println!("Hello, world!");

    println!("{}", add_one(5));
}

// Type annotated
// Expects a 32 bit unsigned (negative or positive number)
// Returns a 32 bit unsigned integer
fn add_one(x: u32) -> u32 {
    x + 1
}

// Functions that don't return a value, actually return the unit type `()`
// In which case, the return type can be omitted from the signature
fn foobar(x: u32) -> () {
    println!("foobar");
}
```

### Advanced functions

* This technique is useful when you want to pass a function you've already defined rather than defining a new closure

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);

    println!("The answer is: {}", answer);
}
```

## Methods

* Methods are similar to functions
* They are declared with the `fn`
* They can have parameters and a return value
* The difference: They are defined within the context of a struct (or an enum or a trait object)
* The benefit of using methods instead of functions, in addition to using method syntax and not having to repeat the type of self in every method's signature, is for organization

```rust
struct User {
    first_name: String,
    last_name: String,
    age: u64,
}

// To define the function within the context of User, we start an impl (implementation) block
// And change the first (and in this case, only) parameter to be self
impl User {
    fn full_name(&self, nickname: &str) -> String {
        format!("{} {} aka {}", self.first_name, self.last_name, nickname)
    }

    fn can_drive(&self) -> bool {
        if self.age >= 18 {
            true
        } else {
            false
        }
    }
}

fn main() {
    let user1 = User {
        first_name: String::from("Foo"),
        last_name: String::from("Bar"),
        age: 18,
    };

    println!("{}", user1.full_name("Bugsy")); // Returns "Foo Bar aka Foopub"
    println!("This user can drive: {}", user1.can_drive()); // Returns "This user can drive: true"
}
```

### Associated functions

* To call this associated function, we use the `::` syntax with the struct name
* Define functions within `impl` blocks that don't take `self` as a parameter

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);

    // Returns:
    // Rectangle {
    //    width: 3,
    //    height: 3
    //}
    println!("{:#?}", sq);
}
```

## Closures

* Closures are functions that can capture the enclosing environment

```rust
fn main() {
    let outer_var = 42;

    // A regular function can't refer to variables in the enclosing environment
    //fn function(i: i32) -> i32 { i + outer_var }
    // TODO: uncomment the line above and see the compiler error. The compiler
    // suggests that we define a closure instead.

    // Closures are anonymous, here we are binding them to references.
    // Annotation is identical to function annotation but is optional
    // as are the `{}` wrapping the body. These nameless functions
    // are assigned to appropriately named variables.
    let closure_annotated = |i: i32| -> i32 { i + outer_var };
    let closure_inferred  = |i| i + outer_var;

    // Call the closures.
    println!("closure_annotated: {}", closure_annotated(1));
    println!("closure_inferred: {}", closure_inferred(1));
    // Once closure's type has been inferred, it cannot be inferred again with another type.
    //println!("cannot reuse closure_inferred with another type: {}", closure_inferred(42i64));
    // TODO: uncomment the line above and see the compiler error.

    // A closure taking no arguments which returns an `i32`.
    // The return type is inferred.
    let one = || 1;
    println!("closure returning one: {}", one());
}
```

## Statements and expressions

* Statements are instructions that perform some action and do not return a value
* Statements end with `;`
* Expressions evaluate to a resulting value
* Expressions do not include an ending `;`

```rust
let y = 6; // Statement
y + 1 // Expression
```

## Conditions

```rust
let x = 1;

// Ternary condition
if x == 1 { "One" } else { "Other" };

// Condition blocks
let number_to_s = if x == 1 {
  "One"
}
else if x == 2 {
  "Two"
}
else {
  "Three"
}
```

## Modules

* `mod` keyword declares a new module
* By default, functions, types, constants, and modules are private. The `pub` keyword makes an item public and therefore visible outside its namespace.
* The `use` keyword brings modules, or the definitions inside modules, into scope

```console
cargo new communicator --lib // Creates a new module
cd communicator
```

Notice: Cargo generated src/lib.rs instead of src/main.rs

### Sample module (in `src/lib.rs`, multiple modules with the function name `connect`)

* Call `server::connect` or `client::connect` respectively

```rust
mod server {
    fn connect() {}
}

mod client {
    fn connect() {}
}
```

### Module inside a module

* Call `server::connect` or `server::client::connect` respectively

```rust
mod server {
    fn connect() {
    }

    mod client {
        fn connect() {
        }
    }
}
```

### Module loading

```rust
// src/lib.rs
mod client;
mod server;
```

Note that we dont need a `mod` declaration. It was already declared `src/lib.rs`.

```rust
// src/client.rs
fn connect() {
  // Some code here
}
```

```rust
// src/server.rs
fn server() {
  // Some code here
}
```

### Module filesystems

* If a module named `foo` has no submodules, you should put the declarations for `foo` in a file named `foo.rs`
* If a module named `foo` does have submodules, you should put the declarations for `foo` in a file named `foo/mod.rs`

```console
└── foo
    ├── bar.rs (contains the declarations in `foo::bar`)
    └── mod.rs (contains the declarations in `foo`, including `mod bar`)
```

### Privacy / public rules

```rust
pub mod a {
    pub fn public_function() {
        println!("Here");
    }

    fn private_function() {
        println!("Here");
    }

    pub mod series {
        pub mod of {
            pub fn nested_modules() {
                println!("Here");
            }
        }
    }
}

use a::series::of;

fn main() {
    a::public_function(); // Returns "Here"
    a::series::of::nested_modules(); // Returns "Here"
    of::nested_modules()
    a::private_function(); // Returns error: function `private_function` is private
}
```

### Compiling modules

Use `cargo build` instead of `cargo run` because we have a library crate rather than a binary crate

## `use` keyword

* Shortens lengthy function calls by bringing the modules of the function you want to call into scope
* Can be used against `enums`

```rust
pub mod a {
    pub mod really {
        pub mod long {
            pub mod list {
                pub mod of {
                    pub fn nested_modules() {
                        println!("Here");
                    }
                }
            }
        }
    }
}

enum TrafficLight {
    Red,
    Yellow,
    Green,
}

use TrafficLight::{Red, Yellow};
use a::really::long::list::of;

fn main() {
    of::nested_modules(); // Returns "Here"

    let red = Red;
    let yellow = Yellow;
    let green = TrafficLight::Green;
}
```

### Super and self

* The `self` keyword refers to the current module scope
* The `super` keyword refers to the parent scope (i.e. outside a module)

```rust
fn function() {
    println!("called `function()`");
}

mod cool {
    pub fn function() {
        println!("called `cool::function()`");
    }
}

mod my {
    fn function() {
        println!("called `my::function()`");
    }

    mod cool {
        pub fn function() {
            println!("called `my::cool::function()`");
        }
    }

    pub fn indirect_call() {
        // Let's access all the functions named `function` from this scope!
        print!("called `my::indirect_call()`, that\n> ");

        // The `self` keyword refers to the current module scope - in this case `my`.
        // Calling `self::function()` and calling `function()` directly both give
        // the same result, because they refer to the same function.
        self::function();
        function();

        // We can also use `self` to access another module inside `my`:
        self::cool::function();

        // The `super` keyword refers to the parent scope (outside the `my` module).
        println!("here");
        super::function();

        // This will bind to the `cool::function` in the *crate* scope.
        // In this case the crate scope is the outermost scope.
        {
            use crate::cool::function as root_function;
            root_function();
        }
    }
}

fn main() {
    my::indirect_call();
}
```

Output:

```
called `my::indirect_call()`, that
> called `my::function()`
called `my::function()`
called `my::cool::function()`
called `function()`
called `cool::function()`
```

## Attributes

* An attribute is metadata applied to some module, crate or item
* Attributes look like `#[outer_attribute]` or `#![inner_attribute]`

### #[outer_attribute]

Applies to the item immediately following it. Some examples of items are: a function, a module declaration,
a constant, a structure, an enum. Here is an example where attribute `#[derive(Debug)]` applies to the struct `Rectangle`:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
```

### ![inner_attribute]

Applies to the enclosing item (typically a module or a crate). In other words, this attribute is intepreted as applying to the entire scope in which it's place

```rust
// Applies to the whole crate (if placed in main.rs)
#![allow(unused_variables)]

fn main() {
    let x = 3; // This would normally warn about an unused variable.
}
```

### Sytaxes

Attributes can take arguments with different syntaxes:

```rust
#[attribute = "value"]
#[attribute(key = "value")]
#[attribute(value)]

// Attributes can have multiple values and can be separated over multiple lines, too
#[attribute(value, value2)]
#[attribute(value, value2, value3,
            value4, value5)]
```

### Dead code

Is an attribute that disables the `dead_code` lint

```rust
#[allow(dead_code)]
fn unused_function() {}

// Warning will trigger here
fn noisy_unused_function() {}

fn main() {
    used_function();
}
```

### cfg

Configuration conditional checks are possible through two different operators:

* The `cfg` attribute: `#[cfg(...)]` in attribute position
* The `cfg!` macro: `cfg!(...)` in boolean expressions

```rust
// This function only gets compiled if the target OS is linux
#[cfg(target_os = "linux")]
fn are_you_on_linux() {
    println!("You are running linux!");
}

// And this function only gets compiled if the target OS is *not* linux
#[cfg(not(target_os = "linux"))]
fn are_you_on_linux() {
    println!("You are *not* running linux!");
}

fn main() {
    are_you_on_linux();

    println!("Are you sure?");
    if cfg!(target_os = "linux") {
        println!("Yes. It's definitely linux!");
    } else {
        println!("Yes. It's definitely *not* linux!");
    }
}
```

### Customer cfg conditions

```rust
#[cfg(some_condition)]
fn conditional_function() {
    println!("condition met!");
}

fn main() {
    conditional_function();
}
```

Then in console:

```
$ rustc --cfg some_condition custom.rs && ./custom
condition met!
```

## Vectors

* Can only hold elements of one type
* You can resize vectors: push / remove elements, append other vectors

```rust
// Creating a new, empty vector to hold values of type i32
let v: Vec<i32> = Vec::new();

// Creating a new vector to hold values of type i32
let v = vec![1, 2, 3];

// Updating a vector
let mut v = Vec::new();

// Use the push method to add values to a vector
v.push(5);
v.push(6);
v.push(7);
v.push(8);

// Reading elements
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2]

// Handling out of bounds
let v = vec![1, 2, 3, 4, 5];
let v_index = 10;

match v.get(v_index) {
    Some(_) => { println!("Reachable element at index: {}", v_index); },
    None => { println!("Unreachable element at index: {}", v_index); }
} // Returns None

let does_not_exist = &v[100]; // Returns an error
let does_not_exist = v.get(100); // Returns None

// Iterating over a vector
let v = vec![100, 32, 57];

for i in &v {
    println!("{}", i);
}

// Iterating over a vector and making changes
let mut v = vec![100, 32, 57];

for i in &mut v {
    *i += 50;
}

// Building a vector using an iterator
let v: Vec<i32> = (0..5).collect();
```

## Slices

* TODO

## Hash maps

* Useful when you want to look up data not by using an index, as you can with vectors, but by using a key that can be of any type

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// Iterate over a hash map
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
// Returns:
// Yellow: 50
// Blue: 10

// Accesing a key in the hash map
let team_name = String::from("Blue");
let score = scores.get(&team_name);

println!("{:?}", score); // Returns Some(10)
println!("{:?}", scores); // Returns {"Blue": 10, "Yellow": 50}

// Overwriting a value
let mut teams = HashMap::new();
teams.insert(String::from("Rangers"), 2);
teams.insert(String::from("Falcons"), 1);

println!("{:?}", teams); // Returns {"Rangers": 2, "Falcons": 1}

teams.insert(String::from("Falcons"), 2);
println!("{:?}", teams); // Returns {"Rangers": 2, "Falcons": 1}

// Insert if key has no value
let mut groceries = HashMap::new();
groceries.insert(String::from("Apples"), 1);

groceries.entry(String::from("Apples")).or_insert(10);
groceries.entry(String::from("Oranges")).or_insert(1);

println!("{:?}", groceries); // Returns {"Apples": 1, "Oranges": 1}

// A more complex example
let text = "hello world wonderful world";

let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0); // Returns a mutable reference (&mut V)
    *count += 1; // In order to assign to that value, we must first dereference count using the asterisk (*)
}

println!("{:?}", map); // Returns: {"world": 2, "wonderful": 1, "hello": 1}
```

## Error handling

### `panic!`

Force a panic:

```rust
panic!("crash and burn"); // Will cause an immediate panic
```

Running with backtrace:

```rust
let v = vec![1, 2, 3];
v[99];
```

```shell
$ env RUST_BACKTRACE=1 cargo run
```

### Recoverable Errors with `Result`

If file doesn't exist, create it. And if creation succeeds, return file
Else, panic with a message

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Tried to create file but there was a problem: {:?}", e),
            },
            other_error => panic!("There was a problem opening the file: {:?}", other_error),
        },
    };
}
```

### `.expect()`

Returns: `thread 'main' panicked at 'Failed to open hello.txt: ...`

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").expect("Failed to open hello.txt");
}
```

## Generics

More info on **generics** [here](examples/generics/index.md)

## Lifetimes

More info on **lifetimes** [here](examples/lifetimes/index.md)

## Generic types

### In `struct`'s

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

###  In `enum`'s

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

### In methods

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

## Traits

More info on **traits** [here](examples/traits/index.md)

## Generic Type Parameters, Trait Bounds, and Lifetimes Together

Explanation follows in the sub-sections below

```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(x: &'a str, y: &'a str, ann: T) -> &'a str
    where T: Display
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

### Lifetime annotations

* Names of lifetime parameters must start with an apostrophe `'` and are usually all lowercase and very short
* Most people use the name `'a`
* We place lifetime parameter annotations after the `&` of a reference, using a space to separate the annotation from the reference's type

```rust
&i32        // A reference
&'a i32     // A reference with an explicit lifetime
&'a mut i32 // A mutable reference with an explicit lifetime
```

### Lifetime Annotations in Function Signatures

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

## Closures (anonymous functions)

* The closure definition comes after the `=`
* A closure has params (separated by `,`)
* Body of closure is inside the `{}`. These are optional if the closure body is a single expression
* Annotation types of the parameters or the return value are not required like `fn` functions
* Closures are usually short and relevant only within a narrow context

```rust
let foo = |bar, baz| {
    // Body of clusure starts here
    bar // Value returned
};

// Closure without {}, because the closure body has a single expression
let foo = |bar, baz| bar + baz;

// Adding optional type annotations of the parameter and return value types
let foo = |bar: u32| -> u32 { ... };
```

`foo` is the name of the closure
Closure has two parameters `bar` and `baz`

## Iterators

* Iterators are lazy. Meaning they have no effect until you call methods that consume the iterator to use it up
* All iterators implement a trait named `Iterator`

```rust
let v1 = vec![1, 2, 3];

let v1_iter = v1.iter();

for val in v1_iter {
    println!("Got: {}", val);
}

// Accessing the next() method iin the Iterator trait
let mut v2_iter = v1.iter();
println!("{:?}", v2_iter.next()); // Returns Some(1)
```

## Pointers

### Reference pointers

* Common kind of pointer is called a reference: `&`
* No special capabilities other than referring to data
* References are pointers that only borrow data

```rust
&foo
```

### Box pointers

* The easiest way to allocate value in the heap is to use a `Box::new`

```rust
let t = (12, "eggs");
let b = Box::new(t); // Will allocate tuple in the heap
```

The type of `t` is `(i32, &str)`, so the type of `b` is `Box<(i32, &str)>`

### Smart pointers

* Have additional metadata and capabilities
* Smart pointers own the data they point to
* `String`, `Struct` and `Vec<T>` are considered smart pointers. Because they own some memory and allow you to manipulate it

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created.");
}

// Returns:
// CustomSmartPointers created.
// Dropping CustomSmartPointer with data `other stuff`!
// Dropping CustomSmartPointer with data `my stuff`!
```

## Fearless concurrency

```rust
```

## 

```rust
```

## 

```rust
```

## 

```rust
```

## 

```rust
```

---

# At the pool:

- [ ] Create a blog
- [ ] Create a shopping cart
- [x] Implement chapter 17.3 - Implementing a State
- [ ] Go through chapter 18.3 - All the pattern syntax

# Todo

- [x] Read 19 (read once)
- [ ] Read 20 (useless)
- [ ] Read 21 (skim once)
