# LearningMachina — Curriculum Incarné

Un parcours structuré du débutant complet au constructeur de robots autonomes.

Contrairement aux cours traditionnels, LearningMachina ne sépare pas la théorie de la pratique.
**Tout ce que l'élève apprend contrôle directement le robot — et crée un comportement visible et physique.**

Le robot n'est pas un simple outil.
C'est le **médium d'apprentissage**.

Tout le matériel est conçu pour être enseigné de manière interactive par le robot lui-même —
posez-lui une question à tout moment, et il expliquera, démontrera et vous guidera.

---

## Principe fondamental d'apprentissage

> **Le code n'est pas un concept abstrait — c'est un comportement.**

Chaque concept que l'élève apprend se traduit par :
- Du mouvement
- De la réaction
- De l'interaction
- De la prise de décision

L'élève n'écrit pas simplement du code.
Il **apprend au robot comment se comporter**.

---

## Format des leçons

Chaque leçon suit une structure cohérente et incarnée :

1. **Concept**
   Le robot explique l'idée en 2–3 phrases simples.

2. **Code**
   L'élève écrit ou modifie un petit morceau de code à l'aide du SDK du robot.

3. **Action (Retour physique)**
   Le robot exécute le code immédiatement :
   - Se déplace
   - Parle
   - S'illumine
   - Réagit à l'environnement

4. **Réflexion**
   L'élève explique ce qui a changé dans le comportement du robot.

5. **Extension**
   L'élève modifie le code pour changer à nouveau le comportement du robot.

Toutes les leçons sont de simples fichiers Markdown. Le LLM du robot les lit comme contexte,
permettant à l'élève de poser toute question de suivi et d'obtenir une réponse fondée.

---

## Le SDK du Robot (Abstraction Centrale)

Tous les langages de programmation utilisent une interface unifiée et simple pour contrôler le robot.

Les élèves ne commencent pas par des problèmes de programmation abstraits.
Ils commencent par **contrôler un système physique**.

Cela permet :
- Un retour immédiat
- Une compréhension intuitive
- Une cohérence entre les langages (Python, C++, Rust)

---

## Modèle d'apprentissage

Au lieu de :
> Théorie → Exercices → Tests

LearningMachina utilise :
> **Comportement → Compréhension → Contrôle**

Les élèves apprennent en :
- Observant ce que fait le robot
- Modifiant le code et voyant des résultats différents
- Construisant des comportements de plus en plus complexes

---

## Pistes comportementales

En parallèle de la progression principale, les élèves développent des compétences selon trois pistes :

### Mouvement
- Navigation
- Contrôle de précision
- Mouvement fluide

### Interaction
- Réponses vocales
- Détection visuelle
- Interaction humaine

### Intelligence
- Prise de décision
- Apprentissage à partir de données
- Comportement autonome

---

## Stage 0 — Environnement & Premier Contrôle

*Donne vie au robot.*

Les élèves apprennent les bases de Linux, l'utilisation du terminal, les fichiers et les processus —
mais chaque interaction a un **résultat physique** :
- Exécuter un script → le robot réagit
- Boucle → mouvement ou signal répété
- Commande → sortie visible sur le robot

> **Objectif du stage :**
> Comprendre que les commandes sur un ordinateur peuvent directement contrôler une machine.

**La ligne de commande Linux**
- Qu'est-ce qu'un système d'exploitation ? Qu'est-ce qui rend Linux différent ?
- Le terminal : navigation dans le système de fichiers (cd, ls, pwd, mkdir, rm)
- Lire et écrire des fichiers (cat, nano, less, echo, redirection)
- Les processus : exécuter des programmes, les mettre en arrière-plan, les arrêter (ps, top, kill, &)
- Les permissions : ce que signifie rwx et pourquoi c'est important
- La gestion des paquets : installer des logiciels avec apt
- Les bases du scripting shell : variables, boucles, if/else en bash
- SSH : se connecter au robot à distance depuis une autre machine

**Le matériel du robot**
- Qu'est-ce qu'un Jetson Orin Nano ? CPU vs GPU expliqué simplement
- Le boîtier compatible LEGO : comment il fonctionne et ce qui peut être attaché
- Ports et connecteurs : USB, GPIO, UART, I2C, SPI, caméra
- La carte de pilotage moteur : ce qu'elle fait et comment la câbler
- Alimenter le robot : batterie, rails de tension, toujours éteindre avant de câbler
- Première interaction : allumer une LED depuis la ligne de commande

