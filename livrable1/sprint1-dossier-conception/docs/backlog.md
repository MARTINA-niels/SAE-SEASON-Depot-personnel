# Backlog des sprints 2 à 8 (user stories)

Estimations en points (1 = très simple, 2 = simple, 3 = moyen, 5 = complexe). Priorité : **M** = indispensable, **S** = souhaitable. « En tant que » : D = développeur de l'équipe, U = utilisateur (client / enseignant).

## Sprint 2 — Modèle en code et moteur qui démarre (livraison 16 oct.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-201 | En tant que D, je code les énumérations `Energie`, `Saison`, `Rarete` et le cours des énergies | M | 2 |
| US-202 | En tant que D, je code `CompteurEnergies`, `ReserveEnergie` (capacité 7), `StockEnergie` | M | 3 |
| US-203 | En tant que D, je code `RoueDesSaisons`, `EchelleDesAnnees`, `ResultatAvancee` | M | 3 |
| US-204 | En tant que D, je code `De`, `FaceDe`, `ActionDe` et le chargement de `des.json` | M | 3 |
| US-205 | En tant que D, je code `Joueur`, `PlateauIndividuel`, `JaugeInvocation`, `PisteDesBonus` | M | 3 |
| US-206 | En tant que D, je code `Plateau` et `PisteDesCristaux` | M | 2 |
| US-207 | En tant que D, je code `MiseEnPlace` pour 2 à 4 joueurs (N+1 dés par couleur) | M | 3 |
| US-208 | En tant que D, je code `Partie` et `Manche` avec la boucle de manches jusqu'à la fin de l'année 3 | M | 5 |
| US-209 | En tant que D, je code l'interface `Robot` et `RobotPasseur` | M | 2 |
| US-210 | En tant que U, je lance `mvn exec:java@partie` et je vois l'avancement (année, saison, manche) | M | 2 |
| US-211 | En tant que D, je mets en place les tests T01 à T04 et le workflow `mvn clean verify` | M | 2 |

## Sprint 3 — Manches complètes avec dés, sans cartes (livraison 23 oct.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-301 | En tant que D, je code le choix des dés dans l'ordre du tour (`choisirDe`) | M | 3 |
| US-302 | En tant que D, je résous les actions d'un dé : énergies, cristaux, jauge | M | 3 |
| US-303 | En tant que D, je gère le dépassement de la réserve (`garderEnReserve`) | M | 2 |
| US-304 | En tant que D, je code la cristallisation (`ActionCristalliser`, `Regles.valeurCristallisation`) | M | 3 |
| US-305 | En tant que D, je code le dé restant et l'avancée de la saison, les changements de saison et d'année | M | 3 |
| US-306 | En tant que D, je code la rotation du premier joueur et la fin de partie | M | 2 |
| US-307 | En tant que D, je code `Decompte` (score = cristaux) | M | 2 |
| US-308 | En tant que D, je code `RobotAleatoire` v1 | M | 3 |
| US-309 | En tant que U, je vois un affichage de fin de partie (scores, cristaux, énergies) | M | 3 |
| US-310 | En tant que D, j'écris les tests T05 à T12 et I01, I02 | M | 3 |

## Sprint 4 — Cartes, invocation, effets d'arrivée en jeu (livraison 30 oct.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-401 | En tant que D, je code `CartePouvoir`, `CarteEnJeu`, `Cout`, `CoutInvocation`, `Effet` et ses 4 interfaces | M | 3 |
| US-402 | En tant que D, je remplis `cartes.json` avec les 30 cartes (coûts, catégories, prestige) | M | 3 |
| US-403 | En tant que D, je code `CatalogueCartes` (60 cartes) et `RegistreEffets` | M | 3 |
| US-404 | En tant que D, je code `Pioche`, `Defausse` et le remélange | M | 2 |
| US-405 | En tant que D, je code `DistributionPreconstruite`, la répartition en 3 paquets et les jetons bibliothèque | M | 3 |
| US-406 | En tant que D, je code l'invocation (`ActionInvoquer`, `Regles.peutInvoquer`) | M | 5 |
| US-407 | En tant que D, je code les effets d'arrivée simples : cartes 1, 2, 3, 4, 22, 28, 29 | M | 3 |
| US-408 | En tant que D, je code `ContexteEffet` et son implémentation dans `Partie` | M | 3 |
| US-409 | En tant que D, je code le décompte complet : prestige des cartes et −5 par carte en main | M | 2 |
| US-410 | En tant que D, je passe `RobotAleatoire` en v2 (répartition, invocation, pioche) | M | 3 |
| US-411 | En tant que U, je vois les cartes en main et en jeu en fin de partie | M | 2 |
| US-412 | En tant que D, j'écris les tests T20 à T26 et I03 | M | 3 |

