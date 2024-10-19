# Traits: Operator overloading

* Rust provides a set of traits that can be used to define custom behavior for operators
* The `std::ops` module contains the traits that define the behavior of operators
* The `std::ops::Add` trait defines the behavior of the `+` operator
* The `std::ops::Sub` trait defines the behavior of the `-` operator
* The `std::ops::Mul` trait defines the behavior of the `*` operator
* The `std::ops::Div` trait defines the behavior of the `/` operator

## Example: Adding two structs together

```rust
//https://doc.rust-lang.org/std/ops/trait.Add.html
use std::ops::Add;

struct Person {
    first_name: String,
    last_name: String,
}
struct Marriage {
    husband: Person,
    wife: Person,
    location: String,
    date: chrono::NaiveDate,
}

impl Add for Person {
    type Output = Marriage;

    fn add(self, other: Self) -> Self::Output {
        Marriage {
            husband: self,
            wife: other,
            location: "London".to_string(),
            date: chrono::offset::Local::now().date_naive(),
        }
    }
}

fn main() {
    let person1 = Person {
        first_name: "John".to_string(),
        last_name: "Doe".to_string(),
    };

    let person2 = Person {
        first_name: "Jane".to_string(),
        last_name: "Doe".to_string(),
    };

    let marriage = person1 + person2;
    println!(
        "{} got married to {} on {}",
        marriage.husband.first_name, marriage.wife.first_name, marriage.date
    ); // Prints: John got married to Jane on 2022-01-01
}
```

## Example: Adding a vector of structs together

```rust
use std::ops::Add;

#[derive(Debug)]
struct GroceryItem {
    name: String,
    price: f32,
}

#[derive(Debug)]
struct GroceryBill {
    items: Vec<GroceryItem>,
    total: f32,
}

impl GroceryBill {
    fn calculate_total(&mut self) -> f32 {
        let total = self.items.iter().map(|item| item.price).sum();
        self.total = total;
        self.total
    }
}

// Use a generic type GroceryItem to add items to the GroceryBill
// Because we want to add a GroceryItem to a GroceryBill. Not add add a GroceryBill to a GroceryBill
impl Add<GroceryItem> for GroceryBill {
    type Output = GroceryBill;

    fn add(self, other: GroceryItem) -> Self::Output {
        let mut bill = self;
        bill.items.push(other);
        bill
    }
}

fn main() {
    // Create an empty GroceryBill struct
    // We will populate this struct when we call calculate_total()
    let new_bill = GroceryBill {
        items: Vec::new(),
        total: 0.0,
    };

    let carrots = GroceryItem {
        name: "Carrots".to_string(),
        price: 2.2,
    };

    let cottage_cheese = GroceryItem {
        name: "Cottage Cheese".to_string(),
        price: 3.4,
    };

    //let mut bill = new_bill + carrots + cottage_cheese;
    let mut bill = new_bill.add(carrots).add(cottage_cheese);
    println!("Total bill is: {}", bill.calculate_total()); // Prints: Total bill is: 5.6000004
    println!("{:?}", bill); // Prints: GroceryBill { items: [GroceryItem { name: "Carrots", price: 2.2 }, GroceryItem { name: "Cottage Cheese", price: 3.4 }], total: 5.6000004 }
}
```
