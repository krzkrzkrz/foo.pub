# Iterators

* A `for` loop is an iterator of values, so typing `for item in iterator` is the same as typing `for item in iterator.into_iter()`
* `.iter()` for an iterator of references
* `.iter_mut()` for an iterator of mutable references
* `.into_iter()` for an iterator of values (not references)

## .iter() and .iter_mut()

```rust
fn main() {
    let vector1 = vec![1, 2, 3];
    let mut vector2 = vec![10, 20, 30];

    // First, we use .iter() so that vector1 is not destroyed
    //
    // vector1 is not destroyed here, so we can still use it after this for loop
    for num in vector1.iter() {
        println!("Printing a &i32: {num}");
        // Prints:
        // Printing a &i32: 1
        // Printing a &i32: 2
        // Printing a &i32: 3
    }

    println!("{vector1:?}"); // Prints: [1, 2, 3]

    // This is the same as writing "for num in vector1.into_iter()."
    // It owns the values, and vector1 no longer exists after this for loop is done
    //
    // vector1 is destroyed here, so we can't use it after this for loop
    for num in vector1 {
        println!("Printing an i32: {num}");
        // Prints:
        // Printing an i32: 1
        // Printing an i32: 2
        // Printing an i32: 3
    }

    // println!("{vector1:?}"); // Returns an error because vector1 doesn't exist anymore

    // This for loop takes mutable references, so vector2 still exists after it is over
    // vector2 is not destroyed here, so we can still use it after this for loop
    for num in vector2.iter_mut() {
        *num *= 10;
        println!("num is now {num}");
        // Prints:
        // num is now 100
        // num is now 200
        // num is now 300
    }

    println!("{vector2:?}"); // Prints: [100, 200, 300]
}
```

## Without for

* You don't need to use `for` to use an iterator, though. Here is another way to use them:
* `.iter_mut()` plus `.for_each()` is basically a `for` loop

```rust
fn main() {
    let vector1 = vec![1, 2, 3];
    let iter = vector1
        .iter() // Here as well, we are using .iter() first so that vector1 is not destroyed.
        .map(|x| x + 1) // || is a closure. So |x| means "x gets passed into the closure (the function)"
        .collect::<Vec<i32>>();

    println!("{vector1:?}"); // Prints: [1, 2, 3]

    // When you call into_iter() on a collection, it means you are transferring ownership of
    // the collection's elements to the iterator.
    //
    // map() method lets you do something to every item (including turning it into a different type)
    // and then pass it on to make a new iterator
    let into_iter = vector1.into_iter().map(|x| x * 10).collect::<Vec<i32>>();

    // println!("{vector1:?}"); // Will give an error because vector1 has been moved into into_iter

    let mut vector2 = vec![10, 20, 30];

    // for_each() method lets you do something with every item without creating a new iterator
    // Meaning, there is no need to do: .map().collect()
    //
    // It is mutable, so we don't need to use .collect() to create a new Vec. Instead, we
    // change the values in the same Vec with mutable references
    vector2.iter_mut().for_each(|x| *x += 100);

    println!("{vector2:?}"); // Prints: [110, 120, 130]

    println!("{:?}", iter); // Prints: [2, 3, 4]
    println!("{:?}", into_iter); // Prints: [10, 20, 30]
    println!("{:?}", vector2); // Prints: [110, 120, 130]
}
```

## .next()

* Returns an `Option`
* When you use an iterator, it calls `.next()` over and over again to see whether there are
  more items left. If `.next()` returns `Some`, there are still items left, and the iterator
  keeps going. If `None` is returned, the iteration is finished

```rust
fn main() {
    // Just a regular Vec<char>
    let my_vec = vec!['a', 'b', '거', '柳'];

    // The Vec is an Iterator type now, but wehaven’tcalled.next()onityet. It’s an iterator waiting to be called
    let mut my_vec_iter = my_vec.iter();

    // Calls the first item with .next() and then again and again. Each time the iterator will return Some with the value inside
    assert_eq!(my_vec_iter.next(), Some(&'a'));
    assert_eq!(my_vec_iter.next(), Some(&'b'));
    assert_eq!(my_vec_iter.next(), Some(&'거'));
    assert_eq!(my_vec_iter.next(), Some(&'柳'));
    assert_eq!(my_vec_iter.next(), None); // Now the iterator is out of items, so it returns None
    assert_eq!(my_vec_iter.next(), None); // You can keep calling next() and it will keep returning None
}
```

