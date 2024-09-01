# Interior mutability

* Interior mutability means having a little bit of mutability on the inside (the interior)
* There are also some ways to change them without the word `mut`
  For example: Change values inside of a `struct` that is itself immutable

## Cell

* Simplest way to use interior mutability in Rust is called `Cell`
* The signature of Cell is just `Cell<T>`
* Cell has a method called `.set()` where you can change the value
* Also has a methoded called `.get()` where you can get the value

```rust
use std::cell::Cell;

#[derive(Debug)]
struct PhoneModel { // Immutable struct
    company_name: String,
    model_name: String,
    screen_size: f32,
    memory: usize,
    date_issued: u32,
    on_sale: Cell<bool>,
}

impl PhoneModel {
    fn make_not_on_sale(&self) {
        self.on_sale.set(false);
    }
}

fn main() {
    let super_phone_3000 = PhoneModel {
        company_name: "YY Electronics".to_string(),
        model_name: "Super Phone 3000".to_string(),
        screen_size: 7.5,
        memory: 4_000_000,
        date_issued: 2020,
        on_sale: Cell::new(true),
    };

    super_phone_3000.make_not_on_sale();
    println!("{super_phone_3000:#?}"); // Prints:
                                       //PhoneModel {
                                       // company_name: "YY Electronics",
                                       // model_name: "Super Phone 3000",
                                       // screen_size: 7.5,
                                       // memory: 4000000,
                                       // date_issued: 2020,
                                       // on_sale: Cell {
                                       //    value: false,
                                       // },

    println!("{super_phone_3000.on_sale.get()}"); // Prints: false
}
```

## RefCell

* Stands for "reference cell" and is a bit similar not only to a `Cell` but also to regular references

**A simple example**

```rust
use std::cell::RefCell;

fn main() {
    // Create a RefCell containing an integer
    // data is an immutable variable
    let data = RefCell::new(5);

    // Borrow the RefCell immutably and print its value
    println!("Initial value: {}", *data.borrow_mut()); // Prints: Initial value: 5

    // Mutate the data behind the RefCell using interior mutability
    *data.borrow_mut() += 1;

    // Borrow the RefCell immutably again and print its updated value
    println!("Updated value: {}", *data.borrow_mut()); // Prints: Updated value: 6
}
```

**A more complex example**

```rust
use std::cell::RefCell;

#[derive(Debug)]
struct User { // Immutable struct
    id: u32,
    year_registered: u32,
    username: String,
    active: RefCell<bool>,
}

fn main() {
    let user_1 = User {
        id: 1,
        year_registered: 2020,
        username: "User 1".to_string(),
        active: RefCell::new(true),
    };

    *user_1.active.borrow_mut() = false;

    println!("{:?}", user_1.active); // Prints: RefCell { value: true }
    println!("{:?}", user_1.active.borrow()); // Prints: false
}
```

