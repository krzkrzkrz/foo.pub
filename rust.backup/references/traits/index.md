# Traits

* Share behaviour using `Traits`
* `Debug`, `Copy`, and `Clone` are all traits

  Rust uses a special syntax called attributes to automatically implement traits
  like Debug because they are so common:

  ```rust
  #[derive(Debug)]
  struct MyStruct {
      number: usize,
  }
  ```

* Commonly used for separation of concerns
* Think of traits is as *powers* or *qualifications*. If a type has a trait, it can do things it couldn't do before

## Simple example

```rust
struct Dog {
    name: String,
}

struct Parrot {
    name: String,
}

trait PetLike {
    fn cuddles(&self) {
        println!("Im cuddling with my human!");
    }
    fn movement(&self);
}

impl PetLike for Dog {
    fn movement(&self) {
        println!("{} the dog is running!", self.name);
    }
}
impl PetLike for Parrot {
    fn movement(&self) {
        println!("{} the parrot is flying!", self.name);
    }
}

fn main() {
    let rover_the_dog = Dog {
        name: "Rover".to_string(),
    };
    let brian_the_parrot = Parrot {
        name: "Brian".to_string(),
    };

    rover_the_dog.cuddles(); // Prints: Im cuddling with my human!
    rover_the_dog.movement(); // Prints: Rover the dog is running!
    brian_the_parrot.cuddles(); // Prints: Im cuddling with my human!
    brian_the_parrot.movement(); // Prints: Brian the parrot is flying!
}
```

## Complex example

```rust
use std::fmt::Debug;

#[derive(Debug)]
struct Orc {
    health: i32,
}

trait MonsterBehavior: Debug {
    fn take_damage(&mut self, damage: i32);
    fn display_self(&self) {
        println!("The monster is now: {self:?}");
    }
}

impl MonsterBehavior for Ogre {
    fn take_damage(&mut self, damage: i32) {
        self.health -= damage;
    }
}

impl MonsterBehavior for Orc {
    fn take_damage(&mut self, damage: i32) {
        self.health -= damage;
    }
}

#[derive(Debug)]
struct Warrior {
    health: i32,
}

#[derive(Debug)]
struct Ranger {
    health: i32,
}

trait DisplayHealth {
    fn health(&self) -> i32;
}

trait FightClose: Debug {
    // Using trait bounds
    fn attack_with_sword<T: MonsterBehavior>(&self, opponent: &mut T) {
        println!("You attack with your sword!");
        opponent.take_damage(10);
        opponent.display_self();
    }

    // Could have been written as: fn attack_with_hand<T: MonsterBehavior>(&self, opponent: &mut T)
    // Implementing trait bounds with where is often easier to read
    fn attack_with_hand<T>(&self, opponent: &mut T)
    where
        T: MonsterBehavior + Debug,
    {
        println!("You attack with your hand!");
        opponent.take_damage(2);
        opponent.display_self();
    }
}

trait FightFromDistance: Debug {
    // Using trait bounds
    fn attack_with_bow<T: MonsterBehavior>(&self, opponent: &mut T, distance: u32) {
        println!("You attack with your bow!");
        if distance < 10 {
            opponent.take_damage(10);
        } else {
            println!("Too far away!");
        }
        opponent.display_self();
    }

    // Could have been written as: fn attack_with_hand<T: MonsterBehavior>(&self, opponent: &mut T)
    // Implementing trait bounds with where is often easier to read
    fn attack_with_rock<T>(&self, opponent: &mut T, distance: u32)
    where
        T: MonsterBehavior + Debug,
    {
        println!("You attack with a rock!");
        if distance < 3 {
            opponent.take_damage(4);
        } else {
            println!("Too far away!");
        }
        opponent.display_self();
    }
}

impl FightClose for Warrior {}
impl FightFromDistance for Ranger {}

fn main() {
    let radagast = Warrior { health: 60 };
    let aragorn = Ranger { health: 80 };
    let mut uruk_hai = Orc { health: 40 };

    radagast.attack_with_sword(&mut uruk_hai); // Prints: You attack with your sword! The monster is now: Orc { health: 30 }
    aragorn.attack_with_bow(&mut uruk_hai, 8); // Prints: You attack with your bow! The monster is now: Orc { health: 20 }
}
```

## Implement a Display trait

The code below uses the `Debug` trait, but `Debug` print is not exactly the prettiest way to print

```rust
#[derive(Debug)]
struct Cat {
    name: String,
    age: u8,
}

fn main() {
    let mr_mantle = Cat {
        name: "Reggie Mantle".to_string(),
        age: 4,
    };
    println!("Mr. Mantle is a {mr_mantle:?}"); // Prints: Mr. Mantle is a Cat { name: "Reggie Mantle", age: 4 }
}
```

Instead, we can implement the `Display` trait to make it look better

```rust
use std::fmt;

#[derive(Debug)]
struct Cat {
    name: String,
    age: u8,
}

// In the documentation for the Display trait (https://doc.rust-lang.org/std/fmt/ trait.Display.html),
// we can see the general information for Display
impl fmt::Display for Cat {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{} is a cat who is {} years old", self.name, self.age)
    }
}

fn main() {
    let mr_mantle = Cat {
        name: "Reggie Mantle".to_string(),
        age: 4,
    };
    println!("{mr_mantle}"); // Prints: Reggie Mantle is a cat who is 4 years old
}
```



