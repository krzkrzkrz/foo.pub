# Vectors

* Can only hold elements of one type
* You can resize vectors: push / remove elements, append other vectors

## Creating a vec

* There are 2 ways to create a vec

Using the `Vec::new` method

```rust
let my_vec: Vec<String> = Vec::new();
my_vec.push(name1);
my_vec.push(name2);
```

Using the `vec!` macro

```rust
let my_vec: Vec<i32> = vec![1, 2, 3];
```

Other types of vecs:

* `Vec<(i32, i32)>` - This is a `Vec` where each item is a `tuple`: `(i32, i32)`
* `Vec<Vec<String>>` - This is a `Vec` that has Vecs of `Strings`

## Reading elements

```rust
let vec_of_ten = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

let two_to_five = &vec_of_ten[2..5]; // 2..5 means from index 2 up to index 5 but not including index 5
let start_at_one = &vec_of_ten[1..]; // 1.. means from index 1 until the end.
let end_at_five = &vec_of_ten[..5]; // ..5 means from the beginning up to but not including index 5
let everything = &vec_of_ten[..]; // Means to slice the whole vec: beginning to end

println!("Two to five: {two_to_five:?}"); // Prints: Two to five: [2, 3, 4]
println!("Start at one: {start_at_one:?}"); // Prints: Start at one: [1, 2, 3, 4, 5, 6, 7, 8, 9]
println!("End at five: {end_at_five:?}"); // Prints: End at five: [0, 1, 2, 3, 4]
println!("Everything: {everything:?}"); // Prints: Everything: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## Memory allocation

* Vecs allocate memory, so they have some methods to reduce memory usage and make them faster
* As you push new items onto the `Vec`, it gets closer and closer to the capacity. It won't give
  an error if you go past the capacity, so don’t worry. However, if you go past the capacity,
  it will double its capacity and copy the items into this new memory space
* For example, imagine that you have a `Vec` with a capacity of 4 and four items inside it. If you
  add one more item, it will need a new memory space that can hold all five items. So, it will
  double its capacity to 8 and copy the five items over into the new memory space. This is
  called reallocation

```rust
// Use capacity() to get the current capacity of the vec
fn main() {
    let mut num_vec = Vec::new();
    println!("{}", num_vec.capacity()); // Prints: 0

    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 4

    num_vec.push('a');
    num_vec.push('a');
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 4, still 4 elements, so no need to reallocate

    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 8. We have five elements, but it doubled four to eight to make space.
}
```

In above example, this vector has two reallocations: 0 to 4 and 4 to 8. We can make it more
efficient by giving it a capacity of 8 to start:

```rust
fn main() {
    let mut num_vec = Vec::with_capacity(8);
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 8

    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 8

    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 8

    num_vec.push('a');
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // Prints: 8
}
```

## Handling out of bounds

```rust
// Handling out of bounds
let v = vec![1, 2, 3, 4, 5];
let v_index = 10;

match v.get(v_index) {
    Some(_) => { println!("Reachable element at index: {}", v_index); },
    None => { println!("Unreachable element at index: {}", v_index); }
} // Returns None

let does_not_exist = &v[100]; // Returns an error
let does_not_exist = v.get(100); // Returns None
```

## Iterating over a vec

```rust
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

## From trait

* A trait used for value conversions between types

For example:

```rust
fn main() {
    let array_vec = Vec::from([8, 9, 10]);
    println!("Vec from array: {array_vec:?}"); // Prints: Vec from array: [8, 9, 10]

    // Prints the byte values of each character in the string
    let str_vec = Vec::from("What kind of Vec am I?");
    println!("Vec from str: {str_vec:?}"); // Prints: Vec from str: [87, 104, 97, 116, 32, 107, 105, 110, 100, 32, 111, 102, 32, 86, 101, 99, 32, 97, 109, 32, 73, 63]

    // Prints the byte values of each character in the string
    let string_vec = Vec::from("What will a String be?".to_string());
    println!("Vec from String: {string_vec:?}"); // Prints: Vec from String: [87, 104, 97, 116, 32, 119, 105, 108, 108, 32, 97, 32, 83, 116, 114, 105, 110, 103, 32, 98, 101, 63]
}
```

## Custom From trait

* We can write our own custom `From` trait for our own types

```rust
struct Feet {
    value: f64,
}

// Define a custom struct named `Meters`
struct Meters {
    value: f64,
}

// Implement the `From` trait for converting from `Feet` to `Meters`
impl From<Feet> for Meters {
    fn from(feet: Feet) -> Self {
        // Conversion formula: 1 foot = 0.3048 meters
        // Self refers to the type Meters. Could have also been written out as Meters { value: feet.value * 0.3048 }
        Self {
            value: feet.value * 0.3048,
        }
    }
}

fn main() {
    let feet_length = Feet { value: 10.0 };
    // Convert length from feet to meters using `From` trait
    let meters_length: Meters = Meters::from(feet_length);

    println!("Length in meters: {:.2}", meters_length.value); // Prints: Length in meters: 3.05
}
```
