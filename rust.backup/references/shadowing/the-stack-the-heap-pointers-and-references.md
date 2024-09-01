# The stack, the heap, pointers, and references

* The stack is very fast, but the heap is not so fast. It’s not super slow either,
  but the stack is usually faster.
* The stack is fast because it is like a stack: memory for a variable gets stacked
  on top of the last one, right next to it. When a function is done, it removes the
  value of the variables starting from the last one that was added, and now the memory
  is freed again. Some people compare the stack to a stack of dishes: you put one on
  top of the other, and if you want to unstack them, you take the top one off first,
  then the next top one, and so on. The dishes are all right on top of each other, so
  they are quick to find. But you can’t use the stack all the time.
* Simple variables like `i32` can go on the stack because we know their exact size.
  You always know that an `i32` is 4 bytes because 32 bits = 4 bytes. So, `i32` can
  always go on the stack.

## Pointers

* Pointers are like a table of contents in a book:

```
Chapters | Page
---------|-----
My cat   | 1
My dog   | 5
My fish  | 18
```

* My cat is on page 1 (it points to page 1).
* My dog is on page 5 (it points to page 5).
* Etc

## References

The pointer you usually see in Rust is called a *reference*.

A *reference* means you borrow the value, but you don’t own it. It’s the same
as our book: the table of contents doesn’t own the information. The chapters own
the information

* Common kind of pointer is called a reference: `&`
* No special capabilities other than referring to data
* References are pointers that only borrow data

```rust
let my_variable = 8; // Makes a regular variable

// Makes a reference to the data held by my_variable
// Can also be read as my_reference is a reference to my_variable
// my_variable is still the owner of the data
let my_reference = &my_variable
```

