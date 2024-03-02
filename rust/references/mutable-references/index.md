# Mutable references

* `my_number` is an `i32,` and num_ref is `&mut` `i32.`
* In speech, you call this a `"mutable reference to an i32"` or a `"ref mut i32"`.

```rust
fn main() {
    let mut my_number = 8;
    let num_ref = &mut my_number;
}
```

Lets modify the value of `my_number` using `num_ref`.

* You can't write `num_ref += 10` because `num_ref` is not the `i32` value; it is an `&i32`
* There’s nothing to add inside a reference. The value to add is actually inside the `i32`
* To reach the place where the value is, we use `*`
* Using `*` lets you move from the reference to the value behind the reference
* `&` is called `referencing,` using `*` is called `dereferencing`

```rust
let mut my_number = 8;
    let num_ref = &mut my_number;
    *num_ref += 10; // Use * to change the i32 value
    println!("{}", my_number); // Prints 18
```

## Shadowing

The reason why `country_ref` is "Italy" and not `8` is because we shadow `country`  
with `8`, which is an `i32`. But the first country was not destroyed,  
so `country_ref` still points to "Italy", not `8`.

```rust
fn main() {
    let country = String::from("Italy");
    let country_ref = &country;
    let country = 8;
    println!("{country_ref} {country}"); // Prints: Italy 8
}
```

## Giving references to functions

It does not work because country ends up destroyed, and the memory gets cleaned up
after the first calling of the `print_country()` function

1. We create the `String` called `country`. The variable `country` is the owner of the data.
2. We pass `country` to the function `print_country`, which now owns the data. The
   function doesn't have a `->`, so it doesn’t return anything. After `print_country`
   finishes, our `String` is now dead.
3. We try to give `country` to `print_country`, but we already did that, and it died inside
   the function! The data that `country` used to own doesn't exist anymore.

This is also called a `move` because the data moves into the function, and that is where it ends

```rust
fn print_country(country_name: String) {
    println!("{country_name}");
}
fn main() {
    let country = String::from("Italy");
    print_country(country); // Prints "Italy"
    print_country(country); // Error: value used here after move
}
```

One way to solve this is to give a reference to the function:

```rust
fn print_country(country_name: &String) {
    println!("{country_name}");
}

fn main() {
    let country = String::from("Italy");
    print_country(&country); // Prints "Italy"
    print_country(&country); // Prints "Italy"
}
```

Or if you need to mutate the value, you can use a mutable reference:

```rust
fn print_country(country_name: &mut String) {
    println!("{country_name}");
    country_name.push_str(" and is a beautiful country");
}

fn main() {
    let mut country = String::from("Italy is in Europe");
    print_country(&mut country); // Prints "Italy is in Europe"
    print_country(&mut country); // Prints "Italy is in Europe and is a beautiful country"
}
```

## Another example of owenership and references

Since country is not being used in the `main` function after the `adds_hungary` function is called,  
`adds_hungary` takes ownership of `country` and can take country as mutable, and it is perfectly  
safe to do so; nobody else owns it.

```rust

```rust
// Takes ownership of 'ountry' and can take 'country' as mutable
fn adds_hungary(mut string_to_add_hungary_to: String) {
    string_to_add_hungary_to.push_str("-Hungary");
    println!("{}", string_to_add_hungary_to);
}

fn main() {
    let country = String::from("Austria");
    adds_hungary(country); // Prints: "Austria-Hungary"
}
```
