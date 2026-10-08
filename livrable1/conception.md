# Note de conception — Seasons (version débutante)

**Sprint 1 — livraison du vendredi 9 octobre 2026**

## 1. Objectif et périmètre

Réaliser une version électronique du jeu **Seasons** en Java, avec un modèle du jeu, un moteur de règles, des robots de niveaux croissants, une simulation de 500 parties et un affichage textuel.

**Périmètre : version débutante** (niveau « Apprenti magicien »)

- 30 cartes pouvoir (n° 1 à 30), chacune en 2 exemplaires, soit 60 cartes ;
- 4 jeux pré-construits de 9 cartes à la place du draft ;
- 2 à 4 joueurs, 3 années de jeu ;
- hors périmètre : cartes 31 à 50, niveaux Magicien et Archimage, draft.

## 2. Vocabulaire

| Terme | Définition |
|---|---|
| Manche | Un tour de table complet, joué avec les dés d'une saison |
| Saison / année | 12 cases sur la roue : hiver 1-3, printemps 4-6, été 7-9, automne 10-12 ; 3 années par partie |
| Énergie | Air, eau, feu, terre ; stockées dans la réserve (7 max) |
| Cristal | Point de prestige ; obtenus par dés, cristallisation, effets |
| Jauge d'invocation | Nombre maximal de cartes en jeu (15 max) |
| Invoquer | Payer le coût d'une carte de la main et la mettre en jeu |
| Activer | Incliner une carte à effet d'activation et payer son coût d'activation |
| Sacrifier / défausser | Sacrifier : carte en jeu vers la défausse ; défausser : carte de la main (ou énergies vers le stock) |
| Bonus | Avantage utilisable 3 fois maximum, avec un malus de points en fin de partie |

## 3. Règles à modéliser et classe responsable

| Règle (source : règles FR) | Classe responsable |
|---|---|
| N+1 dés par couleur pour N joueurs (p. 4) | `MiseEnPlace`, `Plateau` |
| Le premier joueur lance les dés, chacun en choisit un dans l'ordre, un dé reste (p. 5-6) | `Manche` |
| Le dé restant fait avancer la roue de 1, 2 ou 3 cases (p. 8) | `Manche`, `RoueDesSaisons` |
| Réserve limitée à 7 énergies, à réduire avant toute autre action (p. 6) | `ReserveEnergie`, `Tour` |
| Jauge d'invocation limitée à 15 (p. 6) | `JaugeInvocation` |
| Résolution des actions du dé (pioche, énergies, cristaux, jauge) avant les autres actions (p. 8) | `Tour` |
| Cristallisation selon le cours des énergies de la saison, valable jusqu'à la fin du tour (p. 7, 16) | `Saison`, `Regles` |
| Invocation : payer le coût et avoir une jauge suffisante, plusieurs cartes par tour (p. 7) | `Regles`, `Tour` |
| Activation : incliner la carte, payer le coût, redressement en début de manche (p. 8) | `CarteEnJeu`, `Regles` |
| Bonus : 4 types, 3 maximum, malus 5 / 12 / 20 (p. 8) | `PisteDesBonus` |
| Changement de saison aux cases 3, 6, 9, 12 ; changement d'année de la case 12 à 1 (p. 9) | `RoueDesSaisons`, `Manche` |
| Cartes sous les jetons bibliothèque ajoutées à la main en début d'année 2 et 3 (p. 5, 9) | `Manche`, `JetonBibliotheque` |
| Nouveau premier joueur = voisin de gauche (p. 9) | `Manche` |
| Fin : année 3 et marqueur au-delà de la case 12 (p. 9) | `Manche`, `Partie` |
| Score : cristaux + prestige des cartes en jeu − 5 par carte en main − malus des bonus (p. 9) | `Decompte` |
| Égalité : le joueur qui a invoqué le plus de cartes gagne (p. 9) | `Decompte` |
| L'effet d'une carte l'emporte sur une règle qui s'y oppose (p. 7) | `Regles` consulte les `EffetPermanent` |
| Une carte mise en jeu gratuitement n'est pas « invoquée » (p. 7) | `ContexteEffet.mettreEnJeuGratuitement` |

## 4. Choix de conception

