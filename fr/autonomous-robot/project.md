# Mission de fin d'études du Stage 4 — Comportement autonome original

## Objectif

Concevez, construisez et documentez un comportement autonome original. Présentez-le au robot — il vous demandera d'expliquer chaque couche de la pile.

## Exigences

**Comportement**
- Doit être original : pas une copie directe d'un comportement construit dans les Stages 0–4.
- Doit intégrer au moins trois des cinq couches de capacités :
  1. Contrôle moteur
  2. Perception par capteurs (distance, vision, audio ou IMU)
  3. Raisonnement par IA (LLM, détection d'objets ou classificateur entraîné)
  4. Interaction vocale (STT et/ou TTS)
  5. Tableau de bord web ou API

**Qualité du code**
- Implémenté sous forme de package ROS2 (ou architecture modulaire équivalente).
- Au moins 10 tests unitaires réussis couvrant la logique centrale.
- Pas de nombres magiques codés en dur : tous les paramètres ajustables sont des constantes nommées ou des valeurs de configuration.

**Documentation**
- `README.md` avec : objectif, lien vers la vidéo de démonstration, diagramme d'architecture, instructions d'installation et limitations connues.
- Diagramme d'architecture montrant chaque nœud/composant et les données circulant entre eux.
- Court texte (500 mots) expliquant les décisions de conception et ce que vous feriez différemment la prochaine fois.

**Dépôt**
- Tout le code, la documentation et le jeu de données (si vous avez entraîné un modèle) commité dans votre dépôt `robot-sandbox` sous `graduation-project/`.

## Présentation au robot

Le robot vous posera des questions couvrant les six stages. Soyez prêt à expliquer :

- **Stage 0** — Comment vous vous êtes connecté en SSH au robot pendant le développement et quels outils shell vous ont aidé à déboguer.
- **Stage 1** — Quelles structures de données vous avez utilisées et pourquoi ; comment vous avez géré les erreurs de manière élégante.
- **Stage 2** — Comment les concepts de développement de jeux (boucles temps réel, machines à états, réseau) ont influencé votre conception.
- **Stage 3** — Comment les capteurs sont lus, filtrés et alimentent la boucle de contrôle.
- **Stage 4** — Quel(s) modèle(s) d'IA vous avez utilisé(s), comment vous les avez évalués et quels sont leurs modes de défaillance.
- **Stage 5** — Comment toutes les couches se connectent de bout en bout ; quelle est la latence ; comment vous l'amélioreriez.

## Critères d'évaluation

| Critère                                              | Pondération |
|------------------------------------------------------|-------------|
| Le comportement fonctionne de manière fiable (démo)  | 30 %        |
| Qualité du code et tests                             | 20 %        |
| Clarté de la documentation                           | 20 %        |
| Profondeur de l'explication au robot                 | 20 %        |
| Créativité et ambition                               | 10 %        |

## Idées (choisissez-en une ou inventez la vôtre)

- Un robot qui joue à un simple jeu de quiz : pose des questions à voix haute, écoute les réponses, tient le score.
- Un robot de patrouille de sécurité qui cartographie une pièce, détecte les intrus et envoie une alerte au tableau de bord.
- Un robot qui trie des blocs de couleur en utilisant la vision et une pince.
- Un robot tuteur qui enseigne de manière interactive les commandes Linux du Stage 0 à un autre étudiant.
- Un robot qui apprend un itinéraire en observant un humain le conduire une fois, puis le répète de manière autonome.

---

*Accomplir ce projet signifie que vous avez construit un robot qui pense, voit, parle et agit — entièrement hors ligne, sur du matériel qui tient dans votre main. Ce n'est pas rien.*
