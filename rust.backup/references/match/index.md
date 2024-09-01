# Match

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

## Match can destructure

A `match` block can destructure items in a variety of ways

### Tuples

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

### Arrays

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

### Enums

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

### Pointers / ref

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

### Structs

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

## Match guards

A `match guard` can be added to filter the arm

```rust
#[derive(PartialEq, Debug)]
struct PositiveNonzeroInteger(u64);

#[derive(PartialEq, Debug)]
enum IntegerError {
    Negative,
    Zero,
}

fn main() {
  let value: i32 = 10;
  return match value {
      num if num > 0 => PositiveNonzeroInteger(num),
//    ^ "value" is assigned to "num". "if num > 0" is a guard
      0 => Err(IntegerError::Zero),
      _ => Err(IntegerError::Negative),
  };
}
```

A more complex example:

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

## Using @

You can also use `@` to give a name to the value of a match expression, and then you can use it

```rust
fn match_number(input: i32) {
    match input {
      number @ 4 => println!("{number} is unlucky in China (sounds close to 死)!"),
      number @ 13 => println!("{number} is lucky in Italy! In bocca al lupo!"),
      number @ 14..=19 => println!("Some other number that ends with -teen: {number}"),
      _ => println!("Some other number, I guess"),
    }
}

fn main() {
    match_number(50); // Prints: "Some other number, I guess"
    match_number(13); // Prints: "13 is lucky in Italy! In bocca al lupo!"
    match_number(16); // Prints: "Some other number that ends with -teen: 16"
    match_number(4); // Prints: "4 is unlucky in China (sounds close to 死)!"
}
```

