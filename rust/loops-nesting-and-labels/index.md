# Loops: Nesting and labels

* Loops must be annotated with some `'label`

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

