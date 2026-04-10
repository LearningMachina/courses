# Mission de l'étape 1 — API de capteurs en direct

## Objectif

Construire un serveur web Python qui expose les lectures des capteurs du robot sous forme d'une API JSON en direct, consultable depuis n'importe quel appareil sur le réseau local.

## Exigences

- **Framework :** FastAPI (ou Flask)
- **Endpoints :**
  - `GET /status` → JSON avec le nom d'hôte, la température du processeur, le temps de fonctionnement
  - `GET /sensors` → JSON avec au moins trois lectures de capteurs (réelles ou simulées)
  - `WebSocket /ws/live` → envoie un instantané des capteurs chaque seconde
- **Page HTML** servie sur `GET /` qui met à jour automatiquement les valeurs des capteurs dans le navigateur via le WebSocket.
- Le serveur se lie à `0.0.0.0` afin d'être accessible depuis un autre appareil sur le même réseau.
- Le code est commité dans votre dépôt Git `robot-sandbox` sous `stage1-project/`.

## Structure suggérée

```
stage1-project/
├── server.py        # FastAPI app
├── static/
│   └── index.html   # dashboard page
├── requirements.txt
└── README.md        # how to run it
```

## Objectifs supplémentaires

- Ajoutez un endpoint `POST /command` qui accepte `{"cmd": "forward" | "stop" | "backward"}` et enregistre la commande avec un horodatage.
- Affichez un graphique en barres simple des valeurs des capteurs en utilisant du HTML/CSS pur (aucun framework nécessaire).
- Écrivez un script client Python (`client.py`) qui se connecte au WebSocket et enregistre les lectures dans un fichier CSV.

## Réflexion

Faites une démonstration du serveur en fonctionnement au robot et expliquez :

1. Ce qui se passe dans le navigateur lorsque la connexion WebSocket se coupe et se reconnecte.
2. En quoi un endpoint `async def` de FastAPI diffère d'un endpoint `def` classique, et quand cela a de l'importance.
3. Pourquoi vous ne devriez jamais exposer ce serveur directement sur l'internet public sans authentification.
