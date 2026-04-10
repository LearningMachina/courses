# Multiplayer with Rust

## Concept

Multiplayer games need two things that Rust excels at: performance and safety. A game server must handle many connections simultaneously, keep state consistent, and never crash. Rust's ownership model prevents data races at compile time, making it ideal for networked game servers. You will build a real-time multiplayer game from scratch.

## Topics

- Why Rust for multiplayer? Memory safety, concurrency, performance
- Networking fundamentals: TCP vs UDP, latency, packet loss
- Async programming with Tokio: handling many connections
- Game server architecture: authoritative server model
- State synchronisation: sending world updates to all clients
- Client-side prediction and server reconciliation
- Serialisation with serde: encoding game state as binary or JSON
- Lobby systems: matchmaking and session management
- Handling disconnections and reconnections gracefully
- Anti-cheat basics: why the server is the source of truth

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

## Action

1. Create a new Rust project: `cargo new multiplayer_game && cd multiplayer_game`.
2. Add dependencies to `Cargo.toml`: `tokio`, `serde`, `serde_json`, `anyhow`.
3. Replace `src/main.rs` with the server code above.
4. Build and run: `cargo run`.
5. Test by sending JSON messages with `echo '{"Join":{"name":"Player1"}}' | nc -u localhost 7878`.
6. Open a second terminal and join as a second player — observe both receiving state updates.

## Reflection

- Why use UDP instead of TCP for real-time games?
- What happens if two players send a move at the same time? Is it safe?
- How does Rust's ownership system help prevent bugs in concurrent server code?

## Extension

- Add a game tick loop that runs at 30 ticks per second, independent of client messages.
- Implement client-side prediction: the client moves immediately and corrects if the server disagrees.
- Add a simple lobby system where players wait until 2 have joined before the game starts.
