# Traits

* `trait` implementations are used to implement traits for types, or other traits
```rust
// Define a trait called `Shape` with a method signature `area`
trait Shape {
    fn area(&self) -> f64;
}

// Implement the `Shape` trait for a `Circle` struct
struct Circle {
    radius: f64,
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * self.radius * self.radius
    }
}

// Implement the `Shape` trait for a `Rectangle` struct
struct Rectangle {
    width: f64,
    height: f64,
}

impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
}

fn main() {
    let circle = Circle { radius: 10.0 };
    let rectangle = Rectangle { width: 5.0, height: 10.0 };

    println!("Circle area: {}", circle.area()); // Returns: Circle area: 314.1592653589793
    println!("Rectangle area: {}", rectangle.area()); // Returns: Rectangle area: 50
}
```
