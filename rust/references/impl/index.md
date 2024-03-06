# Impl

* Implement some functionality for a type
* The `impl` keyword is primarily used to define implementations on types
* `trait` implementations are used to implement traits for types, or other traits

## Impl on an enum and a struct

```rust
#[derive(Debug)]
enum AnimalType {
    Cat,
    Dog,
}

#[derive(Debug)]
struct Animal {
    age: u8,
    animal_type: AnimalType,
}

impl Animal {
    // Here, Self means Animal. You can also write Animal instead of Self
    // To the compiler, it is the same thing
    fn new() -> Self {
        // When we write Animal::new(), we always get a cat that is 10 years old
        Self {
            age: 10,
            animal_type: AnimalType::Cat,
        }
    }

    fn check_type(&self) {
        match self.animal_type {
            AnimalType::Dog => println!("The animal is a dog"),
            AnimalType::Cat => println!("The animal is a cat"),
        }
    }

    // Because we are inside impl Animal, &mut self means &mut Animal. Use
    // .change_to_dog() to change the cat to a dog. Taking &mut self lets us change it
    fn change_to_cat(&mut self) {
        self.animal_type = AnimalType::Cat;
        println!("Changed animal to cat! Now it's {self:?}");
    }

    fn change_to_dog(&mut self) {
        self.animal_type = AnimalType::Dog;
        println!("Changed animal to dog! Now it's {self:?}");
    }
}

fn main() {
    // This associated function will create a new Animal for us: a cat, 10 years old
    let mut new_animal = Animal::new();
    new_animal.check_type(); // The animal is a cat
    new_animal.change_to_dog(); // Changed animal to dog! Now it's Animal { age: 10, animal_type: Dog }
    new_animal.check_type(); // The animal is a dog
    new_animal.change_to_cat(); // Changed animal to cat! Now it's Animal { age: 10, animal_type: Cat }
    new_animal.check_type(); // The animal is a cat
}
```

## Impl on an enum

```rust
enum Mood {
    Good,
    Bad,
    Sleepy,
}


impl Mood {
    fn check(&self) {
        match self {
            Mood::Good => println!("Feeling good!"),
            Mood::Bad => println!("Eh, not feeling so good"),
            Mood::Sleepy => println!("Need sleep NOW"),
        }
    }
}

fn main() {
    let my_mood = Mood::Sleepy;
    my_mood.check(); // Prints: Need sleep NOW
}
```
