# Projet Seasons (Java) — Planning initial des sprints

**Version :** v1.0 — rendu du vendredi 2 octobre 2026 (soir)
**Équipe :** 5 personnes maximum
**Outils :** Java, Maven, Git/GitHub, diagrammes UML (classes, séquence…), documentation en Markdown

> Ce planning est une première version. Il évoluera avec l'avancement réel. Chaque révision sera versionnée (`PLANNING_v1.1.md`, etc.) pour garder la trace des évolutions et des inflexions du client.

## 0. Périmètre du projet : version débutante

Le projet se limite à la **version débutante** du jeu (niveau « Apprenti magicien » des règles) :

- **30 cartes pouvoir de base**, numérotées de 1 à 30 (chacune en 2 exemplaires, soit 60 cartes). Les cartes 31 à 50 sont **hors périmètre**.
- **Jeux pré-construits** : au lieu du draft, chaque joueur reçoit l'un des 4 jeux de 9 cartes prédéfinis (jeu n°1 : 1, 2, 7, 17, 18, 20, 26, 29, 30 ; jeu n°2 : 3, 5, 9, 14, 15, 21, 23, 25, 28 ; jeu n°3 : 4, 6, 7, 9, 12, 16, 22, 24, 30 ; jeu n°4 : 1, 2, 3, 11, 13, 15, 18, 25, 27). Le joueur répartit ensuite ses 9 cartes en 3 paquets de 3 (main, année 2, année 3).
- Le reste des cartes (1 à 30) constitue la pioche.
- Les niveaux Magicien et Archimage, et le draft de 9 cartes, ne sont pas développés (ils pourraient faire l'objet d'une inflexion du client).

## 1. Vue d'ensemble

| Sprint | Période | Livraison | Thème principal |
|---|---|---|---|
| 0 | jusqu'au dim. 4 oct. | **Ven. 2 oct.** (planning) | Cadrage, outillage, planning |
| 1 | 5 → 9 oct. | **Ven. 9 oct.** | **Sprint théorique : conception UML complète** |
| 2 | 12 → 16 oct. | **Ven. 16 oct.** | Modèle en code + moteur qui démarre |
| 3 | 19 → 23 oct. | **Ven. 23 oct.** | Manches complètes avec dés, sans cartes |
| 4 | 26 → 30 oct. | **Ven. 30 oct.** | Cartes (jeux pré-construits), invocation, effets d'arrivée en jeu |
| 5 | 2 → 6 nov. (sem. 45) | **Ven. 6 nov.** | Effets permanents et d'activation, bonus, simulation 500 parties, **rapport** |
| 6 | 9 → 13 nov. (sem. 46) | **Ven. 13 nov.** | **Inflexion client**, cartes à effets complexes |
| 7 | 16 → 20 nov. | **Ven. 20 nov.** | 30 cartes complètes, robots avancés, visualisation |
| 8 | 23 → 27 nov. | **Ven. 27 nov.** | Stabilisation, doc finale, release |
| — | 30 nov. → 4 déc. (sem. 49) | — | Marge + préparation de la soutenance |
| — | 7 → 18 déc. (sem. 50 ou 51) | — | **Soutenance** (semaine à confirmer) |

## 2. Jalons imposés par le client

| Jalon | Date / semaine | Où il est placé |
|---|---|---|
| Équipes de 5 max | début de projet | Sprint 0 |
| Découpage en 8 sprints + livraisons prévues | ven. 2 oct. soir | Sprint 0 (ce document) |
| Fin du sprint zéro | dim. 4 oct. | Sprint 0 |
| Livraison chaque vendredi | à partir du 9 oct. | Sprints 1 à 8 |
| Rapport | semaine 45 (2 → 8 nov.) | Sprint 5 |
| Évolution des exigences du client | semaine 46 (9 → 15 nov.) | Sprint 6 |
| Soutenance | semaine 50 ou 51 | Après le sprint 8 |

## 3. Règles transverses

- **Chaque nouvel élément du moteur et du modèle doit être utilisé par au moins un robot.**
- À partir du sprint 2, chaque livraison compile, passe `mvn clean verify` et peut être lancée via Maven.
- Deux exécutions Maven à configurer dans le `pom.xml` : `partie` (une partie détaillée et lisible) et `simulation` (500 parties, uniquement le résumé global).
- Les diagrammes sont versionnés dans `docs/` et mis à jour dans le sprint où le code évolue (le sprint 1 pose la référence).
- Fin de chaque sprint : revue, rétrospective courte, mise à jour du backlog et du planning.

## 4. Détail des sprints

### Sprint 0 — Cadrage (jusqu'au dimanche 4 octobre)
**Livraison : vendredi 2 octobre au soir — le planning de 8 sprints (ce document).**

- Constitution de l'équipe (5 max) et répartition des rôles.
- Lecture et analyse des règles ; liste des éléments du jeu (plateau, dés, cartes, pions, énergies, cristaux, bonus).
- Délimitation du périmètre : version débutante, 30 cartes.
- Dépôt GitHub, branches, conventions, `README.md`, projet Maven vide qui compile.
- Premier backlog.
- **Fin du sprint zéro : dimanche 4 octobre.**

