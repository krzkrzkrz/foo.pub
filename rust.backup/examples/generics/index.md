# Rust - Generics

* Generics are a way to reduce the need to write repetitive code and instead delegate this task
  to the compiler while also making the code more flexible. Many languages support some way to
  do this, even though they might call it something different
* The simplest and most common use of generics is for type parameters
* Generic type parameters" are typically represented as `<T>`
* In Rust, "generic" also describes **anything that accepts one or more generic type parameters** `<T>`

  For example, defining a *generic* function named `foo` that takes an argument `T` of any type:

  ```rust
  fn largest<T>(arg: &[T]) -> &T { ... }
  ```

  We read this definition as: the function `largest` is generic over some type `T`. This function has one parameter
  named `arg`, which is a slice of values of type `T`. The `largest` function will return a reference to a value of the same type `T`.

Think of *generics* as writing code with placeholder types instead of actual types. The actual types are inserted later.

For example:

```rust
enum Option<T>{
  Some(T),
  None
}
```

`T` is a type parameter. Whenever we use it with a type, compiler generates the enum definition tailored to that particular type.  
For example, if we use `Option` for a `String`, the compiler will essentially generate a definition similar to the below:

```rust
enum StringOption{
  Some(String),
  None
}
```

All of this happens at the compilation stage; thus, we don’t have to worry about defining distinct enums for each data type we
want to use them with, and maintaining code for all of them.

Another example:

```rust
// You define any type to take place of T and E, and the compiler will
// generate and use a unique definition for each combination
enum Result<T,E>{
  Ok(T),
  Err(E)
}
```

Another example:

```rust
struct Wrapper<DataType>{
  data:DataType
}

// Can give error sometimes. The compiler usually automatically detects the type to be filled
// in for the generic type, but in this case, 5 can be u8 , u16, usize or quite a few other types
// Thus, better to explicitly declare
let d1 = Wrapper{data:5};

// This is better, since the generic type is explicitly declared as u8
let d1 : DataStore<u8> = DataStore{data:5}; // or:
let d1 = DataStore{data:5_u8};

let d2 = Wrapper{data:"data".to_owned()};
```

### Generics with trait bounds

Trait bounds tells the compiler to only allow types to be substituted here if they match the methods signature
which must be implemented by all the types which implement the trait

To restrict a type parameter by traits, the syntax is:

```rust
fn fun<Type:trait1+trait2+...>(...){...}
```

For example, the `Eq` and `Ord` are two traits in the standard Rust library

```rust
fn sort<Sortable:Ord+Eq>(arr:&mut[Sortable]){ }

// Can also be written out as:
fn sort<Sortable>(arr:&mut [Sortable])
where
    Sortable:Ord+Eq,
{
}
```

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


