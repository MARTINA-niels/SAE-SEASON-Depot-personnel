# Matrice « élément du jeu → classe → robot qui l'utilise »

Règle du projet : **chaque nouvel élément intégré au moteur et à la représentation doit être utilisé par au moins un robot.** Cette matrice sert de contrôle à chaque sprint : un élément livré sans robot qui l'utilise n'est pas terminé.

| Élément | Classe(s) | Sprint | Décision du robot | Robot |
|---|---|---|---|---|
| Boucle de manches, saisons, années | `Partie`, `Manche`, `RoueDesSaisons` | 2 | Terminer son tour | `RobotPasseur` |
| Dés des saisons | `De`, `FaceDe` | 3 | `choisirDe` | `RobotAleatoire` v1 ; `RobotGlouton` (dé le plus rentable) |
| Énergies reçues et réserve limitée | `ReserveEnergie`, `StockEnergie` | 3 | `garderEnReserve` | `RobotAleatoire` v1 ; `RobotStrategique` (garde les énergies utiles à la saison suivante) |
| Cristaux | `PisteDesCristaux` | 3 | (résultat des actions) | tous, via le score |
| Cristallisation et cours des énergies | `Saison`, `Rarete`, `ActionCristalliser` | 3 | Choisir quelles énergies cristalliser | `RobotAleatoire` v1 ; `RobotGlouton` (énergies rares de la saison) |
| Jauge d'invocation | `JaugeInvocation` | 3 | Contrainte pour l'invocation | `RobotAleatoire` v2 ; `RobotStrategique` |
| Pioche et défausse | `Pioche`, `Defausse` | 3 (structure), 4 | `garderCartePiochee` | `RobotAleatoire` v2 ; `RobotStrategique` (garde les cartes rentables) |
| Jeux pré-construits, 3 paquets, jetons bibliothèque | `DistributionPreconstruite`, `JetonBibliotheque`, `RepartitionCartes` | 4 | `repartirCartes` | `RobotAleatoire` v2 ; `RobotStrategique` (répartition par année) ; `RobotCombo` (regroupe les combinaisons) |
| Invocation, coût, points de prestige | `ActionInvoquer`, `CoutInvocation` | 4 | Choisir quelles cartes invoquer et dans quel ordre | `RobotAleatoire` v2 ; `RobotGlouton` ; `RobotStrategique` |
| Effets d'arrivée en jeu (1, 2, 3, 4, 22, 28, 29) | `EffetArrivee` | 4 | Invoquer les cartes à gain direct | `RobotGlouton` (3, 29) ; `RobotStrategique` |
| Pénalité de −5 par carte en main | `Decompte` | 4 | Éviter de garder des cartes en main | `RobotStrategique` |
| Effets permanents (6, 8, 13, 14, 20, 30) | `EffetPermanent`, `BusEvenements` | 5 | Invoquer le Bâton ou le Vase avant d'autres cartes, garder 4 énergies pour le Coffret | `RobotGlouton` (Bâton, Bourse) ; `RobotCombo` |
| Effets d'activation (5, 23, 25, 26) | `EffetActivation`, `ActionActiver`, `CarteEnJeu` | 5 | Activer au bon moment | `RobotGlouton` (Balance, Potion de vie) ; `RobotStrategique` |
| Bonus du plateau individuel | `PisteDesBonus`, `ActionBonus` | 5 | Utiliser un bonus si le gain dépasse le malus | `RobotGlouton` ; `RobotStrategique` |
| Départage des égalités | `Decompte` | 5 | (aucune) | statistiques de simulation |
| Simulation de 500 parties | `Simulateur`, `StatistiquesRobot` | 5 | (aucune) | `RobotAleatoire` contre `RobotGlouton` |
| Changements de saison et d'année (cartes 7, 11, 27) | `ResultatAvancee`, `EvenementJeu` | 6 | Jouer Figrim ou Sablier avant un changement de saison | `RobotStrategique` |
| Cartes avec choix d'adversaire, de carte, d'énergie (10, 12, 16, 17, 21) | `Decideur` | 6 | `choisirAdversaire`, `choisirCarte`, `choisirEnergie` | `RobotAleatoire` ; `RobotStrategique` (cible le meneur) |
| Mise en jeu gratuite (9, 24) | `ContexteEffet.mettreEnJeuGratuitement` | 6 | Choisir la carte à mettre en jeu | `RobotStrategique` |
| Relance de dé (15) | `ContexteEffet.relancerDe` | 6 | Décider de relancer | `RobotStrategique` |
| Grimoire ensorcelé (18) | `ReserveEnergie` (capacité 10) | 6 | Garder jusqu'à 10 énergies | `RobotStrategique` |
| Heaume de Ragfield (19), effets de fin de partie | `EffetFinDePartie` | 6-7 | Garder la carte pour l'année 3 ; viser le plus de cartes en jeu | `RobotCombo` |
| Combinaisons de cartes | `RepartitionCartes` | 7 | Jouer ensemble les cartes qui se renforcent | `RobotCombo` |
| 3 et 4 joueurs | `Partie`, `MiseEnPlace` | 7 | (aucune) | simulation avec tous les robots |
| Affichage en cours de partie | `AfficheurTexte`, `EvenementJeu` | 7 | (aucune) | exécution `partie` |
