# if let

Take for example, using `match`.  
We weren't doing anything in case of `None` because we were only interested in what happens when
we get a `Some`, but we still had to tell Rust what to do in case of `None`


```rust
fn main() {
    let my_vec = vec![2, 3, 4];
    for index in 0..10 {
        match my_vec.get(index) {
            Some(number) => println!("The number is: {number}"),
            None => {}
        }
    }
}
// Prints:
// The number is: 2
// The number is: 3
// The number is: 4
```

Using `if let` makes more sense and means: "do something if it matches, and don’t do anything if it doesn't."
`if let` is for when you don't care about matching for everything

```rust
fn main() {
    let my_vec = vec![2, 3, 4];
    for index in 0..10 {
        if let Some(number) = my_vec.get(index) {
            println!("The number is: {number}");
        }
    }
}
// Prints:
// The number is: 2
// The number is: 3
// The number is: 4
```

# while let

* Can be interpreted as: while the pattern on the left matches the evaluated expression on the right

```rust
fn main() {
    let weather_vec = vec![
        vec!["Berlin", "cloudy", "5", "-7", "78"],
        vec!["Athens", "sunny", "not humid", "20", "10", "50"],
    ];

    for mut city in weather_vec {
        // In our data, every first item is the city name.
        println!("For the city of {}:", city[0]);

        // Use the pop method to get the last element of the vector
        //
        // while let Some(information) = city.pop() means to keep going until finally
        // city runs out of items and .pop() returns None instead of Some.
        while let Some(information) = city.pop() {
            // Here we try to parse the variable we called information into an i32.
            // This returns a Result. If it’s Ok(number), we will now have a variable
            // called number that we can print
            if let Ok(number) = information.parse::<i32>() {
                println!("The number is: {number}");
            } // Nothing happens here because we only care about getting an Ok. We never see anything that returns an Err.
        }
    }
}
// Prints:
// For the city of Berlin:
// The number is: 78
// The number is: -7
// The number is: 5
// For the city of Athens:
// The number is: 50
// The number is: 10
// The number is: 20
```
