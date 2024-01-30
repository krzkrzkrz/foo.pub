# Rust - Lifetimes

* `Lifetime` is a way to specify the scope for which a reference is valid

Take for example something simple like:

```rust
fn main() {
    let x = 5;
    let y = 10;

    let z = add(x, y);

    println!("{}", z); // Returns 15
}

fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

But now imagine you want to create a function that takes a reference to `x` and `y,` and returns a reference to the sum.  
In other words, you want to avoid copying `x` and `y` into the function, and instead work with references to the original variables

```rust
fn main() {
    let x = 5;
    let y = 10;

    let z = add(&x, &y);

    println!("{}", z);
}

fn add(a: &i32, b: &i32) -> &i32 {
    &(a + b)
}
```

Will return an error. The problem is that the return type of add is a reference to the sum of `a` and `b`, but `a` and `b` are only
temporary values that don't actually have a memory address. In other words, the reference returned by add would be pointing
to a location in memory that doesn't actually exist

This is where lifetimes come in. In order to fix this code, we need to tell Rust that the reference returned by `add` must
have the same lifetime as the references passed to it. We do this by adding a lifetime parameter, `'a`, to the `add` function:

```rust
fn main() {
    let x = 5;
    let y = 10;

    let z = add(&x, &y);

    println!("{}", z);
}

fn add<'a>(a: &'a i32, b: &'a i32) -> &'a i32 {
    &(a + b)
}
```

Now the add function takes two references with the lifetime `'a`, and returns a reference with the same lifetime `'a`.
This tells Rust that the reference returned by `add` is valid for the same lifetime as the references passed to it.

In cases with multiple lifetimes, the syntax is similar:

```rust
foo<'a, 'b> // `foo` has lifetime parameters `'a` and `'b`
```

### Functions

Function signatures with lifetimes have a few constraints:

* Any reference *must* have an annotated lifetime
* Any reference being returned *must* have the same lifetime as an input or be `static`

```rust
// One input reference with lifetime `'a` which must live
// at least as long as the function.
fn print_one<'a>(x: &'a i32) {
    println!("`print_one`: x is {}", x);
}

// Mutable references are possible with lifetimes as well.
fn add_one<'a>(x: &'a mut i32) {
    *x += 1;
}

// Multiple elements with different lifetimes. In this case, it
// would be fine for both to have the same lifetime `'a`, but
// in more complex cases, different lifetimes may be required.
fn print_multi<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("`print_multi`: x is {}, y is {}", x, y);
}

// Returning references that have been passed in is acceptable.
// However, the correct lifetime must be returned.
fn pass_x<'a, 'b>(x: &'a i32, _: &'b i32) -> &'a i32 { x }
```

### Methods

Methods are annotated similarly to functions

```rust
struct Owner(i32);

impl Owner {
    // Annotate lifetimes as in a standalone function.
    fn add_one<'a>(&'a mut self) { self.0 += 1; }
    fn print<'a>(&'a self) {
        println!("`print`: {}", self.0);
    }
}

fn main() {
    let mut owner = Owner(18);

    owner.add_one();
    owner.print();
}
```

### Structs

Annotation of lifetimes in structures are also similar to functions

```rust
// A type `Borrowed` which houses a reference to an
// `i32`. The reference to `i32` must outlive `Borrowed`.
#[derive(Debug)]
struct Borrowed<'a>(&'a i32);

// Similarly, both references here must outlive this structure.
#[derive(Debug)]
struct NamedBorrowed<'a> {
    x: &'a i32,
    y: &'a i32,
}

// An enum which is either an `i32` or a reference to one.
#[derive(Debug)]
enum Either<'a> {
    Num(i32),
    Ref(&'a i32),
}

fn main() {
    let x = 18;
    let y = 15;

    let single = Borrowed(&x);
    let double = NamedBorrowed { x: &x, y: &y };
    let reference = Either::Ref(&x);
    let number    = Either::Num(y);

    println!("x is borrowed in {:?}", single);
    println!("x and y are borrowed in {:?}", double);
    println!("x is borrowed in {:?}", reference);
    println!("y is *not* borrowed in {:?}", number);
}
```

### Traits

Annotation of lifetimes in trait methods basically are similar to functions. Note that `impl` may have annotation of lifetimes too

```rust
// A struct with annotation of lifetimes.
#[derive(Debug)]
struct Borrowed<'a> {
    x: &'a i32,
}

// Annotate lifetimes to impl.
impl<'a> Default for Borrowed<'a> {
    fn default() -> Self {
        Self {
            x: &10,
        }
    }
}

fn main() {
    let b: Borrowed = Default::default();
    println!("b is {:?}", b);
}
```

### Bounds

Just like generic types can be bounded, lifetimes (themselves generic) use bounds as well. The : character has a slightly different meaning here, but + is the same. Note how the following read:

* `T: 'a`: All references in `T` must outlive lifetime `'a`
* `T: Trait + 'a`: Type `T` must implement trait `Trait` and all references in `T` must outlive `'a`

```rust
use std::fmt::Debug; // Trait to bound with.

#[derive(Debug)]
struct Ref<'a, T: 'a>(&'a T);
// `Ref` contains a reference to a generic type `T` that has
// an unknown lifetime `'a`. `T` is bounded such that any
// *references* in `T` must outlive `'a`. Additionally, the lifetime
// of `Ref` may not exceed `'a`.

// A generic function which prints using the `Debug` trait.
fn print<T>(t: T) where
    T: Debug {
    println!("`print`: t is {:?}", t);
}

// Here a reference to `T` is taken where `T` implements
// `Debug` and all *references* in `T` outlive `'a`. In
// addition, `'a` must outlive the function.
fn print_ref<'a, T>(t: &'a T) where
    T: Debug + 'a {
    println!("`print_ref`: t is {:?}", t);
}

fn main() {
    let x = 7;
    let ref_x = Ref(&x);

    print_ref(&ref_x);
    print(ref_x);
}
```

### Static

* Rust has a few reserved lifetime names. One of those is `'static`. You might encounter it in two situations
* Both are related but subtly different and this is a common source for confusion when learning Rust:

```rust
// A reference with 'static lifetime:
let s: &'static str = "hello world";

// 'static as part of a trait bound:
fn generic<T>(x: T) where T: 'static {}
```

#### Reference lifetine

As a reference lifetime `'static` indicates that the data pointed to by the reference lives for the remaining lifetime of the running program

There are two common ways to make a variable with `'static` lifetime, and both are stored in the read-only memory of the binary:

1. Make a constant with the static declaration

```rust
// Make a constant with `'static` lifetime.
static NUM: i32 = 18;
```

2. Make a string literal which has type: &'static str

```rust
// Returns a reference to `NUM` where its `'static`
// lifetime is coerced to that of the input argument.
fn coerce_static<'a>(_: &'a i32) -> &'a i32 {
    &NUM
}
```

#### Trait bound

As a trait bound, it means the type does not contain any non-static references.
Eg. the receiver can hold on to the type for as long as they want and it will never become invalid until they drop it.

It's important to understand this means that any owned data always passes a `'static` lifetime bound, but a reference to that owned data generally does not:

```rust
use std::fmt::Debug;

fn print_it( input: impl Debug + 'static ) {
    println!( "'static value passed in is: {:?}", input );
}

fn main() {
    // i is owned and contains no references, thus it's 'static:
    let i = 5;
    print_it(i);

    // oops, &i only has the lifetime defined by the scope of
    // main(), so it's not 'static:
    print_it(&i);
}
```

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