**Outils que vous utiliserez chaque jour**
- Éditeurs de texte : nano pour les modifications rapides, VS Code pour les vrais projets
- Les bases de Git : init, add, commit, push, pull — pourquoi le contrôle de version est important
- L'environnement Python : ce qu'est un environnement virtuel et pourquoi l'utiliser
- Exécuter votre premier script Python sur le robot

> **Mission du stage 0 :** Écrivez un script bash qui fait rapporter au robot ses
> signes vitaux — température du CPU, niveau de batterie et une salutation parlée —
> toutes les 5 secondes. Observez le robot prendre vie à chaque itération de boucle.

---

## Stage 1 — La programmation comme comportement

*Apprenez à penser comme un ordinateur — en contrôlant un robot.*

Chaque concept est lié à un comportement :
- Changer une variable change le mouvement
- Les conditions créent des réactions
- Les boucles créent des motifs
- Les fonctions créent des comportements réutilisables

> **Objectif du stage :**
> Comprendre que le code définit comment un système se comporte dans le temps.

**Les bases de Python** (commencez ici)
- Variables, types de données, opérateurs
- Flux de contrôle : if/else, boucles for, boucles while
- Fonctions : définition, appel, paramètres, valeurs de retour
- Listes, dictionnaires, tuples
- E/S de fichiers : lire et écrire des fichiers texte
- Modules et imports : utiliser la bibliothèque standard
- Introduction aux classes et aux objets
- Gestion des erreurs : try/except, écrire du code défensif

**Structures de données & algorithmes**
- Pourquoi l'efficacité compte sur du matériel limité
- Listes vs dictionnaires : quand utiliser quoi
- Piles, files et pourquoi les robots les utilisent
- Tri et recherche : intuition derrière les algorithmes courants
- Notation Big-O : un modèle mental simple (pas de maths nécessaires)
- Récursion : ce que c'est, quand ça aide, quand ça nuit

**Fondamentaux du C++**
- Pourquoi le C++ ? Vitesse, contrôle mémoire, standard industriel en robotique
- Types, pointeurs, références
- Flux de contrôle et fonctions
- Structs et classes
- Compilation, édition de liens, systèmes de build (bases de CMake)
- Écrire un programme simple de contrôle moteur en C++

**Introduction à Rust**
- Pourquoi Rust ? Sécurité sans ramasse-miettes, systèmes embarqués
- Ownership, borrowing, lifetimes
- Gestion des erreurs avec Result et Option
- Traits et generics
- Les bases de async/await
- Comment le logiciel de contrôle du robot est écrit en Rust

**Réseaux & APIs**
- Comment les ordinateurs communiquent : adresses IP, ports, bases du HTTP
- Écrire un serveur HTTP simple en Python (Flask / FastAPI)
- APIs REST : envoyer et recevoir du JSON
- WebSockets : communication en temps réel entre le robot et le navigateur
- Construire un tableau de bord web minimal pour les données de capteurs du robot
- Node.js en une leçon : quand utiliser JavaScript côté serveur

> **Mission du stage 1 :** Construisez un serveur web Python qui expose les
> lectures de capteurs du robot sous forme d'API JSON en direct, consultable depuis
> n'importe quel appareil sur le réseau local. L'état du robot devient visible en temps réel.

---

## Stage 2 — Développement de jeux

*Construisez des jeux pour maîtriser les systèmes temps réel.*

Les jeux sont le pont entre la programmation et la robotique — les deux sont des systèmes
temps réel avec des entrées, un état et des sorties. Les élèves apprennent :
- Les jeux 2D en Python avec Pygame
- Le rendu 3D en C++ avec OpenGL
- Le réseau multijoueur en Rust
- Les patrons d'architecture de jeux (ECS, machines à états)

> **Objectif du stage :**
> Comprendre les systèmes temps réel, le rendu et le multijoueur en réseau — des compétences
> directement transférables à la robotique et à l'IA.

**Jeux 2D avec Python**
- Boucles de jeu : entrée → mise à jour → rendu
- Les bases de Pygame : fenêtres, événements, dessin
- Sprites, détection de collision, physique simple
- Effets sonores, tile maps, gestion de l'état du jeu

**Jeux 3D avec C++**
- Le pipeline graphique : vertices → shaders → écran
- Les bases d'OpenGL : fenêtres, buffers, VAOs
- Shaders GLSL : vertex et fragment
- Transformations, caméras, textures, éclairage