## Sprint 5 — Effets, bonus, simulation, rapport (livraison 6 nov.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-501 | En tant que D, je code `BusEvenements` et `EvenementJeu` | M | 3 |
| US-502 | En tant que D, je code les effets permanents : cartes 6, 8, 13, 14, 20, 30 | M | 5 |
| US-503 | En tant que D, je code l'activation (inclinaison, redressement) et les cartes 5, 23, 25, 26 | M | 5 |
| US-504 | En tant que D, je code les bonus du plateau individuel (4 types, malus) | M | 3 |
| US-505 | En tant que D, je code le départage des égalités | M | 1 |
| US-506 | En tant que D, je code `RobotGlouton` | M | 5 |
| US-507 | En tant que D, je code `Simulateur`, `StatistiquesRobot`, `Classement`, `AfficheurResume` | M | 5 |
| US-508 | En tant que U, je lance `mvn exec:java@simulation` et je lis uniquement le résumé de 500 parties | M | 2 |
| US-509 | En tant que D, j'écris les tests T27 à T35 et I04 | M | 3 |
| US-510 | En tant que D, je rédige le rapport de la semaine 45 (architecture, diagrammes, résultats, risques) | M | 5 |
| US-511 | En tant que D, je mets à jour le planning (v1.1) et le backlog | M | 1 |

## Sprint 6 — Inflexion du client (livraison 13 nov.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-601 | En tant que D, j'analyse l'inflexion du client et je mets à jour backlog, UML et planning (v1.2) | M | 3 |
| US-602 | En tant que D, j'implémente l'inflexion du client (estimation à faire après annonce) | M | ? |
| US-603 | En tant que D, je code les cartes avec choix : 10 (Syllas), 12 (Naria), 16 (Kairn), 17 (Amsug), 21 (Lewis) via `Decideur` | M | 5 |
| US-604 | En tant que D, je code les effets liés aux saisons : 7 (Bottes), 11 (Figrim), 27 (Sablier) | M | 5 |
| US-605 | En tant que D, je code les mises en jeu gratuites : 9 (Calice), 24 (Potion de rêves) | M | 3 |
| US-606 | En tant que D, je code 15 (Dé de la malice), 18 (Grimoire), 19 (Heaume) | M | 5 |
| US-607 | En tant que D, je code `RobotStrategique` v1 | M | 5 |
| US-608 | En tant que D, j'écris les tests T40 à T50 | M | 3 |

## Sprint 7 — 30 cartes complètes, robots avancés, visualisation (livraison 20 nov.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-701 | En tant que D, je vérifie les 30 cartes avec les cas du lexique (tests T51 à T54) | M | 5 |
| US-702 | En tant que D, je code `RobotCombo` | M | 5 |
| US-703 | En tant que U, je vois le déroulé complet de la partie (événements : dés, actions, invocations, changements de saison) | M | 5 |
| US-704 | En tant que U, je lance des simulations à 2, 3 et 4 joueurs avec tous les robots et je lis le classement | M | 3 |
| US-705 | En tant que D, je règle les paramètres des robots d'après les statistiques | S | 3 |
| US-706 | En tant que D, j'ajoute la vérification des invariants et le test I05 | M | 3 |
| US-707 | En tant que D, j'absorbe les retards ou conséquences de l'inflexion (tampon) | M | 5 |

## Sprint 8 — Stabilisation et release (livraison 27 nov.)

| ID | User story | Prio | Pts |
|---|---|---|---|
| US-801 | En tant que D, je corrige les anomalies restantes | M | 5 |
| US-802 | En tant que D, je relis le code et je mets à jour tous les diagrammes | M | 3 |
| US-803 | En tant que U, je reçois les statistiques finales validées (victoires, moyenne des points, autres indicateurs) | M | 2 |
| US-804 | En tant que D, je rédige `README.md` et le guide d'exécution Maven | M | 2 |
| US-805 | En tant que D, je pose le tag `v1.0` sur GitHub | M | 1 |
| US-806 | En tant que D, je prépare la soutenance (supports, démonstration, répétition) | M | 5 |