### Sprint 1 — Sprint théorique : conception UML (5 → 9 oct.)
**Livraison : vendredi 9 octobre — dossier de conception (aucun code métier attendu).**

Objectif : figer une architecture claire avant de coder, et prévoir dès maintenant les évolutions possibles (inflexion du client, nouveaux robots, éventuelle extension à d'autres cartes).

Diagrammes à produire (Mermaid ou PlantUML, intégrés dans les fichiers Markdown de `docs/`) :

| Diagramme | Contenu |
|---|---|
| Cas d'utilisation | Lancer une partie, lancer une simulation de 500 parties, consulter l'état du jeu, rôle des robots |
| Packages / architecture | `modele`, `moteur`, `robots`, `affichage`, `simulation`, `app` et leurs dépendances |
| Classes — modèle | `Plateau`, `RoueDesSaisons`, `EchelleDesAnnees`, `PisteDesCristaux`, `StockEnergie`, `De` / `FaceDe`, `Joueur`, `PlateauIndividuel` (réserve, jauge, bonus), `Energie`, `Saison`, `Bonus` |
| Classes — cartes | `CartePouvoir` (objet magique / familier), coût d'invocation, type d'effet (arrivée en jeu, permanent, activation), `Effet` extensible, `JeuPreconstruit` |
| Classes — moteur | `Partie`, `Manche`, `Tour`, `Action` (dé, invocation, activation, bonus), `Regles`, `Score` |
| Classes — robots | interface `Robot` / `Strategie`, `RobotPasseur`, `RobotAleatoire`, `RobotGlouton`, `RobotPrestige`, `RobotCombo` |
| Séquence | Mise en place (jeux pré-construits, construction des 3 paquets) ; une manche complète ; le tour d'un joueur ; invocation d'une carte ; activation d'une carte ; cristallisation ; fin de manche et changement de saison / d'année ; fin de partie et décompte |
| États | Cycle de vie d'une partie (mise en place, tournoi, fin) ; états d'une carte (en main, sous jeton bibliothèque, en jeu, inclinée, défaussée) |
| Activité | Déroulé d'une manche, du lancer des dés à l'avancement de la saison |

Autres livrables :

- Dossier `docs/conception.md` qui présente les choix (patterns envisagés : Strategy pour les robots, effets de cartes en objets, Observer pour l'affichage), les règles à modéliser et les points d'attention (limite de 7 énergies, jauge d'invocation, effets qui priment sur les règles).
- Catalogue des 30 cartes classées par type d'effet (arrivée en jeu, permanent, activation, fin de partie).
- Matrice « élément du jeu → classe → robot qui l'utilise », pour respecter la règle transverse.
- Plan de tests (règles critiques) et schéma de configuration Maven (exécutions `partie` et `simulation`).
- Backlog des sprints 2 à 8 détaillé en user stories à partir des diagrammes.

### Sprint 2 — Modèle en code et moteur qui démarre (12 → 16 oct.)
**Livraison : vendredi 16 octobre**

- Implémentation du modèle conçu au sprint 1 : énergies, saisons, dés, plateau, joueurs, pions.
- Moteur minimal : mise en place pour 2 à 4 joueurs (nombre de dés = joueurs + 1 par couleur), boucle de manches, avancement de la saison et de l'année.
- **Livrable :** un moteur qui démarre et affiche seulement l'avancement de la partie (année, saison, manche).
- Robot : `RobotPasseur` (ne fait rien) pour valider la boucle de jeu.
- Maven : exécution `partie` configurée.
- Écarts éventuels avec la conception UML consignés et diagrammes corrigés.

### Sprint 3 — Manches complètes avec dés, sans cartes (19 → 23 oct.)
**Livraison : vendredi 23 octobre**

- Lancer des dés, choix des dés dans l'ordre du tour, dé restant qui fait avancer la saison.
- Actions des dés : énergies (limite de 7), cristaux, jauge d'invocation, cristallisation (cours des énergies selon la saison). La pioche est préparée mais sans cartes.
- Changement de saison et d'année, nouveau premier joueur, fin de partie après la 3e année.
- Score = cristaux.
- Robot : `RobotAleatoire` v1 (choix de dé et de cristallisation au hasard).
- Visualisation textuelle de fin de partie : scores, cristaux, énergies.

### Sprint 4 — Cartes, invocation, effets d'arrivée en jeu (26 → 30 oct.)
**Livraison : vendredi 30 octobre**

- Implémentation de `CartePouvoir` et chargement des 30 cartes de base (2 exemplaires chacune) depuis un fichier de données.
- Pioche, défausse, remélange.
- Mise en place des cartes : attribution des 4 jeux pré-construits, construction des 3 paquets, jetons bibliothèque II et III, ajout des cartes en début d'année 2 et 3.
- Invocation : paiement du coût, jauge suffisante, cartes en jeu.
- Effets d'arrivée en jeu simples : cartes 1, 2, 3, 4, 22, 28, 29.
- Fin de partie : points de prestige des cartes, malus de 5 points par carte restée en main.
- Robot `RobotAleatoire` v2 : répartition aléatoire des 9 cartes en 3 paquets, invocation aléatoire.
- Visualisation : cartes en main et en jeu en fin de partie.

### Sprint 5 — Effets permanents et d'activation, bonus, simulation, rapport (2 → 6 nov., semaine 45)
**Livraison : vendredi 6 novembre — avec le rapport de la semaine 45**

- Effets permanents : cartes 6, 8, 13, 14, 20, 30.
- Effets d'activation (inclinaison, redressement en début de manche, coûts, sacrifice) : cartes 5, 23, 25, 26.
- Bonus du plateau individuel (4 types, limite de 3, malus de 5 / 12 / 20).
- Départage des égalités (plus grand nombre de cartes invoquées).
- Simulation de 500 parties entre 2 robots : victoires et moyenne des points. Exécution Maven `simulation` configurée.
- Robot `RobotGlouton` : heuristique « cristallisation » (dé le plus rentable, cristallisation selon la saison, invocation rentable, usage des bonus).
- **Rapport (semaine 45)** : architecture, diagrammes mis à jour, choix de conception, premiers résultats de simulation, risques pour l'évolution du client.
- Mise à jour du planning (v1.1) selon l'avancement réel.

### Sprint 6 — Inflexion du client (9 → 13 nov., semaine 46)
**Livraison : vendredi 13 novembre**

- **En semaine 46, le client peut modifier ses exigences : la conception et le planning doivent pouvoir évoluer.** Environ 40 % de la capacité du sprint est réservée à l'inflexion.
- Analyse d'impact immédiate : backlog, diagrammes UML et planning (v1.2).
- Cartes à effets complexes : 7 (Bottes temporelles), 9 (Calice divin), 10 (Syllas), 11 (Figrim), 12 (Naria), 15 (Dé de la malice), 16 (Kairn), 17 (Amsug), 18 (Grimoire ensorcelé), 21 (Lewis Grisemine), 24 (Potion de rêves), 27 (Sablier du temps), 19 (Heaume de Ragfield, effet de fin de partie).
- Simulation de 500 parties à 2 et 3 robots avec classement.
- Robot `RobotPrestige` v1 : maximise les points de prestige (répartition des 9 cartes sur les 3 années, invocation des cartes à fort prestige, pas de cartes oubliées en main).

### Sprint 7 — 30 cartes complètes, robots avancés, visualisation (16 → 20 nov.)
**Livraison : vendredi 20 novembre**

- Finalisation et vérification des 30 cartes (cas limites du lexique : Grimoire ensorcelé, Amulette d'eau, cartes mises en jeu gratuitement, changements de saison).
- `RobotCombo` : joue ensemble les cartes à combinaisons la même année, garde les effets de fin de partie pour l'année 3.
- Simulation de 500 parties à 2, 3 et 4 joueurs avec tous les robots, classement global.
- Visualisation textuelle tout au long de la partie (état du plateau, actions de chaque joueur, effets déclenchés).
- Couverture de tests sur les règles critiques.
- Ce sprint sert aussi de tampon pour absorber les conséquences de l'inflexion.

### Sprint 8 — Stabilisation et release (23 → 27 nov.)
**Livraison : vendredi 27 novembre**

- Correction des anomalies, tests complets, relecture du code.
- Statistiques finales à valider avec le client : victoires, moyenne des points, autres indicateurs.
- Documentation finale : `README.md`, guide d'exécution Maven (`partie` / `simulation`), diagrammes à jour, bilan des sprints.
- Tag de release `v1.0` sur GitHub.
- Semaine 49 : marge de sécurité et préparation de la soutenance.

## 5. Exécutions Maven attendues

| Exécution | Commande (exemple) | Sortie |
|---|---|---|
| Une partie | `mvn exec:java@partie` | Déroulé visible et compréhensible de la partie |
| 500 parties | `mvn exec:java@simulation` | Uniquement le résumé : victoires, moyenne des points, classement |

## 6. Hypothèses et risques

**Hypothèses**
- Le sprint 0 se termine le dimanche 4 octobre ; les sprints 1 à 8 vont du lundi au vendredi, avec livraison chaque vendredi.
- Le sprint 1 est purement théorique : la livraison est un dossier de conception, sans code métier.
- Le périmètre est la version débutante : 30 cartes, jeux pré-construits, sans draft.
- Le rapport est rendu en semaine 45 (livraison du vendredi 6 novembre).
- La soutenance a lieu en semaine 50 ou 51, la date exacte n'étant pas encore fixée.

**Risques**
- Le code démarre au sprint 2 : le sprint 5 (effets, bonus, simulation, rapport) reste chargé.
- L'inflexion de la semaine 46 peut toucher le modèle ou les règles : la conception du sprint 1 prévoit des effets de cartes extensibles.
- Plusieurs cartes ont des interactions délicates (Grimoire ensorcelé, Calice divin, Potion de rêves) : les tester dès leur implémentation.
- Si le planning glisse, le sprint 7 sert de tampon et la visualisation en cours de partie peut être allégée, après accord du client.
