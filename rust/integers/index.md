# Integers

* Signed integers are positive or negative numbers
* Unsigned are only positive numbers
* Integers default to `i32`

| Length  | Signed            | Unsigned        |
|---------|-------------------|-----------------|
| 8-bit   | i8 (-128 to 127)  | u8 (0 to 255)   |
| 16-bit  | i16               | u16             |
| 32-bit  | i32               | u32             |
| 64-bit  | i64               | u64             |
| 128-bit | i128              | u128            |
| arch    | isize             | usize           |

```rust
let i: i16 = 5;
let n = 5; // Defaults to i32
```

