# Match guards

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
## A more complex example
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

