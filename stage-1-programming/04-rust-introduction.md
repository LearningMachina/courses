# Rust Introduction

## Concept

Rust delivers C-level speed with compile-time memory safety — no garbage collector, no dangling pointers, no data races. This makes it ideal for embedded systems and the kind of concurrent, always-on control software your robot runs. The robot's own control firmware is written in Rust.

## Topics

- Why Rust? Safety without garbage collection, embedded systems
- Ownership, borrowing, lifetimes
- Error handling with `Result` and `Option`
- Traits and generics
- Async/await basics
- How the robot's own control software is written in Rust

## Example

```rust
// Ownership and borrowing
fn print_speed(speed: &f32) {          // borrow, do not take ownership
    println!("Speed: {:.2}", speed);
}

fn clamp(val: f32, lo: f32, hi: f32) -> f32 {
    val.clamp(lo, hi)
}

// Error handling with Result
use std::fs;

fn read_temperature(path: &str) -> Result<f32, std::io::Error> {
    let raw = fs::read_to_string(path)?;
    let millideg: f32 = raw.trim().parse().unwrap_or(0.0);
    Ok(millideg / 1000.0)
}

fn main() {
    let speed = 0.75_f32;
    print_speed(&speed);                // borrow
    println!("{}", speed);              // still valid — not moved

    match read_temperature("/sys/class/thermal/thermal_zone0/temp") {
        Ok(t)  => println!("CPU temp: {:.1} °C", t),
        Err(e) => eprintln!("Could not read temp: {}", e),
    }
}
```

```rust
// Traits — define shared behaviour
trait Drive {
    fn set_speed(&mut self, speed: f32);
    fn stop(&mut self) { self.set_speed(0.0); }  // default impl
}

struct DcMotor { speed: f32 }

impl Drive for DcMotor {
    fn set_speed(&mut self, speed: f32) {
        self.speed = speed.clamp(-1.0, 1.0);
    }
}
```

## Exercise

1. Install Rust via `rustup` if not already present and run `cargo new robot_basics`.
2. Write a function `celsius_to_fahrenheit(c: f32) -> f32` and call it from `main`.
3. Implement a `Sensor` trait with a method `read(&self) -> f32`, then create a `FakeSensor` struct that always returns `42.0`.
4. Make `read_temperature` above work on your machine and print the result.

## Check

Explain to the robot:

- What is ownership, and why does Rust enforce it at compile time rather than at runtime?
- What is the difference between `Option<T>` and `Result<T, E>`? Give a real example of each.
- Why is Rust a better choice than C++ for writing safe concurrent code?