## Implementing an Iterator for your own type

```rust
#[derive(Debug)]
struct Library {
    name: String,
    // Could have been written as: books: Vec<String>, but we want to implement Iterator on it
    // so we use our own type: BokCollection
    books: BookCollection,
}

// We add a Clone, later we can .clone(), so we can do what we want
// with it without touching the original Library
#[derive(Debug, Clone)]
// BookCollection is just a Vec<String> on the inside, but it’s our type,
// so we can implement traits on it.
struct BookCollection(Vec<String>);

impl Library {
    fn add_book(&mut self, book: &str) {
        self.books.0.push(book.to_string());
    }
    fn new(name: &str) -> Self {
        Self {
            name: name.to_string(),
            books: BookCollection(Vec::new()),
        }
    }
    fn get_books(&self) -> BookCollection {
        self.books.clone()
    }
}

impl Iterator for BookCollection {
    type Item = String;

    fn next(&mut self) -> Option<String> {
        match self.0.pop() {
            Some(book) => {
                println!("Accessing book: {book}");
                Some(book)
            }
            None => {
                println!("Out of books at the library!");
                None
            }
        }
    }
}

fn main() {
    let mut my_library = Library::new("Calgary");
    my_library.add_book("The Doom of the Darksword");
    my_library.add_book("Demian - die Geschichte einer Jugend");
    my_library.add_book("구운몽");
    my_library.add_book("吾輩は猫である");

    for item in my_library.get_books() {
        println!("{item}");
    } // Prints:
      // Accessing book: 吾輩は猫である
      // 吾輩は猫である
      // Accessing book: 구운몽
      // 구운몽
      // Accessing book: Demian - die Geschichte einer Jugend
      // Demian - die Geschichte einer Jugend
      // Accessing book: The Doom of the Darksword
      // The Doom of the Darksword
      // Out of books at the library!
}
```

## Other useful iterator methods

**enumerate()**:

* Gives you the index and the value

```rust
fn main() {
    let char_vec = vec!['z', 'y', 'x'];

    char_vec
        // Makes char_vec into an iterator
        .iter()
        // Now, each item is (usize, char) instead of just char.
        .enumerate()
        .for_each(|(index, c)| println!("Index {index} is: {c}"));
    // Prints:
    // Index 0 is: z
    // Index 1 is: y
    // Index 2 is: x
}
```

**zip()**:

* Put 2 `Vec`'s together into a `HashMap`

```rust
use std::collections::HashMap;

fn main() {
    let keys = vec![0, 1, 2, 3, 4, 5].into_iter();
    let values = vec!["zero", "one", "two", "three", "four", "five"].into_iter();
    let number_word_hashmap: HashMap<i32, &str> = keys.zip(values).collect();

    println!(
        "The value at key 2 is: {}",
        number_word_hashmap.get(&2).unwrap()
    ); // Prints: The value at key 2 is: two
}
```

**filter()**:

* Keep the items in an iterator that you want based on an expression that returns a `bool`

```rust
fn main() {
    let months = vec![
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
    ];

    let filtered_months = months
        .into_iter()
        // Can also be written as a one liner: .filter(|month| month.len() < 5 && month.contains("u"))
        // Purpose is to show you can use .filter() more than once
        .filter(|month| month.len() < 5)
        .filter(|month| month.contains("u"))
        .collect::<Vec<&str>>();

    println!("{:?}", filtered_months); // Prints: ["June", "July"]
}
}
```

**filter_map()**:

* Does both `.filter()` and `.map()`
* Operates on a `Option<T>` and takes the value out of each `Option` if it is `Some`
* For example: `Vec` holding `Some(2)`, `None`, `Some(3)`, it would return `[2, 3]`

```rust
struct Company {
    name: String,
    ceo: Option<String>,
}

impl Company {
    fn new(name: &str, ceo: &str) -> Self {
        let ceo = match ceo {
            "" => None,
            ceo => Some(ceo.to_string()),
        }; // ceo is decided, so now we return Self.

        Self {
            name: name.to_string(),
            ceo,
        }
    }

    fn get_ceo(&self) -> Option<String> {
        // Just returns a clone of the CEO (struct is not Copy)
        self.ceo.clone()
    }
}

fn main() {
    let company_vec = vec![
        Company::new("Umbrella Corporation", "Unknown"),
        Company::new("Ovintiv", "Brendan McCracken"),
        Company::new("The Red-Headed League", ""),
        Company::new("Stark Enterprises", ""),
    ];

    let all_the_ceos = company_vec
        .iter()
        .filter_map(|company| company.get_ceo()) // filter_map needs Option<T>
        .collect::<Vec<String>>();

    println!("{:?}", all_the_ceos); // Prints: ["Unknown", "Brendan McCracken"]
}
```

