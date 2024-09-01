# ? operator

* There is an even shorter way to deal with `Result`, shorter than `match` and even shorter than `if let`
* Give what is inside the `Result` if it is `Ok`
* Pass the error back if it is `Err` (this is called an early return).
* The `?` operator works with `Option`, too, although the majority of the time you see it used to handle a `Result`

```rust
use std::num::ParseIntError;

fn parse_and_log_str(input: &str) -> Result<i32, ParseIntError> {
    // This is the key line in the function. If the &str parses successfully, you will
    // have a variable called parsed_number that is an i32. If it doesn’t parse successfully,
    // the function ends here and returns an error.
    // i.e. an early return
    let parsed_number = input.parse::<i32>()?;

    // If the parse is an error, the next 2 lines will not run
    println!("Number parsed successfully into {parsed_number}");

    // We need to wrap it in Ok because the return value is Result<i32, ParseIntError>, not i32.
    Ok(parsed_number)
}

fn main() {
    let str_vec = vec!["Seven", "8", "9.0", "nice", "6060"];
    for item in str_vec {
        let parsed = parse_and_log_str(item);
        println!("Result: {parsed:?}");
    }
}
// Prints:
// Result: Err(ParseIntError { kind: InvalidDigit })
// Number parsed successfully into 8
// Result: Ok(8)
// Result: Err(ParseIntError { kind: InvalidDigit })
// Result: Err(ParseIntError { kind: InvalidDigit })
// Number parsed successfully into 6060
// Result: Ok(6060)

```