**Multijoueur avec Rust**
- Réseau : TCP vs UDP, latence, perte de paquets
- Programmation asynchrone avec Tokio
- Architecture de serveur autoritatif
- Synchronisation d'état, prédiction côté client

**Architecture & design de jeux**
- Pattern Entity-Component-System (ECS)
- Machines à états de jeu
- Systèmes d'événements, gestion des assets
- Intégration entre langages

> **Mission du stage 2 :** Construisez un jeu multijoueur complet — un serveur Rust,
> un client Python/Pygame, synchronisation d'état en temps réel et au moins une mécanique de jeu.

---

## Stage 3 — Robotique & le monde physique

*Faites en sorte que le robot perçoive, réagisse et se déplace intelligemment.*

Focus :
- Entrée (capteurs) → Sortie (mouvement)
- Interaction avec le monde réel
- Boucles de rétroaction

Les élèves construisent des systèmes où le robot :
- Évite les obstacles
- Réagit à son environnement
- Interprète les entrées visuelles

> **Objectif du stage :**
> Comprendre comment le logiciel interagit avec le monde physique.

**Bases d'électronique**
- Tension, courant, résistance — juste assez pour ne rien casser
- Lire une fiche technique : ce que les chiffres importants signifient vraiment
- Broches GPIO : sortie numérique (on/off), entrée numérique, pull-up/pull-down
- Prototypage sur breadboard : prototyper des circuits en sécurité avant de souder
- Composants courants : LEDs, boutons, résistances, condensateurs
- Sécurité électrique : pourquoi ne jamais connecter du 5 V là où du 3,3 V est attendu

**Contrôle moteur**
- Comment fonctionnent les moteurs DC et les pilotes de moteur
- PWM : la modulation de largeur d'impulsion expliquée
- Entraînement différentiel : comment les robots à chenilles tournent
- Régulation PID : garder le mouvement fluide et précis
- Écrire une boucle de conduite simple en Python

**Capteurs**
- Types de capteurs : distance (ultrasons, IR, lidar), IMU, encodeurs
- Lire les données des capteurs via I2C, UART, SPI
- Filtrer les données de capteurs bruitées (moyenne glissante, bases de Kalman)
- Réagir à l'environnement : évitement d'obstacles

**Vision par ordinateur**
- Comment fonctionnent les caméras : résolution, fréquence d'images, exposition
- Les bases d'OpenCV : capturer des images, espaces de couleur, seuillage
- Détection de contours, recherche de contours
- Détection d'objets : ce que fait YOLO et comment ça fonctionne
- Perception de la profondeur : caméras stéréo et cartes de disparité
- Exécuter la vision sur le robot : accélération CUDA sur Jetson

**Intégration système**
- Les bases de ROS2 : nœuds, topics, messages
- Écrire une boucle complète percevoir-planifier-agir
- Journalisation, débogage et tests sur le matériel

> **Mission du stage 3 :** Construisez un robot qui évite les obstacles tout en diffusant
> son flux caméra vers un tableau de bord web que vous avez écrit au Stage 1. Observez le robot
> naviguer dans son environnement en temps réel.

---

## Stage 4 — IA & Systèmes d'apprentissage

*Apprenez au robot à penser et à s'adapter.*

Le robot évolue de :
- Réactif → Prédictif → Autonome

Les élèves construisent des systèmes où le robot :
- Prend des décisions
- Comprend le langage
- Apprend à partir de données

> **Objectif du stage :**
> Comprendre comment les machines peuvent apprendre et prendre des décisions.

**Fondements du machine learning**
- Qu'est-ce que le machine learning ? Supervisé vs non supervisé vs par renforcement
- Régression linéaire et régression logistique à partir de zéro
- Splits train/validation/test, surapprentissage, régularisation
- Descente de gradient : le moteur de l'apprentissage
- Introduction à PyTorch

**Deep learning**
- Réseaux de neurones : couches, activations, rétropropagation
- Entraîner un réseau sur les propres données de capteurs du robot
- Réseaux de neurones convolutifs (CNNs)
- Comment le détecteur YOLO du robot a été entraîné
- Transfer learning : affiner un modèle pré-entraîné

**Transformers et modèles de langage**
- Mécanisme d'attention : intuition et mathématiques
- L'architecture transformer
- Large Language Models : GPT, Llama, Gemma, Mistral
- Exécuter des LLMs localement avec Ollama sur Jetson Orin Nano
- Prompt engineering et prompts système