Another example:

```rust
fn main() {
    let user_input = vec![
        "8.9",
        "Nine point nine five",
        "8.0",
        "7.6",
        "eleventy-twelve",
    ];

    let successful_numbers = user_input
        .iter()
        // Can you filter_map() here, because ok() is a Result method, and returns an Option<T>
        .filter_map(|input| input.parse::<f32>().ok())
        .collect::<Vec<f32>>();

    println!("{:?}", successful_numbers); // Prints: [8.9, 8.0, 7.6]
}
```

**and_then()**:

* This method's input is an `Option`, and its output is also an `Option`

```rust
fn main() {
    let num_array = ["8", "9", "Hi", "9898989898"];

    // Results go in here
    let mut char_vec = vec![];

    for index in 0..5 {
        char_vec.push(
            num_array
                .get(index) // .get() returns an Option
                .and_then(|number| number.parse::<u32>().ok()), // Next, we try to parse the number into a u32 and then use .ok() to turn it into an Option
        )
    }

    println!("{:?}", char_vec); // Prints: [Some(8), Some(9), None, None, None]
}
```

**and()**:

* Combine `Option` types. If all options are `Some`, it returns the second `Option`.
* If either of them is `None`, it returns `None`

```rust
fn main() {
    let option1: Option<i32> = Some(5);
    let option2: Option<i32> = Some(10);
    let option3: Option<i32> = None;

    let result1 = option1.and(option2);
    let result2 = option1.and(option3);

    println!("Result 1: {:?}", result1); // Prints: Some(10)
    println!("Result 2: {:?}", result2); // Prints: None
}
```

**flatten()**:

* Is a convenient way to ignore all `None` or `Err` values in an iterator and only return the successful values

Consider:

```rust
fn main() {
    for num in ["9", "nine", "ninety-nine", "9.9"]
        .into_iter()
        .map(|num| num.parse::<f32>())
    {
        println!("{num:?}");
        // Prints:
        // Ok(9.0)
        // Err(ParseFloatError { kind: Invalid })
        // Err(ParseFloatError { kind: Invalid })
        // Ok(9.9)
    }
}
```

Now with `flatten()`:

```rust
fn main() {
    for num in ["9", "nine", "ninety-nine", "9.9"]
        .into_iter()
        .map(|num| num.parse::<f32>())
        .flatten()
    {
        println!("{num:?}");
        // Prints:
        // 9.0
        // 9.9
    }
}
```

**all()**:

* It returns `true` if all elements satisfy the predicate

```rust
fn main() {
    let numbers = vec![2, 4, 6, 8, 10];
    let is_even = |&x| x % 2 == 0;

    let all_even = numbers.iter().all(is_even);

    match all_even {
        true => println!("All numbers are even."),
        false => println!("Not all numbers are even."),
    }
}
```

**any()**:

* Checks until it finds one matching item, and then it stops there's no need to
  check the rest of the items at this point

```rust
fn main() {
    let vec: Vec<i32> = (1..=20).collect();
    println!("{:?}", vec.iter().any(|&number| number == 5)); // Prints: true
}
```

**find() and position()**:

* `.find()` - I'll try to get it for you
* `.position()` - I'll try to find where it is for you

```rust
fn main() {
    let num_vec = vec![10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

    // && is a double reference
    // The first & because the closure receives a reference to each element
    // The second & because the closure receives a reference to the reference
    println!("{:?}", num_vec.iter().find(|&&number| number == 30)); // Prints: Some(30)
    println!("{:?}", num_vec.iter().position(|&number| number == 30)); // Prints: Some(2)

    println!("{:?}", num_vec.iter().find(|&&number| number == 110)); // Prints: None
    println!("{:?}", num_vec.iter().position(|&number| number == 110)); // Prints: None
}
```

**fold()**:

* Combine all elements of an iterator into a single value by applying a closure that
  accumulates the result of each iteration

