# Traits: Dynamic dispatch

* Static dispatch is more common than dynamic dispatch due to its performance benefits and how
  Rust's type system is designed
* Static vs. Dynamic Dispatch: Traits can be used for both static dispatch (at compile time)
  and dynamic dispatch (at runtime) using trait objects (`dyn` Trait)
* Dynamic dispatch is slightly slower than static dispatch, but it is more flexible
* Dynamic dispatch can result in smaller binaries, since the dispatch is being done at runtime,
  rather than at compile time

## Example: Dynamic dispatch

```rust
trait AnimalEating {
    fn eat_food(&self);
}

trait AnimalSound {
    fn make_noise(&self);
}

struct Dog;
impl AnimalEating for Dog {
    fn eat_food(&self) {
        println!("The dog is eating dog food");
    }
}
impl AnimalSound for Dog {
    fn make_noise(&self) {
        println!("The dog is barking");
    }
}

struct Antelope;
impl AnimalEating for Antelope {
    fn eat_food(&self) {
        println!("The antelope is eating grass");
    }
}
impl AnimalSound for Antelope {
    fn make_noise(&self) {
        println!("The antelope is bleating");
    }
}

struct Bear;
impl AnimalEating for Bear {
    fn eat_food(&self) {
        println!("The bear is eating meat");
    }
}
impl AnimalSound for Bear {
    fn make_noise(&self) {
        println!("The bear is roaring");
    }
}

// Use generics: Type Animal is going to be anything that has a noise sound. i.e:
// Animal is of type AnimalSound
fn make_some_noise(animal: &dyn AnimalSound) {
    animal.make_noise();
}

fn main() {
    let dog = Dog;
    make_some_noise(&dog);

    let antelope = Antelope;
    make_some_noise(&antelope);
}
```

## Example: Static dispatch

* The difference between, is `make_some_noise` is generic over the type `AnimalSound`

```rust
// Use generics: Type Animal is going to be anything that has a noise sound. i.e:
// Animal is of type AnimalSound
fn make_some_noise<Animal: AnimalSound>(animal: Animal) {
    animal.make_noise();
}

fn main() {
    let dog = Dog;
    make_some_noise(dog);
}
```

