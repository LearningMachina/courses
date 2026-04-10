# Introduction à Rust

## Concept

Rust offre des performances de niveau C avec une sécurité mémoire vérifiée à la compilation — pas de ramasse-miettes, pas de pointeurs pendants, pas de courses de données. Cela en fait un choix idéal pour les systèmes embarqués et le type de logiciel de contrôle concurrent et permanent qu'exécute votre robot. Le firmware de contrôle du robot lui-même est écrit en Rust.

## Thèmes

- Pourquoi Rust ? Sécurité sans ramasse-miettes, systèmes embarqués
- Propriété (ownership), emprunt (borrowing), durées de vie (lifetimes)
- Gestion des erreurs avec `Result` et `Option`
- Traits et génériques
- Bases de async/await
- Comment le logiciel de contrôle du robot est écrit en Rust

## Code

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

## Action

1. Installez Rust via `rustup` si ce n'est pas déjà fait et exécutez `cargo new robot_basics`.
2. Écrivez une fonction `celsius_to_fahrenheit(c: f32) -> f32` et appelez-la depuis `main`.
3. Implémentez un trait `Sensor` avec une méthode `read(&self) -> f32`, puis créez une structure `FakeSensor` qui retourne toujours `42.0`.
4. Faites fonctionner `read_temperature` ci-dessus sur votre machine et affichez le résultat.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Qu'est-ce que la propriété (ownership), et pourquoi Rust l'applique-t-il à la compilation plutôt qu'à l'exécution ?
- Quelle est la différence entre `Option<T>` et `Result<T, E>` ? Donnez un exemple concret de chacun.
- Pourquoi Rust est-il un meilleur choix que le C++ pour écrire du code concurrent sûr ?

## Extension

Modifiez le code Rust pour changer le comportement de sécurité du robot :

1. Modifiez le `DcMotor` pour limiter la vitesse entre -1.0 et 1.0 — le robot dispose désormais de limites de sécurité intégrées appliquées à la compilation.
2. Ajoutez une méthode `emergency_stop()` qui met la vitesse à 0 et retourne un `Result` indiquant si l'arrêt s'est déroulé correctement — le robot peut désormais signaler son propre état de sécurité.
3. Implémentez une seconde structure qui enveloppe `DcMotor` et ajoute une rampe de vitesse (ne dépassant jamais 0.1 par appel) — les mouvements du robot deviennent fluides. Le système de propriété de Rust garantit qu'un seul contrôleur existe à la fois.
