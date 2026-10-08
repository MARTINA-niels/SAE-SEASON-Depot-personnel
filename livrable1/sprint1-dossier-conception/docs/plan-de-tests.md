# Plan de tests (règles critiques)

Tests unitaires avec **JUnit 5** (`mvn test`). Chaque test est tiré d'une règle ou d'un exemple du livret ; ils sont écrits au sprint indiqué.

## 1. Règles de base

| ID | Sprint | Test | Résultat attendu |
|---|---|---|---|
| T01 | 2 | Nombre de dés pour 2, 3, 4 joueurs | 3, 4, 5 dés par couleur |
| T02 | 2 | Marqueurs de départ | Année 1, case 1 de la roue |
| T03 | 2 | Avancer la roue de 3 cases depuis la case 2 | Case 5, changement de saison (hiver → printemps) |
| T04 | 2 | Passer de la case 12 à la case 1 | Changement d'année |
| T05 | 3 | Réserve avec 7 énergies, ajout d'une énergie | `depassement()` = 1 ; le joueur doit en défausser une |
| T06 | 3 | Jauge d'invocation à 15 | Impossible d'augmenter |
| T07 | 3 | Cristallisation en hiver : 1 feu + 2 terres | 2 + 3 + 3 = **8 cristaux** (exemple du livret p. 7) |
| T08 | 3 | Cours des énergies pour chaque saison | Conforme au tableau p. 16 |
| T09 | 3 | Dé restant avec 1, 2 ou 3 points | La roue avance de 1, 2 ou 3 cases |
| T10 | 3 | Piste des cristaux : retrait au-delà du solde | Le solde tombe à 0, jamais négatif |
| T11 | 3 | Nouveau premier joueur après une manche | Voisin de gauche |
| T12 | 3 | Résolution d'un dé « 2 énergies de terre + cristaux » | Énergies en réserve, cristaux avancés |

## 2. Cartes, jauge et bonus

| ID | Sprint | Test | Résultat attendu |
|---|---|---|---|
| T20 | 4 | Invocation avec jauge à 0 (aucune carte possible) | Refusée ; avec jauge 1 : acceptée, une seule carte |
| T21 | 4 | Invocation sans payer le coût | Refusée (action non légale) |
| T22 | 4 | Jeux pré-construits distribués à 4 joueurs | Chaque jeu = 9 cartes ; jamais plus de 2 exemplaires d'une carte |
| T23 | 4 | Début d'année 2 puis 3 | Les cartes du jeton II puis III rejoignent la main ; les cartes déjà en main sont conservées |
| T24 | 4 | Carte 3 (Amulette de terre) | +9 cristaux à l'arrivée |
| T25 | 4 | Carte 1 (Amulette d'air) | Jauge +2 |
| T26 | 4 | Carte 2 (Amulette de feu) | 4 cartes piochées, 1 gardée, 3 défaussées |
| T27 | 5 | Bonus : 1, 2, 3 utilisés | Malus 5, 12, 20 ; un 4e bonus est refusé |
| T28 | 5 | Carte inclinée : seconde activation dans le même tour | Refusée ; redressée en début de manche suivante |
| T29 | 5 | Bâton du printemps (6) | +3 cristaux par invocation depuis la main, 0 pour une mise en jeu gratuite |
| T30 | 5 | Bourse d'Io (8) + Potion de vie (26) | +1 cristal par énergie cristallisée en plus des 4 par énergie |
| T31 | 5 | Coffret merveilleux (13) avec 4 puis 3 énergies | +3 cristaux seulement avec 4 énergies ou plus |
| T32 | 5 | Corne du mendiant (14) avec 1 puis 2 énergies | +1 énergie seulement avec 1 énergie ou moins |
| T33 | 5 | Main de la fortune (20) | Coût réduit de 1 énergie ; coût jamais inférieur à 1 énergie ; coût d'activation inchangé |
| T34 | 5 | **Score de l'exemple du livret (p. 9)** : 72 cristaux, 68 de prestige, 2 bonus, 1 carte en main | 72 + 68 − 12 − 5 = **123** |
| T35 | 5 | Égalité de points | Vainqueur : le joueur ayant invoqué le plus de cartes |

## 3. Effets complexes

| ID | Sprint | Test | Résultat attendu |
|---|---|---|---|
| T40 | 6 | Bottes temporelles (7) recule de l'hiver à l'automne | Année −1, cartes en main conservées |
| T41 | 6 | Bottes temporelles : changement de saison | Figrim (11) et Sablier (27) font effet immédiatement |
| T42 | 6 | Calice divin (9) avec jauge insuffisante | La carte choisie est défaussée |
| T43 | 6 | Syllas (10) contre un adversaire sans carte en jeu | Aucun effet |
| T44 | 6 | Figrim (11) contre un adversaire sans cristal | Non concerné |
| T45 | 6 | Naria (12) à 3 joueurs | 3 cartes piochées, 1 gardée, 1 donnée à chaque adversaire |
| T46 | 6 | Dé de la malice (15) | Le nouveau dé remplace l'ancien, +2 cristaux ; avec deux Dés, +4 |
| T47 | 6 | Kairn (16) | Chaque adversaire recule de 4 cristaux, jamais sous 0 |
| T48 | 6 | Amsug Longcoup (17) | Chaque joueur renvoie un objet magique ; sans objet magique, pas d'effet |
| T49 | 6 | Grimoire ensorcelé (18) | Capacité 10 ; deux Grimoires : toujours 10 |
| T50 | 6 | Potion de rêves (24) sans énergie | Utilisable ; la carte mise en jeu ne déclenche pas Bâton ni Vase |
| T51 | 7 | Heaume de Ragfield (19) à égalité de cartes en jeu | Aucun bonus |
| T52 | 7 | Amulette d'eau (4) | Énergies hors effets de la réserve (Coffret, Corne, Potion de vie) |
| T53 | 7 | Lewis Grisemine (21) | Copie la réserve de l'adversaire, pas l'Amulette d'eau |
| T54 | 7 | Balance d'Ishtar (5) | Conforme à la décision du client sur Q1 |

## 4. Tests d'intégration

| ID | Sprint | Test | Résultat attendu |
|---|---|---|---|
| I01 | 2 | Partie avec `RobotPasseur` x2 | Se termine, ne plante pas |
| I02 | 3 | Partie avec `RobotAleatoire` x2, graine fixe | Résultat identique à chaque exécution |
| I03 | 4 | 1 000 parties aléatoires | Aucune exception, aucun état incohérent (réserve ≤ capacité, jauge ≤ 15, cristaux ≥ 0) |
| I04 | 5 | Simulation de 500 parties | Somme des victoires + égalités = 500 ; sortie limitée au résumé |
| I05 | 7 | Parties à 2, 3 et 4 joueurs | Aucune erreur |

## 5. Invariants vérifiés après chaque action (mode test)

- réserve ≤ capacité (7 ou 10) ;
- jauge d'invocation entre 0 et 15 ;
- cartes en jeu ≤ jauge d'invocation ;
- cristaux ≥ 0 ;
- au plus 2 exemplaires d'une même carte dans la partie ;
- nombre total de cartes constant (pioche + défausse + mains + jetons + cartes en jeu = 60).