| Choix | Raison |
|---|---|
| **Strategy** : interface `Robot` | Ajouter un robot = écrire une classe, sans toucher au moteur |
| **Observer** : `BusEvenements` | L'affichage et les effets permanents réagissent aux événements ; l'affichage est remplaçable (texte, silencieux) |
| **Command** : `ActionJoueur` | Le moteur ne propose que des actions légales : les robots ne peuvent pas tricher |
| Données des cartes en JSON, effets en classes | Ajouter ou corriger une carte (ou répondre à une inflexion) reste local |
| `ContexteEffet` implémenté par le moteur | Les effets ne dépendent pas de `moteur` : pas de dépendance circulaire |
| Interface `Robot` définie dans `moteur` | Le moteur ne dépend pas des robots concrets (inversion de dépendance) |
| `VuePartie` en lecture seule | Un robot ne voit que l'information publique |
| `StrategieDistribution` | Distribution actuelle = jeux pré-construits ; un draft futur s'y branche |
| `Random` injecté avec une graine | Parties reproductibles, simulations vérifiables |
| Enums `Saison` et `Rarete` | Le cours des énergies est une table, pas du code conditionnel |

## 5. Déroulé d'une partie (résumé)

1. **Mise en place** : plateau, N+1 dés par couleur, 60 cartes créées, un jeu pré-construit par joueur, répartition des 9 cartes en 3 paquets (main, année 2, année 3), premier joueur désigné.
2. **Tournoi** : pour chaque manche, lancer des dés, choix des dés, tour de chaque joueur, fin de manche, avancée de la roue, éventuels changements de saison et d'année.
3. **Fin** : après la 3e année, décompte et désignation du vainqueur.

Voir les diagrammes de séquence (`07-sequences.md`) et d'activité (`08-etats-et-activite.md`).

## 6. Extensibilité (face à une inflexion du client)

| Évolution possible | Où intervenir |
|---|---|
| Ajouter ou modifier une carte | `cartes.json` + une classe d'effet dans `cartes.effets` + son entrée dans `RegistreEffets` |
| Ajouter le draft | Nouvelle implémentation de `StrategieDistribution` |
| Changer une règle (limite de réserve, malus…) | `Regles`, `ReserveEnergie`, `PisteDesBonus` |
| Ajouter un robot | Nouvelle classe héritant de `AbstractRobot`, déclarée dans `MainPartie` / `MainSimulation` |
| Autre affichage | Nouvelle implémentation de `Afficheur` |
| Nouvelle statistique | `StatistiquesRobot`, `AfficheurResume` |

## 7. Hypothèses de modélisation

| N° | Hypothèse |
|---|---|
| H1 | Le stock d'énergie est **illimité** (les règles ne fixent pas de nombre de jetons). |
| H2 | Le « joueur le plus jeune » est un attribut `age` ; en simulation, le premier joueur de la partie est tiré avec la graine et la position de départ est alternée. |
| H3 | Si la pioche et la défausse sont vides, une action « piocher » ne donne rien. |
| H4 | Ordre à la fin de manche : effets de fin de manche, avancée de la roue, changements de saison puis d'année, test de fin de partie. |
| H5 | Les effets de fin de manche et de changement de saison se résolvent dans l'ordre du tour, en partant du premier joueur. |
| H6 | Égalité parfaite (points et cartes invoquées) : victoire partagée, comptée « ex æquo ». |
| H7 | Les énergies posées sur l'Amulette d'eau sont utilisables comme la réserve mais ne comptent pas pour les effets liés à la réserve (Coffret, Corne). |
| H8 | Le score est un entier ; les marqueurs de centaines de la piste des cristaux ne sont pas modélisés. |

## 8. Points à valider avec le client

| N° | Point | Détail |
|---|---|---|
| Q1 | **Balance d'Ishtar (carte 5)** | Le texte de la carte indique « défaussez 4 énergies identiques, 12 cristaux » ; le lexique indique « 3 énergies identiques, 9 cristaux ». Quelle version appliquer ? Hypothèse de travail : le lexique (3 énergies, 9 cristaux). |
| Q2 | **Faces des dés** | Les règles ne donnent pas le contenu exact des 20 dés. Peut-on le relever sur le matériel ou le fournir ? Sans cela, on construit un `des.json` plausible (2 faces à 1 point, 2 à 2, 2 à 3 par dé). |
| Q3 | **Coûts d'invocation et catégories des cartes** | Les coûts (symboles) et la couleur de fond (objet magique / familier) sont à relever sur les cartes. Dans l'extrait du PDF, seuls le nom, les points et le texte sont lisibles. |
| Q4 | **Draft** | Confirmer que les jeux pré-construits suffisent (pas de draft). |
| Q5 | **Cartes 8, 10 et 19** | Aucun jeu pré-construit ne les contient : elles ne peuvent venir que de la pioche. Confirmer que c'est voulu. |
| Q6 | **Ordre de fin de manche** | Confirmer l'hypothèse H4. |
| Q7 | **Statistiques de la simulation** | Quels indicateurs, au-delà des victoires et de la moyenne (médiane, écart-type, taux d'égalité, nombre de cartes invoquées) ? |