**IA agentique**
- Qu'est-ce qu'un agent IA ? Objectifs, outils et boucles de rétroaction
- Function calling et utilisation d'outils : donner à un modèle la capacité d'agir
- Construire un agent robot simple : le LLM décide quelle commande moteur envoyer
- Mémoire et contexte : comment les agents se souviennent entre les tours
- Raisonnement en plusieurs étapes : chaîne de pensée et patterns ReAct
- Sécurité et garde-fous : quand ne pas laisser le modèle décider

**Modèles multimodaux**
- Vision-Language Models (VLMs) : voir et comprendre en même temps
- Vision-Language-Action models (VLAs) : de la perception à l'action robot
- Speech-to-Text (STT) : Whisper, Voxtral
- Text-to-Speech (TTS) : Piper, Kokoro, voix expressives
- Pipelines d'interaction vocale de bout en bout

**Comment fonctionne le coach IA** *(méta-leçon)*
- Fine-tuning vs RAG : comment le coach a été construit sur ce curriculum
- Le pipeline vocal : STT → LLM → TTS de bout en bout
- Prompts système et persona : pourquoi le coach répond comme il le fait
- Évaluer un modèle : comment savoir s'il enseigne bien ?
- Comment contribuer une nouvelle leçon pour que le coach l'apprenne aussi

> **Mission du stage 4 :** Affinez un petit modèle local sur un sujet de votre
> choix et ajoutez-le à la base de connaissances du coach. Apprenez quelque chose de nouveau au robot.

---

## Stage 5 — Robot Autonome

*Rassemblez tout en un système complet.*

Les élèves conçoivent des comportements complets :
- Navigation
- Interaction
- Exécution de tâches

Le robot devient :
- Indépendant
- Réactif
- Orienté vers un objectif

> **Objectif du stage :**
> Construire un système entièrement autonome qui opère dans le monde réel.

**Architecture système complète**
- Comment toutes les couches se connectent : capteurs → perception → raisonnement → action
- Latence et contraintes temps réel
- Tout exécuter hors ligne : pas de cloud, pas d'abonnement
- Profilage et optimisation : trouver les goulots d'étranglement sur le matériel Jetson

**Comportements autonomes**
- Détection de personnes et salutation
- Q&R activé par la voix avec le LLM embarqué
- Naviguer un parcours d'obstacles avec la vision et le contrôle moteur
- Suivre une personne avec le flux optique
- Répondre aux commandes vocales par des actions physiques

**Construire vos propres projets**
- Concevoir un comportement robot à partir de zéro
- Déboguer des systèmes matériel + logiciel ensemble
- Documenter et partager votre projet
- Écrire des tests pour le code robot : pourquoi les tests hardware-in-the-loop comptent

**Contribuer en retour**
- Ajouter une nouvelle leçon au curriculum
- Entraîner un modèle personnalisé sur vos propres données
- Soumettre un projet à la communauté LearningMachina
- Ouvrir une pull request : comment fonctionne la collaboration open-source

> **Mission de fin d'études du stage 5 :** Concevez, construisez et documentez un
> comportement autonome original. Présentez-le au robot — il vous demandera d'expliquer
> chaque couche de la pile technologique.

---

## Des missions au lieu d'exercices

Les élèves ne font pas d'exercices abstraits.

Ils accomplissent des **missions** :
- Naviguer dans un espace
- Suivre une personne
- Répondre à des commandes vocales
- Réagir aux changements de l'environnement

Chaque mission combine :
- Programmation
- Robotique
- IA

---

## Déboguer par le comportement

Les erreurs ne se cachent pas dans les logs — elles sont visibles :

- Mauvaise logique → mauvais mouvement
- Erreur de capteur → réaction incorrecte
- État → visible par les lumières, le son, le mouvement

Cela rend le débogage :
- Intuitif
- Immédiat
- Physique

---

## Résultat final

À la fin du curriculum, l'élève peut :

- Programmer dans plusieurs langages
- Contrôler du vrai matériel
- Construire des systèmes intelligents
- Concevoir des comportements autonomes

Le plus important :

> Il comprend comment transformer du code en action dans le monde réel.

---

## Résumé de la philosophie

LearningMachina n'enseigne pas la programmation comme une compétence abstraite.

Il enseigne :

> **Comment donner un comportement à une machine.**
