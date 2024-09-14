# Methods

* Methods are similar to functions
* They are declared with the `fn`
* They can have parameters and a return value
* **The difference**: They are defined within the context of a struct (or an enum or a trait object)
* The benefit of using methods instead of functions, in addition to using method syntax and not
  having to repeat the type of self in every method's signature, **is for organization**

```rust
struct User {
    first_name: String,
    last_name: String,
    age: u64,
}

// To define the function within the context of User, we start an impl (implementation) block
// And change the first (and in this case, only) parameter to be self
impl User {
    fn full_name(&self, nickname: &str) -> String {
        format!("{} {} aka {}", self.first_name, self.last_name, nickname)
    }

    fn can_drive(&self) -> bool {
        if self.age >= 18 {
            true
        } else {
            false
        }
    }
}

fn main() {
    let user1 = User {
        first_name: String::from("Foo"),
        last_name: String::from("Bar"),
        age: 18,
    };

    println!("{}", user1.full_name("Bugsy")); // Returns "Foo Bar aka Foopub"
    println!("This user can drive: {}", user1.can_drive()); // Returns "This user can drive: true"
}
```

