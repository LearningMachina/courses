# Multiplayer mit Rust

## Konzept

Multiplayer-Spiele brauchen zwei Dinge, in denen Rust hervorragend ist: Performance und Sicherheit. Ein Spieleserver muss viele Verbindungen gleichzeitig verwalten, den Zustand konsistent halten und darf niemals abstürzen. Rusts Ownership-Modell verhindert Data Races zur Kompilierzeit, was es ideal für vernetzte Spieleserver macht. Du baust ein Echtzeit-Multiplayer-Spiel von Grund auf.

## Themen

- Warum Rust für Multiplayer? Speichersicherheit, Nebenläufigkeit, Performance
- Netzwerk-Grundlagen: TCP vs UDP, Latenz, Paketverlust
- Asynchrone Programmierung mit Tokio: viele Verbindungen handhaben
- Spieleserver-Architektur: autoritatives Server-Modell
- Zustandssynchronisation: Welt-Updates an alle Clients senden
- Client-seitige Vorhersage und Server-Abgleich
- Serialisierung mit serde: Spielzustand als Binär oder JSON kodieren
- Lobby-Systeme: Matchmaking und Sitzungsverwaltung
- Verbindungsabbrüche und Wiederverbindungen elegant behandeln
- Anti-Cheat-Grundlagen: Warum der Server die Wahrheitsquelle ist

## Code

```rust
use tokio::net::UdpSocket;
use tokio::sync::broadcast;
use serde::{Serialize, Deserialize};
use std::collections::HashMap;
use std::net::SocketAddr;

#[derive(Serialize, Deserialize, Clone, Debug)]
struct PlayerState {
    x: f32,
    y: f32,
    name: String,
}

#[derive(Serialize, Deserialize, Debug)]
enum ClientMessage {
    Join { name: String },
    Move { dx: f32, dy: f32 },
    Leave,
}

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let socket = UdpSocket::bind("0.0.0.0:7878").await?;
    let mut players: HashMap<SocketAddr, PlayerState> = HashMap::new();
    let mut buf = [0u8; 1024];

    println!("Game server running on :7878");

    loop {
        let (len, addr) = socket.recv_from(&mut buf).await?;
        let msg: ClientMessage = serde_json::from_slice(&buf[..len])?;

        match msg {
            ClientMessage::Join { name } => {
                players.insert(addr, PlayerState { x: 0.0, y: 0.0, name });
                println!("Player joined from {addr}");
            }
            ClientMessage::Move { dx, dy } => {
                if let Some(player) = players.get_mut(&addr) {
                    player.x += dx;
                    player.y += dy;
                }
            }
            ClientMessage::Leave => {
                players.remove(&addr);
            }
        }

        // Broadcast world state to all players
        let state = serde_json::to_vec(&players.values().collect::<Vec<_>>())?;
        for peer in players.keys() {
            socket.send_to(&state, peer).await?;
        }
    }
}
```

## Aktion

1. Erstelle ein neues Rust-Projekt: `cargo new multiplayer_game && cd multiplayer_game`.
2. Füge Abhängigkeiten zu `Cargo.toml` hinzu: `tokio`, `serde`, `serde_json`, `anyhow`.
3. Ersetze `src/main.rs` mit dem obigen Server-Code.
4. Baue und starte: `cargo run`.
5. Teste durch Senden von JSON-Nachrichten mit `echo '{"Join":{"name":"Spieler1"}}' | nc -u localhost 7878`.
6. Öffne ein zweites Terminal und tritt als zweiter Spieler bei — beobachte, wie beide Zustandsaktualisierungen erhalten.

## Reflexion

- Warum UDP statt TCP für Echtzeit-Spiele verwenden?
- Was passiert, wenn zwei Spieler gleichzeitig einen Zug senden? Ist das sicher?
- Wie hilft Rusts Ownership-System, Fehler in nebenläufigem Server-Code zu verhindern?

## Erweiterung

- Füge eine Spiel-Tick-Schleife hinzu, die mit 30 Ticks pro Sekunde läuft, unabhängig von Client-Nachrichten.
- Implementiere Client-seitige Vorhersage: Der Client bewegt sich sofort und korrigiert, wenn der Server anderer Meinung ist.
- Füge ein einfaches Lobby-System hinzu, in dem Spieler warten, bis 2 beigetreten sind, bevor das Spiel beginnt.
