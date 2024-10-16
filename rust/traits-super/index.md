# Traits: Super

* A `super trait` is a trait that requires another trait to be implemented
* `Hover` is a super trait of `Drive` and `Float`

```rust
trait Drive {
    fn drive(&self) {
        println!("Driving on land");
    }
}

trait Float {
    fn float(&self) {
        println!("Floating on water");
    }
}

trait Hover: Drive + Float {
    fn hover(&self) {
        println!("Hovering on both land and water");
    }
}

struct Car;
impl Drive for Car {}

struct Boat;
impl Float for Boat {}

#[derive(Copy, Clone)]
struct HoverBoard;
impl Hover for HoverBoard {}
impl Drive for HoverBoard {}
impl Float for HoverBoard {}

fn road_trip(vehicle: impl Drive) {
    vehicle.drive();
}

fn float_on_water(vehicle: impl Float) {
    vehicle.float();
}

fn traverse_frozen_lake(vehicle: impl Hover) {
    vehicle.hover();
}
fn main() {
    let car = Car;
    let boat = Boat;
    let hover_board = HoverBoard;

    road_trip(car); // Prints: Driving on land
    float_on_water(boat); // Prints: Floating on water

    road_trip(hover_board); // Prints: Driving on land
    float_on_water(hover_board); // Prints: Floating on water
    traverse_frozen_lake(hover_board); // Prints: Hovering on both land and water
}
```

