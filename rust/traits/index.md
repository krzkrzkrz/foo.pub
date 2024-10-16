# Traits

* Traits allow you to define a set of methods that a type must implement, enabling you to
* decouple the interface from the implementation. Use traits when you want to define a common
* interface for multiple types
* `trait` implementations are used to implement traits for types, or other traits
* A trait defines methods but doesn't provide an implementation (though it can provide default
  implementations). Any type that implements the trait must define the specified methods or use
  the default if provided
* Static vs. Dynamic Dispatch: Traits can be used for both static dispatch (at compile time)
  and dynamic dispatch (at runtime) using trait objects (`dyn` Trait)

## Example

* The `Greet` trait defines a method `say_hello` that returns a `String`
* The `Greet` trait also defines a method `does_another_action` that returns a `String`
* Since `Person` and `Cat` both implement the `Greet` trait, they can call their own `say_hello` method
* `IntrovertedParrot` implements the `Greet` trait, but does not define its own `say_hello` method,
  so it uses the default implementation provided by the `Greet` trait (i.e. `say_hello` and `does_another_action` are called)

```rust
trait Greet {
    fn say_hello(&self) -> String {
        //String::from("Looks and nods")
        format!("Looks {}", self.does_another_action())
    }

    fn does_another_action(&self) -> String {
        String::from("and nods")
    }
}

struct Person;
impl Greet for Person {
    fn say_hello(&self) -> String {
        String::from("Waves")
    }
}

struct Cat;
impl Greet for Cat {
    fn say_hello(&self) -> String {
        String::from("Meow")
    }
}

struct IntrovertedParrot;
impl Greet for IntrovertedParrot {}

fn main() {
    let person = Person;
    println!("{:?}", person.say_hello()); // Prints: "Hello"

    let cat = Cat;
    println!("{:?}", cat.say_hello()); // Prints: "Meow"

    let introverted_parrot = IntrovertedParrot;
    println!("{:?}", introverted_parrot.say_hello()); // Prints: "Looks and nods"
}
```

