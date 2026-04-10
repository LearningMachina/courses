# Multijoueur avec Rust

## Concept

Les jeux multijoueurs nécessitent deux choses dans lesquelles Rust excelle : la performance et la sécurité. Un serveur de jeu doit gérer de nombreuses connexions simultanément, maintenir la cohérence de l'état et ne jamais planter. Le modèle de propriété de Rust empêche les courses de données à la compilation, ce qui en fait un choix idéal pour les serveurs de jeux en réseau. Vous construirez un jeu multijoueur en temps réel de zéro.

## Thèmes

- Pourquoi Rust pour le multijoueur ? Sécurité mémoire, concurrence, performance
- Fondamentaux du réseau : TCP vs UDP, latence, perte de paquets
- Programmation asynchrone avec Tokio : gérer de nombreuses connexions
- Architecture de serveur de jeu : modèle de serveur autoritaire
- Synchronisation de l'état : envoi des mises à jour du monde à tous les clients
- Prédiction côté client et réconciliation côté serveur
- Sérialisation avec serde : encodage de l'état du jeu en binaire ou JSON
- Systèmes de lobby : matchmaking et gestion des sessions
- Gestion des déconnexions et reconnexions avec élégance
- Bases de l'anti-triche : pourquoi le serveur est la source de vérité

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

1. Créez un nouveau projet Rust : `cargo new multiplayer_game && cd multiplayer_game`.
2. Ajoutez les dépendances dans `Cargo.toml` : `tokio`, `serde`, `serde_json`, `anyhow`.
3. Remplacez `src/main.rs` par le code serveur ci-dessus.
4. Compilez et exécutez : `cargo run`.
5. Testez en envoyant des messages JSON avec `echo '{"Join":{"name":"Player1"}}' | nc -u localhost 7878`.
6. Ouvrez un second terminal et rejoignez en tant que second joueur — observez que les deux reçoivent les mises à jour d'état.

## Réflexion

- Pourquoi utiliser UDP au lieu de TCP pour les jeux en temps réel ?
- Que se passe-t-il si deux joueurs envoient un mouvement en même temps ? Est-ce sûr ?
- Comment le système de propriété de Rust aide-t-il à prévenir les bugs dans le code serveur concurrent ?

## Extension

- Ajoutez une boucle de tick de jeu qui s'exécute à 30 ticks par seconde, indépendamment des messages des clients.
- Implémentez la prédiction côté client : le client se déplace immédiatement et corrige si le serveur n'est pas d'accord.
- Ajoutez un système de lobby simple où les joueurs attendent que 2 joueurs aient rejoint avant que la partie commence.
