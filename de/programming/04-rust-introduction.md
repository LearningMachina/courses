# Rust-Einführung

## Konzept

Rust liefert C-Level-Geschwindigkeit mit Speichersicherheit zur Kompilierzeit — kein Garbage Collector, keine hängenden Pointer, keine Data Races. Das macht es ideal für eingebettete Systeme und die Art von nebenläufiger, immer aktiver Steuerungssoftware, die dein Roboter ausführt. Die eigene Steuerfirmware des Roboters ist in Rust geschrieben.

## Themen

- Warum Rust? Sicherheit ohne Garbage Collection, eingebettete Systeme
- Ownership, Borrowing, Lifetimes
- Fehlerbehandlung mit `Result` und `Option`
- Traits und Generics
- Async/Await-Grundlagen
- Wie die eigene Steuerungssoftware des Roboters in Rust geschrieben ist

## Code

```rust
// Ownership und Borrowing
fn print_speed(speed: &f32) {          // ausleihen, nicht Besitz übernehmen
    println!("Speed: {:.2}", speed);
}

fn clamp(val: f32, lo: f32, hi: f32) -> f32 {
    val.clamp(lo, hi)
}

// Fehlerbehandlung mit Result
use std::fs;

fn read_temperature(path: &str) -> Result<f32, std::io::Error> {
    let raw = fs::read_to_string(path)?;
    let millideg: f32 = raw.trim().parse().unwrap_or(0.0);
    Ok(millideg / 1000.0)
}

fn main() {
    let speed = 0.75_f32;
    print_speed(&speed);                // ausleihen
    println!("{}", speed);              // immer noch gültig — nicht verschoben

    match read_temperature("/sys/class/thermal/thermal_zone0/temp") {
        Ok(t)  => println!("CPU-Temp: {:.1} °C", t),
        Err(e) => eprintln!("Konnte Temperatur nicht lesen: {}", e),
    }
}
```

```rust
// Traits — gemeinsames Verhalten definieren
trait Drive {
    fn set_speed(&mut self, speed: f32);
    fn stop(&mut self) { self.set_speed(0.0); }  // Standard-Implementierung
}

struct DcMotor { speed: f32 }

impl Drive for DcMotor {
    fn set_speed(&mut self, speed: f32) {
        self.speed = speed.clamp(-1.0, 1.0);
    }
}
```

## Aktion

1. Installiere Rust über `rustup`, falls noch nicht vorhanden, und führe `cargo new robot_basics` aus.
2. Schreibe eine Funktion `celsius_to_fahrenheit(c: f32) -> f32` und rufe sie aus `main` auf.
3. Implementiere einen `Sensor`-Trait mit einer Methode `read(&self) -> f32`, dann erstelle ein `FakeSensor`-Struct, das immer `42.0` zurückgibt.
4. Bringe die obige `read_temperature`-Funktion auf deiner Maschine zum Laufen und gib das Ergebnis aus.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist Ownership, und warum erzwingt Rust es zur Kompilierzeit statt zur Laufzeit?
- Was ist der Unterschied zwischen `Option<T>` und `Result<T, E>`? Gib jeweils ein echtes Beispiel.
- Warum ist Rust eine bessere Wahl als C++ für das Schreiben von sicherem nebenläufigen Code?

## Erweiterung

Verändere den Rust-Code, um das Sicherheitsverhalten des Roboters zu ändern:

1. Ändere den `DcMotor`, sodass die Geschwindigkeit zwischen -1.0 und 1.0 begrenzt wird — der Roboter hat jetzt eingebaute Sicherheitsgrenzen, die zur Kompilierzeit erzwungen werden.
2. Füge eine `emergency_stop()`-Methode hinzu, die die Geschwindigkeit auf 0 setzt und ein `Result` zurückgibt, das anzeigt, ob der Stopp sauber war — der Roboter kann jetzt seinen eigenen Sicherheitszustand melden.
3. Implementiere ein zweites Struct, das `DcMotor` umhüllt und eine Geschwindigkeitsrampe hinzufügt (nie mehr als 0,1 pro Aufruf springen) — die Bewegungen des Roboters werden sanft. Rusts Ownership stellt sicher, dass nur ein Controller gleichzeitig existiert.