A simple exmplae:

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    // initial_num starts with 5, and then we add each number in the array to it
    // For example: 5+1, then 6+2, then 8+3, then 11+4, then 15+5 = 20
    let sum: i32 = numbers.iter().fold(5, |initial_num, &x| initial_num + x);

    println!("Sum of all numbers: {}", sum); // Prints: Sum of all numbers: 20
}
```

A more complex example:

```rust
#[derive(Debug)]
struct CombinedEvents {
    num_of_events: u32,
    data: Vec<String>,
}

fn main() {
    let events = [
        "Went to grocery store",
        "Came home",
        "Fed cat",
        "Fed cat again",
    ];

    // We'll start with an empty CombinedEvents struct. You could also use #[derive(Default)] on
    // top and then write CombinedEvents::default() to do the same thing.
    let empty_events = CombinedEvents {
        num_of_events: 0,
        data: vec![],
    };

    let combined_events = events
        .iter()
        // .fold() needs a default value, which is the empty struct. Then, for every item in our
        // events array, we get access to the CombinedEvents struct and the next event (a &str).
        .fold(empty_events, |mut total_events, next_event| {
            // We increase the number of events by 1 every time, push the next event to the data
            // field and pass on the struct so it is available for the next iteration.
            total_events.num_of_events += 1;
            total_events.data.push(next_event.to_string());
            total_events
        });

    println!("{combined_events:#?}");
    // Prints:
    // CombinedEvents {
    //     num_of_events: 4,
    //     data: [
    //         "Went to grocery store",
    //         "Came home",
    //         "Fed cat",
    //         "Fed cat again",
    //     ],
    // }
}
```

**by_ref()**:

* This method borrows an iterator, so you can use it multiple times

```rust
fn main() {
    let mut number_iter = [7, 8, 9, 10].into_iter();

    // This will coplain because .take() method takes a self, so it takes the whole iterator
    // And since second_two is trying to take the iterator again, it will complain
    // let first_two = number_iter.take(2).collect::<Vec<_>>();

    // Instead, we can use .by_ref() to take a reference to the iterator, so we can use it again
    let first_two = number_iter.by_ref().take(2).collect::<Vec<_>>();
    let second_two = number_iter.take(2).collect::<Vec<_>>();

    println!("{:?}", first_two); // Prints: [7, 8]
    println!("{:?}", second_two); // Prints: [9, 10]
}
```

**chunks() and windows()**:

* Assuming you have a vec with 10 elements
* `.chunks()` will give you four slices: `[0, 1, 2]`, `[3, 4, 5]`, `[6, 7, 8]` and `[9]`
* `.windows()` will give you slices of 3 elements: `[0, 1, 2]`, `[1, 2, 3]`, `[2, 3, 4]`, `[3, 4, 5]`, `[4, 5, 6]`, `[5, 6, 7]`, `[6, 7, 8]`, `[7, 8, 9]`

```rust
fn main() {
    let num_vec = vec![1, 2, 3, 4, 5, 6, 7];

    for chunk in num_vec.chunks(3) {
        println!("{:?}", chunk);
        // Prints:
        // [1, 2, 3]
        // [4, 5, 6]
        // [7]
    }

    println!();

    for window in num_vec.windows(3) {
        println!("{:?}", window);
        // Prints:
        // [1, 2, 3]
        // [2, 3, 4]
        // [3, 4, 5]
        // [4, 5, 6]
        // [5, 6, 7]
    }
}
```

**match_indices()**:

* A combination of `.find()` and `.position()`, except that it doesn't involve returning an
  `Option`. Instead, it returns a `tuple` of the index and the item that matches

```rust
fn main() {
    let some_str = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do.";

    // Destrucutring the tuple into index and item, from match_indices()
    for (index, item) in some_str.match_indices(", ") {
        println!("'{item}' at index {index}");
        // Prints:
        // ', ' at index 26
        // ', ' at index 55
    }
}
```

**peekable()**:

* Peek at the next itema, except that the iterator doesn't move
* It gives an `Option`

```rust
fn main() {
    let just_numbers = vec![1, 5, 100];
    let mut number_iter = just_numbers.iter().peekable();

    println!("One: {}", number_iter.peek().unwrap()); // Prints: One: 1
    println!("Still one: {}", number_iter.peek().unwrap()); // Prints: Still one: 1
    number_iter.next();
    println!("Five: {}", number_iter.peek().unwrap()); // Prints: Five: 5
}
```


